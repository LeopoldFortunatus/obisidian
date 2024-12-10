1. Comment: 
```
//c.nodeCtrl.Start()  
//c.vmCtrl.Start()  
//c.checkCtrl.Start()  
//c.dnsCtrl.Start()
```
2. setup local ip: `sudo ip addr add 172.29.77.2/24 dev lo`
3. run vmctl: `-c ../local-configs/native/vmctl-new.yml list node`
4. debug with grpcurl: 
```
$grpcurl -v -d '{}'   -cacert /home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmctl/vmctl-ca.crt   -cert /home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmctl/vmctl.crt   -key /home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmctl/vmctl.key   -proto proto/vmctrl/v1/api.proto   -import-path proto/vmctrl/v1 -import-path  proto 172.29.77.2:2000 models.grpc.Service/ListNodes
```


### GRPC upgrade problem
grpc.WithTransportCredentials(insecure.NewCredentials()),

work:
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
dies not work:
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