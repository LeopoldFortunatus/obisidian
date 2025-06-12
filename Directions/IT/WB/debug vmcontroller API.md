1. Comment in internal/vmcontroller/vmcontroller.go: 
```
//c.nodeCtrl.Start()  
//c.vmCtrl.Start()  
//c.checkCtrl.Start()  
//c.dnsCtrl.Start()
```
2. setup local ip: `sudo ip addr add 172.29.77.2/24 dev lo`
3. copy /etc/cloud/native/ssl from MR to local
4. Create local vmcontroler and vmctl configs
5. start vmcontroller with params: `-c vmcontroller.yml`
6. run vmctl with params: `-c vmctl.yml list node`
7. debug with grpcurl: 
```
$grpcurl -v -d '{}'   -cacert /home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmctl/vmctl-ca.crt   -cert /home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmctl/vmctl.crt   -key /home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmctl/vmctl.key   -proto proto/vmctrl/v1/api.proto   -import-path proto/vmctrl/v1 -import-path  proto 172.29.77.2:2000 models.grpc.Service/ListNodes
```

DB connect:
```
go install github.com/pressly/goose/v3/cmd/goose@latest
goose -dir='./migrations' postgres "host=127.0.0.1 user=postgres password=postgres dbname=nativedb" up
migrate -path migrations -database "postgresql://postgres:postgres@127.0.0.1:5432/nativedb?sslmode=disable" up
```
configs examples:
vmcontroller:
```
---  
environment: mr                                                                   # type of environment where application is running (mr | stage | prod)  
cluster_cpu_vendor: "Intel"                                                                             # vendor of cpu worker node in cluster (Intel | AMD)  
api:                                                                                                    # Grpc user api with tls for GW, vmctl and other  
  listen: "0.0.0.0:2000"  
  tls:  
    enabled: True  
    key_file: "/home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmcontroller_api/vmcontroller_api.key"  
    cert_file: "/home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmcontroller_api/vmcontroller_api.crt"  
    ca_cert_file: "/home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmcontroller_api/vmcontroller_api-ca.crt"  
  
tokens:  
  vmagent: "JPp1KWvjVp5GsAJCupKYPXoORZwFZmD2q"  
  vmctrl:  
    - LTUy5WYpAuTdLXi9bRQ5KvCEJx2Q3Cyi  
    - i4spHRdnqHZMLZmlFGcCMnRWmTzRQgY6  
  
authorization:  
#  jwt_public_key_path: "/home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/opt-spicedb/ssl/jwt_provider.pem"  
  jwt_public_key_path: "/home/arykalin/go/src/git.wildberries.ru/cloud/local-configs/jwt/jwtRS256.key.pub"  
  spicedb_address: localhost:50052  
  spicedb_timeout: 30m  
  
log_level: "debug"                                                               # log level: debug | info | error  
log_level_rbac: "error"                                                                                 # log level: debug | info | error  
http_listen: "0.0.0.0:8088"                                                                             # http server for metrics, pprof and prometheus discovery: 0.0.0.0:8088 [/metrics|/debug/pprof|/discovery/nodes|vms]  
postgresql_dsn: "host=172.29.62.2 pool_max_conns=30 port=5432 user=nativeuser dbname=nativedb"                                                   # postgresql dsn used by vmcontroller  
task_worker: 100                                                           # number of tasks running in parallel by the controller  
  
ovn:  
  nbDatabases: ['172.29.62.2:6641', '172.29.62.3:6641', '172.29.62.4:6641']  
  sbDatabases: ['172.29.62.2:6642', '172.29.62.3:6642', '172.29.62.4:6642']  
  icNbDatabases: ['172.29.62.2:6645', '172.29.63.2:6645']  
  icSbDatabases: ['172.29.62.2:6646', '172.29.63.2:6646']  
  tls:  
    enabled: True  
    key_file: "/home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmcontroller_ovn/vmcontroller_ovn.key"  
    cert_file: "/home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmcontroller_ovn/vmcontroller_ovn.crt"  
    ca_cert_file: "/home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmcontroller_ovn/vmcontroller_ovn-cachain.crt"  
  
vault:  
  url: https://cloud-stage-1c1.dl.wb.ru:8200  
  token: hvs.CAESIBFnMfDLtZFQTroWy6zxV0w9yQNYuvZZRhO1myMcrYkWGh4KHGh2cy5pRW1lZjViNGhTOWdNUmpidHAyeFFrY3U  
  
s3:                                                                                                       # s3 server config with credential and service buckets:  
  access_key: "vmcontrollerq68p"  
  secret_access_key: "kmSCdDpEjCuxxs3DiWE24Sa0i995xSpccFLQ8uo9"  
  endpoint: "https://s3.cloud.devel:9000"  
  region: "us-east-1"  
  bucket_for_backups: "backups"                                            # bucketForBackups - for saving vm backups  
  bucket_for_user_images: "user-images"                                    # bucketForUserImages - for saving qcow2 user images created from a vm  
  
vmagent_tls: # tls config for vmagent client used when vmcontroller interacts with vmagents  
  enabled: True  
  key_file: "/home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmcontroller_vmagent/vmcontroller_vmagent.key"  
  cert_file: "/home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmcontroller_vmagent/vmcontroller_vmagent.crt"  
  ca_cert_file: "/home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmcontroller_vmagent/vmcontroller_vmagent-ca.crt"  
  
network:                                                                                                  # network config used vmcontroller  
  dns_servers: ["10.15.12.100","10.15.12.200"]                                                            # list of DNS servers for VMs DHCP options  
  availability_zone_id: 1                                                                       # number of availabilit zone: 0 - 255  
  zone_dns_postfix: "az1.n-cl-3348-spicedb-test-permissions-checks-2.mr.cloud.devel"                                                                      # dns postfix for VMs DNS names: cloud.devel  
  interconnect_network_cidr: "100.64.0.0/10"                                             # prefix for template ic networks: 100.64.0.0/10  
  external_network_cidr: "172.20.62.0/28"                                           # external network CIDR, one addr per node for node SNAT  
  system_network_cidr: "169.254.0.0/30"                                                                   # system network CIDR  
  node_network_cidr: "169.254.128.0/17"                                                                   # node network CIDR  
  netspace_network_cidr: "169.254.32.0/20"                                                                # netspace network CIDR  
  core_network_cidr: "169.254.1.0/30"                                                                     # core network CIDR  
  allowed_networks_cidr: ["192.168.0.0/16"]                                                               # network prefixes allowed to be used when creating vm  
  
dns:                                                                                                      # dns discovery service  
  xfr:                                                                                                    # XFR DNS server options  
    enabled: true                                                                                          # start server  
    listen: "0.0.0.0:852"                                                                               # ip address and port: 127.0.0.1:852  
    dns_clients:                                                                                          # list of ip addresses and ports for allowed DNS clients: 127.0.0.1:854  
      - "172.29.62.2:854"                                                                                   #  
      - "172.29.62.2:855"                                                                                   #  
  xot:                                                                                                    # XOT DNS server options  
    listen: "0.0.0.0:853"                                                                               # ip address and port: 127.0.0.1:853  
    dns_clients:                                                                                          # list of ip addresses and ports for allowed DNS clients: 127.0.0.1:854  
      - "172.29.62.2:854"                                                                               #  
      - "172.29.62.2:855"                                                                               #  
    tls:  
      enabled: True                                                             # start server  
      key_file: "/home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmcontroller_dns/vmcontroller_dns.key"                                          # path for key file: /home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmcontroller_dns/vmcontroller_dns.key  
      cert_file: "/home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmcontroller_dns/vmcontroller_dns.crt"                                         # path for cert file: /home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmcontroller_dns/vmcontroller_dns.crt  
      ca_cert_file: "/home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmcontroller_dns/vmcontroller_dns-ca.crt"                                   # path for ca cert file: /home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmcontroller_dns/vmcontroller_dns-ca.crt
```
vmctl:
```
log_level: info  
log_level_rbac: debug  
vmcontroller:  
  addresses:  
    172.29.77.2:  
      address: 172.29.77.2  
      port: 2000  
    #172.29.77.4:  
    #address: 172.29.77.4    #port: 2000  exec_timeout: 30  
  tls:  
    enabled: True  
    key_file: "/home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmctl/vmctl.key"  
    cert_file: "/home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmctl/vmctl.crt"  
    ca_cert_file: "/home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmctl/vmctl-ca.crt"  
  token: LTUy5WYpAuTdLXi9bRQ5KvCEJx2Q3Cyi
```


#### test with vmctl
remove additional instqances from  /etc/cloud/vmctl.yml (leanve only 172.30.12.2)
```
export JWT_TOKEN=...
vmctl -j $JWT_TOKEN create namespace -n spicedb-ns-1
OR for local dev
./target/vmctl -j $JWT_TOKEN -c ../local-configs/native/vmctl-new.yml create namespace -n test55
```

```
./target/vmctl -j $JWT_TOKEN -c ../local-configs/native/vmctl-new.yml create namespace -n ns1
./target/vmctl -j $JWT_TOKEN -c ../local-configs/native/vmctl-new.yml create netspace -n ns1 -p nts1
./target/vmctl -j $JWT_TOKEN -c ../local-configs/native/vmctl-new.yml create network -n ns1 -p nts1 -x 192.168.0.0/24 -w net1
```
create admin relation with zed
```
zed --insecure relationship touch namespace:1015efbc389f7c1bc10c3aafd21d803a76b239a40651d7d8597c949a9aa28af5 admin user:user567
```