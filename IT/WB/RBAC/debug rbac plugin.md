```
# build plugin with debug falgs
DEBUG=true make build_rbac_plugi
# run plugin with delve debug tool
DEBUG=true make generate_<SOMETHING>
```
This will force protoc to run dlv wrapper scripts/debug/protoc-gen-go-rbac for rbac plugin.
You will see:
```
2024-12-26T12:21:28+03:00 warning layer=rpc Listening for remote connections (connections are not authenticated nor encrypted)

```
After that create go remote configuration in IDE to localhost port 2345. Set breakpoints in plugin and run configuration.