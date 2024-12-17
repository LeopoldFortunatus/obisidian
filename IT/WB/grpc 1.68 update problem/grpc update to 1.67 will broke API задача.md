### Проблема обновления gRPC с 1.66.3 до 1.67.0

#### Суть проблемы
Обновление API в `cloud-native` до `google.golang.org/grpc@v1.67.0` с `google.golang.org/grpc@v1.66.3` приводит к ошибке при вызове API со стороны клиентоа:
```
[FATAL] failed to execute: "rpc error: code = Unavailable desc = last connection error: connection error: desc = \"error reading server preface: EOF\""
```
Проблема заключается в специфической конфигурации клиентов (pkg/pb/ctrlpb/v1/client/client.go и pkg/pb/ctrlpb/v2/client/client.go), нам необходимо использовать конфигурацию:
```go
grpc.WithContextDialer(func(ctx context.Context, s string) (net.Conn, error) {  
    return tls.Dial("tcp", s, tlsCfg)  
})
```
вместо стандартной:
```go
[]grpc.DialOption{  
    grpc.WithTransportCredentials(credentials.NewTLS(tlsCfg)),  
    ....
}
```


В целях тестирования конфигурация tls была приведена к простому виду чтобы сузить место проблемы.
##### Рабочая конфигурация:
```go
// Setup TLS config  
tlsCfg = &tls.Config{  
    Certificates: []tls.Certificate{cert},  
    RootCAs:      caCertPool,  
}  
  
connOpts := []grpc.DialOption{  
    grpc.WithTransportCredentials(credentials.NewTLS(tlsCfg)),  
    grpc.WithDefaultServiceConfig(serviceConfig),  
    grpc.WithConnectParams(grpc.ConnectParams{  
       Backoff: backoff.Config{  
          BaseDelay:  1.0 * time.Second,  
          Multiplier: 1.6,  
          Jitter:     0.2,  
          MaxDelay:   20 * time.Second,  
       },  
       MinConnectTimeout: 5 * time.Second,  
    }),  
    grpc.WithUnaryInterceptor(jwtauth.NewClientInterceptor(true)),  
}  
  
//if tlsCfg != nil {  
//  // https://github.com/grpc/grpc-go/issues/1627  
//  connOpts = append(connOpts, grpc.WithContextDialer(func(ctx context.Context, s string) (net.Conn, error) {  
//     return tls.Dial("tcp", s, tlsCfg)  
//  }))  
//}  
  
conn, err := grpc.NewClient(address, connOpts...)  
if err != nil {  
    return nil, nil, fmt.Errorf("failed to create grpc connection: %w", err)  
}  
return api.NewServiceClient(conn), conn, nil
```

##### Конфигурация, приводящая к ошибке:
```go
// Setup TLS config  
tlsCfg = &tls.Config{  
    Certificates: []tls.Certificate{cert},  
    RootCAs:      caCertPool,  
}  
  
connOpts := []grpc.DialOption{  
    grpc.WithTransportCredentials(insecure.NewCredentials()),  
    grpc.WithDefaultServiceConfig(serviceConfig),  
    grpc.WithConnectParams(grpc.ConnectParams{  
       Backoff: backoff.Config{  
          BaseDelay:  1.0 * time.Second,  
          Multiplier: 1.6,  
          Jitter:     0.2,  
          MaxDelay:   20 * time.Second,  
       },  
       MinConnectTimeout: 5 * time.Second,  
    }),  
    grpc.WithUnaryInterceptor(jwtauth.NewClientInterceptor(true)),  
}  
  
if tlsCfg != nil {  
    // https://github.com/grpc/grpc-go/issues/1627  
    connOpts = append(connOpts, grpc.WithContextDialer(func(ctx context.Context, s string) (net.Conn, error) {  
       return tls.Dial("tcp", s, tlsCfg)  
    }))  
}  
  
conn, err := grpc.NewClient(address, connOpts...)  
if err != nil {  
    return nil, nil, fmt.Errorf("failed to create grpc connection: %w", err)  
}  
return api.NewServiceClient(conn), conn, nil
```

