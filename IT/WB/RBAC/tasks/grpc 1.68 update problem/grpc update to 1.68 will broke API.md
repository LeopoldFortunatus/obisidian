### GRPC upgrade problem
### Суть проблемы
Обновление API в cloud-native до google.golang.org/grpc@v1.68.1 с google.golang.org/grpc@v1.66.3 приводит к тому что любой вызов к API со стороны клиента приводит к ошибке:
```
[FATAL] failed to execute: "rpc error: code = Unavailable desc = last connection error: connection error: desc = \"error reading server preface: EOF\""
```
Проблема заключается в специфической конфигурации клиента, нам необходимо использовать конфигурцию:
```
grpc.WithContextDialer(func(ctx context.Context, s string) (net.Conn, error) {  
    return tls.Dial("tcp", s, tlsCfg)  
})
```
вместо стандартной:
```
[]grpc.DialOption{  
    grpc.WithTransportCredentials(credentials.NewTLS(tlsCfg)),  
    ....
}
```


работающая конфигурация:
```
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
конфигурация которая приводит к ошибке:
```
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

#### grpc update
API will be broken:
```
 go get google.golang.org/grpc@v1.68.1 && go mod vendor
```
API will be fixed:
```
go get google.golang.org/grpc@v1.66.3 && go mod vendor
```

Error:
```
[FATAL] failed to execute: "rpc error: code = Unavailable desc = last connection error: connection error: desc = \"error reading server preface: EOF\""
```

debug with grpcurl:
```
$grpcurl -v -d '{}'   -cacert /home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmctl/vmctl-ca.crt   -cert /home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmctl/vmctl.crt   -key /home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmctl/vmctl.key   -proto proto/vmctrl/v1/api.proto   -import-path proto/vmctrl/v1 -import-path  proto 172.29.77.2:2000 models.grpc.Service/ListNodes

```

use DialWithDialer instead of tls.Dial:
```
connOpts = append(connOpts, grpc.WithContextDialer(func(ctx context.Context, addr string) (net.Conn, error) {
    d := &net.Dialer{
        Timeout:   5 * time.Second,
        KeepAlive: 5 * time.Second,
    }
    return tls.DialWithDialer(d, "tcp", addr, tlsCfg)
}))
```
#### Files involved
pkg/pb/ctrlpb/v1/client/client.go - client settings
cmd/vmctl/config/config.go - load TLS config for vmctl
internal/vmcontroller/api/config.go - API configuration with TLS listener
pkg/cfg/tls/config.go - TLS settings
pkg/grpc-multi-resolver/multi.go - multi resolver

get server name func:
```
func getServerNameFromkCert(cert *tls.Certificate) string {  
    if len(cert.Certificate) == 0 {  
       return ""  
    }  
    c, err := x509.ParseCertificate(cert.Certificate[0])  
    if err != nil {  
       return ""  
    }  
    return c.Subject.CommonName  
}
```

