### zed schema diag
```
export SPICEDB_ADMIN_ADDRESS=localhost:50053 
export ZED_KEYRING_PASSWORD=""
zed context set dev $SPICEDB_ADMIN_ADDRESS "" --insecure
zed schema read --insecure
```