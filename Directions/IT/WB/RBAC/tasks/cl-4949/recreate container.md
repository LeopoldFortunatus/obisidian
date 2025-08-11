#docker
# Get the container ID
CONTAINER_ID=$(docker ps -qf "name=token-exchange")

# Create a new container using the same image and options but with new env file

```
# Get the container ID
CONTAINER_ID=$(docker ps -qf "name=token-exchange")
```

```
NEW_CONTAINER_ID=$(docker container create --name token-exchange-new \
  $(docker inspect --format '{{range .Config.Env}}--env {{.}} {{end}}' $CONTAINER_ID | sed 's/--env [^=]*=token[^=]*[[:space:]]//g') \
  --env-file /opt/token-exchange/config.env \
  $(docker inspect --format '{{range $mount := .Mounts}}--volume {{$mount.Source}}:{{$mount.Destination}}{{if $mount.Mode}}:{{$mount.Mode}}{{end}} {{end}}' $CONTAINER_ID) \
  $(docker inspect --format '{{range $p, $conf := .NetworkSettings.Ports}}{{range $conf}}--publish {{.HostIp}}:{{.HostPort}}:{{$p}} {{end}}{{end}}' $CONTAINER_ID) \
  $(docker inspect --format '{{.Config.Image}}' $CONTAINER_ID))
```