#### Упрощенные клиентские настройки tlsCfg:
```go
const (  
    CaCertFile = "/etc/ssl/vmctl/vmctl-ca.crt"  
    CertFile   = "/etc/native/ssl/vmctl/vmctl.crt"  
    KeyFile    = "/etc/ssl/vmctl/vmctl.key"  
)  
  
func New(address string, tlsCfg *tls.Config) (api.ServiceClient, *grpc.ClientConn, error) {  
    serviceConfig := `{  
    "loadBalancingPolicy": "round_robin",    "healthCheckConfig": {       "serviceName": ""    }}`  
  
    
       // simple tlsCfg for debug       tlsCfg.GetConfigForClient = nil  
       tlsCfg.GetClientCertificate = nil  
       tlsCfg.GetCertificate = nil  
  
       // Load client cert  
       cert, err := tls.LoadX509KeyPair(CertFile, KeyFile)  
       if err != nil {  
          return nil, nil, fmt.Errorf("failed to load client certificate: %w", err)  
       }  
  
       // Load CA cert  
       caCert, err := ioutil.ReadFile(CaCertFile)  
       if err != nil {  
          return nil, nil, fmt.Errorf("failed to read CA certificate: %w", err)  
       }  
       caCertPool := x509.NewCertPool()  
       if !caCertPool.AppendCertsFromPEM(caCert) {  
          return nil, nil, fmt.Errorf("failed to append CA certificate")  
       }  
  
       // Setup TLS config  
       tlsCfg = &tls.Config{  
          Certificates: []tls.Certificate{cert},  
          RootCAs:      caCertPool,  
       }  
    
```

####  файлы
- `pkg/pb/ctrlpb/v1/client/client.go` - клиентские настройки
- `cmd/vmctl/config/config.go` - загрузка TLS конфигурации для vmctl
- `internal/vmcontroller/api/config.go` - конфигурация API с TLS listener
- `pkg/cfg/tls/config.go` - настройки TLS
- `pkg/grpc-multi-resolver/multi.go` - multi resolver

#### Что уже было протестировано
1. Вызов через grpcurl работает. Так мы точно знаем, что проблема на стороне клиента, а не сервера, и не в самих сертификатах, поскольку те же сертификаты и пути к ним использовались в grpcurl.
2. Если использовать простой tls.Config как представлен в примере:
   ```go
         tlsCfg = &tls.Config{  
          Certificates: []tls.Certificate{cert},  
          RootCAs:      caCertPool,  
       } 
    ```
то будет работать с использованием `grpc.WithTransportCredentials(credentials.NewTLS(tlsCfg))`. Однако мы используем более сложную конфигруацию tls представленную в методе LoadTlsConfig. И так как мы используем свой собственный multi resolver (pkg/grpc-multi-resolver/multi.go) который позволяет прописывать несколько ip адресов в конфигурации вместо использования round robin в DNS то получается ошибка:
```
\"transport: authentication handshake failed: tls: failed to verify certificate: x509: certificate is valid for c1-az1, c1-az1.az1.n-cl-3348-upgrade-grpc-test.mr.cloud.devel, az1.n-cl-3348-upgrade-grpc-test.mr.cloud.devel, not 172.29.60.4:2000,172.29.60.2:2000,172.29.60.3:2000\"
```


#### Заметки для тестирования

Ссылка на тестовый МР - https://gitlab-private.wildberries.ru/cloud/cloud-native/-/merge_requests/1370
##### Обновление grpc:
  TLS сломается:
