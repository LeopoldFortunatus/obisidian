# Environement
```
docker ps -q | while read container; do
  echo "## Container: $(docker inspect --format '{{.Name}}' $container | sed 's/\///')"
  docker inspect --format '{{range .Config.Env}}{{println .}}{{end}}' $container
  echo "----------------------------------------"
done

```

## Container: pgaas
PGSQLDSN=postgresql://pgaasdb:pgaasdb_passw0rd@pgaas_postgresql:5432/pgaasdb?pool_max_conns=10
MIGRATION_CONN_STRING=host=pgaas_postgresql dbname=pgaasdb user=pgaasdb password=pgaasdb_passw0rd port=5432 
MIGRATION_ENV=dev
WBLOGGER_LEVEL=DEBUG
PGAAS_VAULT_ADDR=https://cloud-stage-1c1.dl.wb.ru:8200
PGAAS_VAULT_TOKEN=hvs.CAESILBIWQiwb4jXOwr7EaDNALF4nJ2hSYzKIxOcS1Ci01K0Gh4KHGh2cy5CY3hCY3YzbG1BMWhYbjlXa0RNMXljS2E
PGAAS_MOUNT_PATH_KV=pgaas/
PGAAS_MOUNT_PATH_TRANSIT=transit-pgaas/
INFRA_MOUNT_PATH_KV=infrasecret/
PGAAS_MOUNT_PATH_SSH=ssh_pgaas/
PGAAS_AUTH_USER=PGAAS_CLUSTER_ADMIN_ACC
PGAAS_AUTH_PASSWORD=ssZfFw6YhCBi5Ygnx1N
PGAAS_BACKUPS_BUCKET=pgaas-backups
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/app

----------------------------------------
## Container: pgaas_postgresql
POSTGRES_DB=pgaasdb
POSTGRES_USER=pgaasdb
POSTGRES_PASSWORD=pgaasdb_passw0rd
POSTGRES_HOST_AUTH_METHOD=trust
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/postgresql/15/bin
GOSU_VERSION=1.16
LANG=en_US.utf8
PG_MAJOR=15
PG_VERSION=15.1-1.pgdg110+1
PGDATA=/var/lib/postgresql/data

----------------------------------------
## Container: front_quota_1
QUOTA_IMAGE=harbor.wildberries.ru/cloud/gateway-services/quota
QUOTA_TAG=cl-4949-gw-rbac-1
MIGRATION_CONN_STRING=dbname=quotadb user=quotadb host=quotadb port=5432 sslmode=verify-full sslrootcert=/etc/quota/quotadb-client-cachain.crt sslcert=/etc/quota/quotadb-client.crt sslkey=/etc/quota/quotadb-client.key
CONFIG_PATH=/etc/quota/quota-config.yml
DEBUG=false
GRPC_GO_LOG_VERBOSITY_LEVEL=1
GRPC_GO_LOG_SEVERITY_LEVEL=info
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

----------------------------------------
## Container: front_teleport_1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

----------------------------------------
## Container: front_metrics_1
METRICS_IMAGE=harbor.wildberries.ru/cloud/gateway-services/openresty
METRICS_TAG=cl-4949-gw-rbac-1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/local/openresty/luajit/bin:/usr/local/openresty/nginx/sbin:/usr/local/openresty/bin

----------------------------------------
## Container: front_proxy_1
PROXY_TAG=cl-4949-gw-rbac-1
PROXY_IMAGE=harbor.wildberries.ru/cloud/gateway-services/openresty
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/local/openresty/luajit/bin:/usr/local/openresty/nginx/sbin:/usr/local/openresty/bin

----------------------------------------
## Container: front_clickhouse-query_1
CLICKHOUSE_USER=billing_user
CLICKHOUSE_PASSWORD=hLC8sX6aP4IxWZlluuMfukQW7moRTFyi
CLICKHOUSE_QUERY_INTERVAL_SEC=43200
CLICKHOUSE_HOST=clickhouse.cloud.devel
CLICKHOUSE_DB=billing
CLICKHOUSE_PORT=9001
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
LANG=en_US.UTF-8
LANGUAGE=en_US:en
LC_ALL=en_US.UTF-8

----------------------------------------
## Container: front_gwadmin_1
CONFIG_PATH=/etc/gwadmin/gwadmin-config.yml
GWADMIN_IMAGE=harbor.wildberries.ru/cloud/gateway-services/gwadmin
GWADMIN_TAG=cl-4949-gw-rbac-1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

