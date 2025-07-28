#native #token-exchange #rbac #wildberries #IT
# Setup
## IP
```
sudo ip addr add 172.29.140.2/24 dev lo
```
# TokenExchange

## config
```
ADDRESS=172.29.140.2:1024
ENABLED_TLS_SERVICE_NAMES=synchronizer
EXTERNAL_PROVIDER_NAMES_AND_PUBLIC_KEYS_FILEPATH=exampleProvider:/home/arykalin/go/src/gitlab-private.wildberries.ru/cloud/rbac-interceptor/.my/jwtRS256.key.pub
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
eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJ1c2VyNTY3IiwiZXhwIjoxODkyMTk3Nzc2LCJwdWJsaWNfa2V5X2lkIjoiU0hBMjU2OndUTTB1VkZ5dHNWMHl3eUthaFFVcEhlT0tmS0dJS09LY2xXNU15d1d5ZGsiLCJpc19zZXJ2aWNlIjpmYWxzZX0.nSIPZnjtxG0Dcr1YwsgkUKaq-a6aKJz6YwGkq4iPNLQFAOQj3d5RSxEXwMJfGCMZ9RVusLeKAY0l64J36O8yZ5qPjx8QyCA68Zmm901yblBOCm9q2EZ07ZpSm16krqdSdKKk2cwOK5fgnf5r15N1WsNtM8qyHh7w-dlvlujdXIwZjnecVYGteAhV6hT-E33uG2OfL5TZKoQR9YUajNlMrazlKGeSAIoSV1e9wQMi1Dc2rg7AXUakvMyAuzS_c0cYNRjDbB3uG5hCib5eDK2Pf9qMuxs9Bmky8R-onsO7cygfg2kDtcNHnsYVhX3vN7mMlTuRRPm2zwkuLX2UhREl4PBdKrdWlTpnvodNKVam9ibSfb7C9PDDkYI4ubpP3bfRr2YjXsTmFq-AEHsZv2MkdCj7zH5gTSGpFY9KXTaOeqw9vyXh6EhsZbLgkfMi7BVQ349p-TBDWudj8r3D9vG-71jLxRCjdNKj1JsBnMPc-nSMMCb1lvd6QTvtXeSYs6UdzMwNw7E4O_B0T9Pv5Pb3hvK1umaTyVv9h8mYp8RDCduS8CRPH0EbEwjShUKNvvkyFi8ha-zmUqaQesuEOfX4Wg6z7SYOkk4Ecr6_z8bFCioN1-u0ZyftuuwAm_X2qHpr16QCpYg2cV74MCGNI0xGKaI0YnHxpWY5huLFvkhKEyQ
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
