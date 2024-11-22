
1. Update rbac in Makefile: https://gitlab-private.wildberries.ru/cloud/cloud-native/blob/Makefile#L58
2. run `make generate-controller-rbac`
3. check
4. Update rbac in docker - https://gitlab-private.wildberries.ru/cloud/sre-tools/docker_containers/-/edit/master/native-golang/Dockerfile?ref_type=heads
5. create MR, request merge https://band.wb.ru/wbcloud/channels/gitlab
6. update native-golang to new version. replace harbor.wildberries.ru/cloud/native-golang:OLD_DATE