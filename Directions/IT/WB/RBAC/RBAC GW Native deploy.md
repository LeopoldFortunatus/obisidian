# Environement
```
docker ps -q | while read container; do
  echo "## Container: $(docker inspect --format '{{.Name}}' $container | sed 's/\///')"
  docker inspect --format '{{range .Config.Env}}{{println .}}{{end}}' $container
  echo "----------------------------------------"
done

```

## Container: maintenance
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
NGINX_VERSION=1.22.1
NJS_VERSION=0.7.11
PKG_RELEASE=1~bullseye

----------------------------------------
## Container: front-proxy-1
PROXY_IMAGE=harbor.wildberries.ru/cloud/gateway-services/openresty
PROXY_TAG=cl-4949-gw-rbac-1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/local/openresty/luajit/bin:/usr/local/openresty/nginx/sbin:/usr/local/openresty/bin

----------------------------------------
## Container: front-metrics-1
METRICS_TAG=cl-4949-gw-rbac-1
METRICS_IMAGE=harbor.wildberries.ru/cloud/gateway-services/openresty
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/local/openresty/luajit/bin:/usr/local/openresty/nginx/sbin:/usr/local/openresty/bin

----------------------------------------
## Container: front-frontend-1
FRONTEND_IMAGE=harbor.wildberries.ru/cloud/gateway-services/frontend
FRONTEND_TAG=cl-4949-gw-rbac-1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
NGINX_VERSION=1.22.1
NJS_VERSION=0.7.11
PKG_RELEASE=1~bullseye

----------------------------------------
## Container: front-notificationsdb-1
POSTGRES_DB=notificationsdb
POSTGRES_USER=notificationsdb
POSTGRES_PASSWORD=notificationsdb
POSTGRES_HOST_AUTH_METHOD=trust
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/postgresql/15/bin
GOSU_VERSION=1.16
LANG=en_US.utf8
PG_MAJOR=15
PG_VERSION=15.1-1.pgdg110+1
PGDATA=/var/lib/postgresql/data

----------------------------------------
## Container: front-docker_exporter-1
PATH=/usr/local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
LANG=C.UTF-8
GPG_KEY=A035C8C19219BA821ECEA86B64E628F8D684696D
PYTHON_VERSION=3.11.10
PYTHON_SHA256=07a4356e912900e61a15cb0949a06c4a05012e213ecd6b4e84d0f67aabbee372

----------------------------------------
## Container: front-clickhouse-query-1
CLICKHOUSE_PASSWORD=hLC8sX6aP4IxWZlluuMfukQW7moRTFyi
CLICKHOUSE_QUERY_INTERVAL_SEC=43200
CLICKHOUSE_HOST=clickhouse.cloud.devel
CLICKHOUSE_DB=billing
CLICKHOUSE_PORT=9001
CLICKHOUSE_USER=billing_user
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
LANG=en_US.UTF-8
LANGUAGE=en_US:en
LC_ALL=en_US.UTF-8

----------------------------------------
## Container: front-swagger-1
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
## Container: front-userdb-1
POSTGRES_DB=userdb
POSTGRES_USER=userdb
POSTGRES_PASSWORD=userdb
POSTGRES_HOST_AUTH_METHOD=trust
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/postgresql/15/bin
GOSU_VERSION=1.16
LANG=en_US.utf8
PG_MAJOR=15
PG_VERSION=15.1-1.pgdg110+1
PGDATA=/var/lib/postgresql/data

----------------------------------------
## Container: front-ipamdb-1
POSTGRES_PASSWORD=ipamdb
POSTGRES_HOST_AUTH_METHOD=trust
POSTGRES_DB=ipamdb
POSTGRES_USER=ipamdb
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/postgresql/15/bin
GOSU_VERSION=1.16
LANG=en_US.utf8
PG_MAJOR=15
PG_VERSION=15.1-1.pgdg110+1
PGDATA=/var/lib/postgresql/data