----------------------------------------
## Container: front_gateway_1
GRPC_GO_LOG_VERBOSITY_LEVEL=1
GATEWAY_IMAGE=harbor.wildberries.ru/cloud/gateway-services/gateway
ACCESS_CONTROL_ALLOW_HEADERS=X-Requested-With
DEBUG=false
GRPC_GO_LOG_SEVERITY_LEVEL=error
MIGRATION_CONN_STRING=dbname=gatewaydb user=gatewaydb host=gatewaydb port=5432 sslmode=verify-full sslrootcert=/etc/gateway/gatewaydb-client-cachain.crt sslcert=/etc/gateway/gatewaydb-client.crt sslkey=/etc/gateway/gatewaydb-client.key
ACCESS_CONTROL_ALLOW_ORIGIN=*
CONFIG_PATH=/etc/gateway/gateway-config.yml
ACCESS_CONTROL_ALLOW_CREDENTIALS=true
OS_META_CONFIG_PATH=/etc/gateway/os_meta_config
GATEWAY_TAG=cl-4949-gw-rbac-1
OS_CONFIG_PATH=/etc/gateway/os_config
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

----------------------------------------
## Container: front_notifications_1
NOTIFICATIONS_IMAGE=harbor.wildberries.ru/cloud/gateway-services/notifications
NOTIFICATIONS_TAG=cl-4949-gw-rbac-1
MIGRATION_CONN_STRING=dbname=notificationsdb user=notificationsdb host=notificationsdb port=5432 sslmode=verify-full sslrootcert=/etc/notifications/notificationsdb-client-cachain.crt sslcert=/etc/notifications/notificationsdb-client.crt sslkey=/etc/notifications/notificationsdb-client.key
CONFIG_PATH=/etc/notifications/notifications-config.yml
DEBUG=false
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

----------------------------------------
## Container: front_billing_1
CONFIG_PATH=/etc/billing/billing-config.yml
PRICE_CONFIG_PATH=/etc/billing/price.yml
Environment=DEBUG=false
BILLING_IMAGE=harbor.wildberries.ru/cloud/gateway-services/billing
BILLING_TAG=cl-4949-gw-rbac-1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

----------------------------------------
## Container: front_image_1
MIGRATION_CONN_STRING=dbname=imagedb user=imagedb host=imagedb port=5432 sslmode=verify-full sslrootcert=/etc/image/imagedb-client-cachain.crt sslcert=/etc/image/imagedb-client.crt sslkey=/etc/image/imagedb-client.key
CONFIG_PATH=/etc/image/image-config.yml
DEBUG=false
GRPC_GO_LOG_VERBOSITY_LEVEL=1
GRPC_GO_LOG_SEVERITY_LEVEL=info
IMAGESERVICE_IMAGE=harbor.wildberries.ru/cloud/gateway-services/image
IMAGESERVICE_TAG=cl-4949-gw-rbac-1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

----------------------------------------
## Container: front_ipam_1
IPAM_IMAGE=harbor.wildberries.ru/cloud/gateway-services/ipam
IPAM_TAG=cl-4949-gw-rbac-1
MIGRATION_CONN_STRING=dbname=ipamdb user=ipamdb host=ipamdb port=5432 sslmode=verify-full sslrootcert=/etc/ipam/ipamdb-client-cachain.crt sslcert=/etc/ipam/ipamdb-client.crt sslkey=/etc/ipam/ipamdb-client.key
CONFIG_PATH=/etc/ipam/ipam-config.yml
DEBUG=false
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

----------------------------------------
## Container: front_userserver_1
DEBUG=false
USERSERVER_IMAGE=harbor.wildberries.ru/cloud/gateway-services/userserver
USERSERVER_TAG=cl-4949-gw-rbac-1
MIGRATION_CONN_STRING=dbname=userdb host=userdb port=5432 user=userdb sslmode=verify-full sslrootcert=/etc/userserver/userdb-client-cachain.crt sslcert=/etc/userserver/userdb-client.crt sslkey=/etc/userserver/userdb-client.key
CONFIG_PATH=/etc/userserver/userserver-config.yml
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

----------------------------------------
## Container: front_swagger_1
SWAGGER_IMAGE=harbor.wildberries.ru/cloud/gateway-services/swagger
SWAGGER_TAG=cl-4949-gw-rbac-1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
NGINX_VERSION=1.23.2
NJS_VERSION=0.7.7
PKG_RELEASE=1
API_KEY=**None**
SWAGGER_JSON=/app/swagger.json
PORT=8080
BASE_URL=
SWAGGER_JSON_URL=

