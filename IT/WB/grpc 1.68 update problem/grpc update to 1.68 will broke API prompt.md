### Проблема обновления gRPC

#### Суть проблемы
Обновление API в `cloud-native` до `google.golang.org/grpc@v1.68.1` с `google.golang.org/grpc@v1.66.3` приводит к ошибке при вызове API со стороны клиентоа:
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

#### Обновление gRPC
TLS сломается:
```
 go get google.golang.org/grpc@v1.68.1 && go mod vendor
```
TLS работает:
```
go get google.golang.org/grpc@v1.66.3 && go mod vendor
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

#### Клиентские настройки tlsCfg:
```go
const (  
    CaCertFile = "/home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmctl/vmctl-ca.crt"  
    CertFile   = "/home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmctl/vmctl.crt"  
    KeyFile    = "/home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmctl/vmctl.key"  
)  
  
func New(address string, tlsCfg *tls.Config) (api.ServiceClient, *grpc.ClientConn, error) {  
    serviceConfig := `{  
    "loadBalancingPolicy": "round_robin",    "healthCheckConfig": {       "serviceName": ""    }}`  
  
    ///*  
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
    //*/
```

####  файлы
- `pkg/pb/ctrlpb/v1/client/client.go` - клиентские настройки
- `cmd/vmctl/config/config.go` - загрузка TLS конфигурации для vmctl
- `internal/vmcontroller/api/config.go` - конфигурация API с TLS listener
- `pkg/cfg/tls/config.go` - настройки TLS
- `pkg/grpc-multi-resolver/multi.go` - multi resolver

#### Что уже было протестировано
1. Конфигурация файлов точно работает, использование grpcurl приводит к нужным результатам. Так мы точно знаем, что проблема на стороне клиента, а не сервера, и не в самих сертификатах, поскольку те же сертификаты и пути к ним использовались в grpcurl.
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


Ссылка на тестовый МР - https://gitlab-private.wildberries.ru/cloud/cloud-native/-/merge_requests/1370