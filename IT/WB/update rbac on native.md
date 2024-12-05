
1. Update rbac in Makefile: https://gitlab-private.wildberries.ru/cloud/cloud-native/blob/Makefile#L58
2. run `make generate-controller-rbac`
3. update in go mod: `go get gitlab-private.wildberries.ru/cloud/rbac-interceptor@v$VERSION$`
4. check
5. Update rbac in docker - https://gitlab-private.wildberries.ru/cloud/sre-tools/docker_containers/-/edit/master/native-golang/Dockerfile?ref_type=heads
6. create MR, request merge https://band.wb.ru/wbcloud/channels/gitlab
7. update native-golang to new version. replace harbor.wildberries.ru/cloud/native-golang:OLD_DATE

harbor images: https://harbor.wildberries.ru/harbor/projects/702/repositories/native-golang/artifacts-tab