----------------------------------------
## Container: front_frontend_1
FRONTEND_TAG=cl-4949-gw-rbac-1
FRONTEND_IMAGE=harbor.wildberries.ru/cloud/gateway-services/frontend
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
NGINX_VERSION=1.22.1
NJS_VERSION=0.7.11
PKG_RELEASE=1~bullseye

----------------------------------------
## Container: front_notificationsdb_1
POSTGRES_USER=notificationsdb
POSTGRES_PASSWORD=notificationsdb
POSTGRES_HOST_AUTH_METHOD=trust
POSTGRES_DB=notificationsdb
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/postgresql/15/bin
GOSU_VERSION=1.16
LANG=en_US.utf8
PG_MAJOR=15
PG_VERSION=15.1-1.pgdg110+1
PGDATA=/var/lib/postgresql/data

----------------------------------------
## Container: front_quotadb_1
POSTGRES_PASSWORD=quotadb
POSTGRES_HOST_AUTH_METHOD=trust
POSTGRES_DB=quotadb
POSTGRES_USER=quotadb
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/postgresql/15/bin
GOSU_VERSION=1.16
LANG=en_US.utf8
PG_MAJOR=15
PG_VERSION=15.1-1.pgdg110+1
PGDATA=/var/lib/postgresql/data

----------------------------------------
## Container: front_userdb_1
POSTGRES_USER=userdb
POSTGRES_PASSWORD=userdb
POSTGRES_HOST_AUTH_METHOD=trust
POSTGRES_DB=userdb
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/postgresql/15/bin
GOSU_VERSION=1.16
LANG=en_US.utf8
PG_MAJOR=15
PG_VERSION=15.1-1.pgdg110+1
PGDATA=/var/lib/postgresql/data

----------------------------------------
## Container: front_imagedb_1
POSTGRES_PASSWORD=imagedb
POSTGRES_HOST_AUTH_METHOD=trust
POSTGRES_DB=imagedb
POSTGRES_USER=imagedb
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/postgresql/15/bin
GOSU_VERSION=1.16
LANG=en_US.utf8
PG_MAJOR=15
PG_VERSION=15.1-1.pgdg110+1
PGDATA=/var/lib/postgresql/data

----------------------------------------
## Container: front_ipamdb_1
POSTGRES_DB=ipamdb
POSTGRES_USER=ipamdb
POSTGRES_PASSWORD=ipamdb
POSTGRES_HOST_AUTH_METHOD=trust
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/postgresql/15/bin
GOSU_VERSION=1.16
LANG=en_US.utf8
PG_MAJOR=15
PG_VERSION=15.1-1.pgdg110+1
PGDATA=/var/lib/postgresql/data

----------------------------------------
## Container: front_gatewaydb_1
POSTGRES_DB=gatewaydb
POSTGRES_USER=gatewaydb
POSTGRES_PASSWORD=userdb
POSTGRES_HOST_AUTH_METHOD=trust
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/postgresql/15/bin
GOSU_VERSION=1.16
LANG=en_US.utf8
PG_MAJOR=15
PG_VERSION=15.1-1.pgdg110+1
PGDATA=/var/lib/postgresql/data

----------------------------------------
## Container: maintenance
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
NGINX_VERSION=1.22.1
NJS_VERSION=0.7.11
PKG_RELEASE=1~bullseye

----------------------------------------
## Container: front-docker_exporter-1
PATH=/usr/local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
LANG=C.UTF-8
GPG_KEY=A035C8C19219BA821ECEA86B64E628F8D684696D
PYTHON_VERSION=3.11.10
PYTHON_SHA256=07a4356e912900e61a15cb0949a06c4a05012e213ecd6b4e84d0f67aabbee372

