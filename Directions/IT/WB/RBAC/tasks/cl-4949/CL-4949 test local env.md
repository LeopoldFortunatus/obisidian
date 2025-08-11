#native
#token-exchange #rbac #wildberries #IT
#wb-local-dev

# Setup
## IP
```
sudo ip addr add 172.29.140.2/24 dev lo
```
# TokenExchange

### External provdeirs
file:///home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/token-exchange/.configs
external1:.configs/external1_jwt_rs256.key.pub
external2:configs/external2_jwt_rs256.key.pub
## config
```
ADDRESS=172.29.140.2:1024
ENABLED_TLS_SERVICE_NAMES=synchronizer
EXTERNAL_PROVIDER_NAMES_AND_PUBLIC_KEYS_FILEPATH=external1:.configs/external1_jwt_rs256.key.pub
INTERNAL_PROVIDER_SERVICE_TOKEN_TTL=24h
POSTGRES_IS_ENABLED=false
TLS_CA_CERT_FILEPATH=/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/token-exchange/ssl/token_exchange-cachain.crt
TLS_ENABLED=true
TLS_SERVER_CERT_FILEPATH=/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/token-exchange/ssl/token_exchange.crt
TLS_SERVER_KEY_FILEPATH=/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/token-exchange/ssl/token_exchange.key
```

# Vmcontroller
file:///home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/cloud/vmcontroller.yml

```
api:                                                                                                    # Grpc user api with tls for GW, vmctl and other
  listen: "0.0.0.0:2000"
  tls:
    enabled: True
    key_file: "/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/etc/ssl/vmcontroller_api/vmcontroller_api.key"
    cert_file: "/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/etc/ssl/vmcontroller_api/vmcontroller_api.crt"
    ca_cert_file: "/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/etc/ssl/vmcontroller_api/vmcontroller_api-ca.crt"

tokens:
  vmagent: "JPp1KWvjVp5GsAJCupKYPXoORZwFZmD2q"
  vmctrl:
    - i4spHRdnqHZMLZmlFGcCMnRWmTzRQgY6
    - poG9wPTnKOBdVv9dczEZqkocgzccTHRF

authorization:
  spicedb:
    addresses: 172.29.140.2:50052
    client_timeout: 100s
    tls:
      enabled: True
      key_file: "/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/spicedb/ssl/spicedb_vmcontroller.key"
      cert_file: "/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/spicedb/ssl/spicedb_vmcontroller.crt"
      ca_cert_file: "/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/spicedb/ssl/spicedb-ca.crt"
  token_exchange:
    addresses: 172.29.140.2:1024
    client_timeout: 100s
    tls:
      enabled: True
      key_file: "/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/token-exchange/ssl/token_exchange_vmcontroller.key"
      cert_file: "/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/token-exchange/ssl/token_exchange_vmcontroller.crt"
      ca_cert_file: "/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/token-exchange/ssl/token_exchange-ca.crt"

```

# vmctl
-j 
```
eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE3ODYxMDgzMTQsImlzcyI6ImV4dGVybmFsMSIsInN1YiI6InVzZXIxIn0.2lwSOkdHLuTP3pXN8dnu3VYbdz5wzwkfWZFwllcy8pcPiRUaLVy-2UO90A5jMZnDz7iQHHVh2ko10H4aMILlv5YjwN58AiAR-FLHbsSbq_U0c4eaQWpQz6mFpnik1UMktjZiRf4vUZIGC9MV0n5HNbtRjbvl-LEks5PgW_DbNsqbCt4qPHVnmfyieWU4Cni0Ssi2wklMzC5tI-mNeOpTCPzNQAHeP602QGZjFVOtzVZjQodAju9fyqwz8CXtUCaC0xOpv4pMraUyuTfKQSZWiyPmoFRVy8w9mnIU3yxjR-nqWQu4Ym1DbC9YpuqyI8WZcqG8f1BI9pCb8VtZQRyX49Uz9FBDxCZBb40ucSA2P2wroqGOiQ3C8AKItM6WBXzGMd-KQ9BUMXwFSZ47QKArE4S0JBVqKDmotaVRzRr_J1GwdGrO7yzw5xrywJAAOlrHojgk1dTbbwEcH5RczLZH6StwlvXsUt4218yfdqRJMOnrsi_K-By-x7Z_EaBJCenk4KfPJY4x7uavZUK482Tp0NHJaaN6K9BzpoWCb3y-2N1snbUsKbZrH8pCBpLQggqQyPTEGOwJ8pH8YAencA2QFYjOv74hvvhp6O_pDdeZYO4hxwrHtj3S4mvaGnkaORFYxOeELJk5Jhx7-pehSgqXSgs5U9FN057Gupyw19s0oAU
```
 file:///home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/cloud/vmctl.yml
```
 log_level: info  
log_level_rbac: error  
vmcontroller:  
  addresses:  
    - 172.29.140.2:2000  
  exec_timeout: 30  
  tls:  
    enabled: True  
    key_file: "/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/etc/ssl/vmctl/vmctl.key"  
    cert_file: "/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/etc/ssl/vmctl/vmctl.crt"  
    ca_cert_file: "/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/etc/ssl/vmctl/vmctl-ca.crt"  
  token: i4spHRdnqHZMLZmlFGcCMnRWmTzRQgY6
```

To setup external token in vmctl:
cmd/vmctl/config/config.go:164
```
if len(Cfg.jwtToken) > 0 {  
    ctx = metadata.NewOutgoingContext(ctx, metadata.New(map[string]string{  
       jwtexchange.ExternalJWTKey: Cfg.jwtToken,  
    }))  
}
```
# RBAC-Server
```
SPICEDB_TLS_CA_CERT_FILEPATH=/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/spicedb/ssl/spicedb-ca.crt
SPICEDB_TLS_CLIENT_CERT_FILEPATH=/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/spicedb/ssl/spicedb_client.crt
SPICEDB_TLS_CLIENT_KEY_FILEPATH=/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/spicedb/ssl/spicedb_client.key
SPICEDB_TLS_SERVER_CERT_FILEPATH=/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/spicedb/ssl/spicedb.crt
SPICEDB_TLS_SERVER_KEY_FILEPATH=/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/spicedb/ssl/spicedb.key
SPICEDB_TOKEN_EXCHANGE_TLS_CA_CERT_FILEPATH=/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/token-exchange/ssl/token_exchange-cachain.crt
SPICEDB_TOKEN_EXCHANGE_TLS_CLIENT_CERT_FILEPATH=/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/token-exchange/ssl/token_exchange_spicedb.crt
SPICEDB_TOKEN_EXCHANGE_TLS_CLIENT_KEY_FILEPATH=/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/local-configs/cl-4664-new2/token-exchange/ssl/token_exchange_spicedb.key
```