----------------------------------------
## Container: front-quotadb-1
POSTGRES_DB=quotadb
POSTGRES_USER=quotadb
POSTGRES_PASSWORD=quotadb
POSTGRES_HOST_AUTH_METHOD=trust
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/postgresql/15/bin
GOSU_VERSION=1.16
LANG=en_US.utf8
PG_MAJOR=15
PG_VERSION=15.1-1.pgdg110+1
PGDATA=/var/lib/postgresql/data

----------------------------------------
## Container: front-gatewaydb-1
POSTGRES_USER=gatewaydb
POSTGRES_PASSWORD=userdb
POSTGRES_HOST_AUTH_METHOD=trust
POSTGRES_DB=gatewaydb
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/postgresql/15/bin
GOSU_VERSION=1.16
LANG=en_US.utf8
PG_MAJOR=15
PG_VERSION=15.1-1.pgdg110+1
PGDATA=/var/lib/postgresql/data

----------------------------------------
## Container: front-imagedb-1
POSTGRES_HOST_AUTH_METHOD=trust
POSTGRES_DB=imagedb
POSTGRES_USER=imagedb
POSTGRES_PASSWORD=imagedb
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/postgresql/15/bin
GOSU_VERSION=1.16
LANG=en_US.utf8
PG_MAJOR=15
PG_VERSION=15.1-1.pgdg110+1
PGDATA=/var/lib/postgresql/data

----------------------------------------
## Container: rbac_interceptor_spicedb
SPICEDB_TELEMETRY_ADDRESS=tempo:4317
SPICEDB_TELEMETRY_METER_INTERVAL=60s
SPICEDB_TELEMETRY_TRACE_TIMEOUT=5s
SPICEDB_TELEMETRY_IS_USE_PROMETHEUS_METER=true
SPICEDB_METRICS_ADDRESS=0.0.0.0:50054
SPICEDB_METRICS_READ_TIMEOUT=30s
SPICEDB_METRICS_WRITE_TIMEOUT=60s
SPICEDB_METRICS_IDLE_TIMEOUT=60s
SPICEDB_METRICS_CRT_FILEPATH=
SPICEDB_LOG_LEVEL=debug
SPICEDB_LOG_JSON_OUTPUT=true
SPICEDB_CONNECTION=postgres://spicedb@172.31.113.2:5445/spicedb
SPICEDB_ADDRESS=0.0.0.0:50052
SPICEDB_ADMIN_ADDRESS=localhost:50053
SPICEDB_JWT_PUBLIC_KEY_PATH=/etc/jwt_provider.pem
SPICEDB_CLIENT_TIMEOUT=300ms
SPICEDB_READ_RESOURCE_TIMEOUT=5s
SPICEDB_SCOPE_CONFIG_FILE=./config/scope/scope_config.yaml
SPICEDB_SERVICE_NAME=rbac-interceptor-spicedb
SPICEDB_MIGRATE_DB=true
SPICECTL_SCHEMA_FILE=
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

----------------------------------------
## Container: spicedb
POSTGRES_DB=spicedb
POSTGRES_USER=spicedb
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
## Container: token-exchange
ADDRESS=0.0.0.0:1024
METRICS_ADDRESS=0.0.0.0:1026
TELEMETRY_ADDRESS=tempo:4317
TLS_ENABLED=false
TLS_CA_CERT_FILEPATH=/etc/token-exchange-ca.crt
TLS_SERVER_CERT_FILEPATH=/etc/token-exchange.crt
TLS_SERVER_KEY_FILEPATH=/etc/token-exchange.key
TELEMETRY_IS_USE_PROMETHEUS_METER=true
POSTGRES_IS_ENABLED=true
POSTGRES_DSN=postgres://token-exchangedb@172.31.113.2:5447/token-exchangedb
EXTERNAL_PROVIDER_NAMES_AND_PUBLIC_KEYS_FILEPATH=teleport:/etc/teleport.public.pem,gateway:/etc/gateway-public.pem
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