----------------------------------------
## Container: spicedb
SPICEDB_TOKEN_EXCHANGE_ADDRESSES=172.29.147.2:1024
SPICEDB_TOKEN_EXCHANGE_CLIENT_TIMEOUT=15s
SPICEDB_TOKEN_EXCHANGE_ENABLED_TLS=true
TLS_ENABLED=true
SPICEDB_TLS_ENABLED=true
SPICEDB_TLS_CA_CERT_FILEPATH=/etc/ssl/spicedb-ca.crt
SPICEDB_TLS_SERVER_CERT_FILEPATH=/etc/ssl/spicedb.crt
SPICEDB_TLS_SERVER_KEY_FILEPATH=/etc/ssl/spicedb.key
SPICEDB_TLS_CLIENT_CERT_FILEPATH=/etc/ssl/spicedb_client.crt
SPICEDB_TLS_CLIENT_KEY_FILEPATH=/etc/ssl/spicedb_client.key
SPICEDB_TOKEN_EXCHANGE_TLS_CA_CERT_FILEPATH=/etc/token_exchange-cachain.crt
SPICEDB_TOKEN_EXCHANGE_TLS_CLIENT_CERT_FILEPATH=/etc/token_exchange_spicedb.crt
SPICEDB_TOKEN_EXCHANGE_TLS_CLIENT_KEY_FILEPATH=/etc/token_exchange_spicedb.key
SPICEDB_TELEMETRY_ADDRESS=tempo:4317
SPICEDB_TELEMETRY_METER_INTERVAL=60s
SPICEDB_TELEMETRY_TRACE_TIMEOUT=5s
SPICEDB_TELEMETRY_IS_USE_PROMETHEUS_METER=true
SPICEDB_METRICS_ADDRESS=spicedb:50054
SPICEDB_METRICS_READ_TIMEOUT=30s
SPICEDB_METRICS_WRITE_TIMEOUT=60s
SPICEDB_METRICS_IDLE_TIMEOUT=60s
SPICEDB_METRICS_CRT_FILEPATH=
SPICEDB_LOG_LEVEL=debug
SPICEDB_LOG_JSON_OUTPUT=true
SPICEDB_CONNECTION=postgres://spicedb_db@172.29.147.2:5445/spicedb_db
SPICEDB_ADDRESS=spicedb:50052
SPICEDB_ADMIN_ADDRESS=spicedb:50053
SPICEDB_CLIENT_TIMEOUT=300ms
SPICEDB_READ_RESOURCE_TIMEOUT=5s
SPICEDB_SCOPE_CONFIG_FILE=./config/scope/scope_config.yaml
SPICEDB_SERVICE_NAME=spicedb
SPICEDB_MIGRATE_DB=true
SPICECTL_SCHEMA_FILE=
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

----------------------------------------
## Container: spicedb_db
POSTGRES_DB=spicedb_db
POSTGRES_USER=spicedb_db
POSTGRES_HOST_AUTH_METHOD=trust
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/postgresql/15/bin
GOSU_VERSION=1.16
LANG=en_US.utf8
PG_MAJOR=15
PG_VERSION=15.1-1.pgdg110+1
PGDATA=/var/lib/postgresql/data

----------------------------------------
## Container: grafana
GF_AUTH_ANONYMOUS_ENABLED=true
GF_AUTH_ANONYMOUS_ORG_ROLE=Admin
GF_AUTH_DISABLE_LOGIN_FORM=true
GF_FEATURE_TOGGLES_ENABLE=traceqlEditor
PATH=/usr/share/grafana/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
GF_PATHS_CONFIG=/etc/grafana/grafana.ini
GF_PATHS_DATA=/var/lib/grafana
GF_PATHS_HOME=/usr/share/grafana
GF_PATHS_LOGS=/var/log/grafana
GF_PATHS_PLUGINS=/var/lib/grafana/plugins
GF_PATHS_PROVISIONING=/etc/grafana/provisioning

----------------------------------------
## Container: tempo
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/busybox
SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt

----------------------------------------
## Container: token_exchange
ADDRESS=token_exchange:1024
METRICS_ADDRESS=token_exchange:1026
TELEMETRY_ADDRESS=tempo:4317
TLS_ENABLED=true
TLS_CA_CERT_FILEPATH=/etc/ssl/token_exchange-cachain.crt
TLS_SERVER_CERT_FILEPATH=/etc/ssl/token_exchange.crt
TLS_SERVER_KEY_FILEPATH=/etc/ssl/token_exchange.key
TELEMETRY_IS_USE_PROMETHEUS_METER=true
POSTGRES_IS_ENABLED=true
POSTGRES_DSN=postgres://token-exchangedb@172.29.147.2:5447/token-exchangedb
EXTERNAL_PROVIDER_NAMES_AND_PUBLIC_KEYS_FILEPATH=cloud-teleport:/etc/teleport.public.pem,gateway:/etc/gateway-public.pem
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

----------------------------------------
## Container: token-exchangedb
POSTGRES_DB=token-exchangedb
POSTGRES_USER=token-exchangedb
POSTGRES_HOST_AUTH_METHOD=trust
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/postgresql/15/bin
GOSU_VERSION=1.16
LANG=en_US.utf8
PG_MAJOR=15
PG_VERSION=15.1-1.pgdg110+1
PGDATA=/var/lib/postgresql/data

----------------------------------------