```bash
 go get google.golang.org/grpc@v1.67.0 && go mod vendor
```
TLS работает:
```bash
go get google.golang.org/grpc@v1.66.3 && go mod vendor
```
##### тест через grpcurl:
  ```
  $grpcurl -v -d '{}'   -cacert /etc/ssl/vmctl/vmctl-ca.crt   -cert /etc/ssl/vmctl/vmctl.crt   -key /etc/ssl/vmctl/vmctl.key   -proto proto/vmctrl/v1/api.proto   -import-path proto/vmctrl/v1 -import-path  proto 172.29.77.2:2000 models.grpc.Service/ListNodes
```
##### настройка vmcontroller для локального тестрования. patch
Для упрощения тестирования можно отключить все сервисы в vmcontroler кроме API и запускать его локально с конфигурацией из MR. Также надо поставить заглушку на какой-нибудь тестируемый метод, например listNode. Вот пример патча:
```
Index: pkg/pb/ctrlpb/v1/client/client.go
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/pkg/pb/ctrlpb/v1/client/client.go b/pkg/pb/ctrlpb/v1/client/client.go
--- a/pkg/pb/ctrlpb/v1/client/client.go	(revision f930bacdb158f7c0ed8490e3de3dec6dba3db043)
+++ b/pkg/pb/ctrlpb/v1/client/client.go	(date 1733909127847)
@@ -1,9 +1,11 @@
 package client
 
 import (
+	"context"
 	"crypto/tls"
 	"fmt"
-	"google.golang.org/grpc/credentials"
+	"google.golang.org/grpc/credentials/insecure"
+	"net"
 	"time"
 
 	"gitlab-private.wildberries.ru/cloud/rbac-interceptor/pkg/interceptors/jwtauth"
@@ -24,7 +26,7 @@
 }`
 
 	connOpts := []grpc.DialOption{
-		grpc.WithTransportCredentials(credentials.NewTLS(tlsCfg)),
+		grpc.WithTransportCredentials(insecure.NewCredentials()),
 		grpc.WithDefaultServiceConfig(serviceConfig),
 		grpc.WithConnectParams(grpc.ConnectParams{
 			Backoff: backoff.Config{
@@ -38,12 +40,12 @@
 		grpc.WithUnaryInterceptor(jwtauth.NewClientInterceptor(true)),
 	}
 
-	//if tlsCfg != nil {
-	//	// https://github.com/grpc/grpc-go/issues/1627
-	//	connOpts = append(connOpts, grpc.WithContextDialer(func(ctx context.Context, s string) (net.Conn, error) {
-	//		return tls.Dial("tcp", s, tlsCfg)
-	//	}))
-	//}
+	if tlsCfg != nil {
+		// https://github.com/grpc/grpc-go/issues/1627
+		connOpts = append(connOpts, grpc.WithContextDialer(func(ctx context.Context, s string) (net.Conn, error) {
+			return tls.Dial("tcp", s, tlsCfg)
+		}))
+	}
 
 	conn, err := grpc.NewClient(address, connOpts...)
 	if err != nil {
Index: internal/vmcontroller/vmcontroller.go
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/internal/vmcontroller/vmcontroller.go b/internal/vmcontroller/vmcontroller.go
--- a/internal/vmcontroller/vmcontroller.go	(revision f930bacdb158f7c0ed8490e3de3dec6dba3db043)
+++ b/internal/vmcontroller/vmcontroller.go	(date 1733909157714)
@@ -63,10 +63,11 @@
 	}(c.api)
 	log.Info().Msg("service web server started")
 
-	c.nodeCtrl.Start()
-	c.vmCtrl.Start()
-	c.checkCtrl.Start()
-	c.dnsCtrl.Start()
+	// disable for debug
+	//c.nodeCtrl.Start()
+	//c.vmCtrl.Start()
+	//c.checkCtrl.Start()
+	//c.dnsCtrl.Start()
 	c.discoveryCtrl.Start()
 
 	go func(api *api.Api) {
Index: pkg/pb/ctrlpb/v1/api/api_grpc.pb.go
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/pkg/pb/ctrlpb/v1/api/api_grpc.pb.go b/pkg/pb/ctrlpb/v1/api/api_grpc.pb.go
--- a/pkg/pb/ctrlpb/v1/api/api_grpc.pb.go	(revision f930bacdb158f7c0ed8490e3de3dec6dba3db043)
+++ b/pkg/pb/ctrlpb/v1/api/api_grpc.pb.go	(date 1733909157769)
@@ -8,6 +8,7 @@
 
 import (
 	context "context"
+	"github.com/rs/zerolog/log"
 	grpc "google.golang.org/grpc"
 	codes "google.golang.org/grpc/codes"
 	status "google.golang.org/grpc/status"
@@ -2203,6 +2204,8 @@
 }
 
 func _Service_ListNodes_Handler(srv interface{}, ctx context.Context, dec func(interface{}) error, interceptor grpc.UnaryServerInterceptor) (interface{}, error) {
+	log.Debug().Msg("ListNodes was called")
+	return nil, status.Error(codes.Unimplemented, "method ListNodes not implemented")
 	in := new(ListNodesRequest)
 	if err := dec(in); err != nil {
 		return nil, err

```
Чтобы не генерить свои конфиги и брать конфиги из MR надо ещё прописать адрес на локалхосте:
```
sudo ip addr add 172.29.77.2/24 dev lo
```

### Решение
1.67 требует alpn {h2}
Установка ALPN `[]string{"h2"}` в `tls.Config:NextProtos`

```
if tlsCfg, err = Cfg.Vmcontroller.Tls.LoadTlsConfig([]string{"h2"}); err != nil {  
    return fmt.Errorf("failed to load tls config: %w", err)  
}
```