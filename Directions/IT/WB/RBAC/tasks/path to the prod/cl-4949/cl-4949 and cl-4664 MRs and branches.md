#wb-cl-4949 #wb-cl-4664
# CL-4664 token-exchange integration
## Gateway
### cl-4664-new2 -> master
[MR 1568](https://gitlab-private.wildberries.ru/cloud/gateway-services/-/merge_requests/1568) 
Обновление версии RBAC в Gateway
#### gitlab-ci.yaml
```
- project: "cloud/sre-tools/ci"  
  ref: cl-4738
```
#### requirements.yml
```
collections:  
  - name: "https://gitlab-private.wildberries.ru/cloud/sre-tools/wbcloud_sre.git"  
    type: git  
    version: cl-4738
```
#### go.mod
```
gitlab-private.wildberries.ru/cloud/box v0.1.35  
gitlab-private.wildberries.ru/cloud/rbac-interceptor v0.0.46  
gitlab-private.wildberries.ru/cloud/token-exchange v0.0.31
```
# CL-4949 saga and native integration

## Gateway
### cl-4949-gw-rbac-1 -> cl-4664-new2 -> master
[MR 1533](https://gitlab-private.wildberries.ru/cloud/gateway-services/-/merge_requests/1533)

**NATIVE_BRANCH:** "cl-4949-gw-rbac"
[NATIVE MR 1626](https://gitlab-private.wildberries.ru/cloud/cloud-native/-/merge_requests/1626/pipelines)
CL-4949 Проверка интеграции native-gw-rbac

#### gitlab-ci.yaml
```
- project: "cloud/sre-tools/ci"  
  ref: cl-4738
```
#### requirements.yml
```
collections:  
  - name: "https://gitlab-private.wildberries.ru/cloud/sre-tools/wbcloud_sre.git"  
    type: git  
    version: cl-4738
```
#### go.mod
```
gitlab-private.wildberries.ru/cloud/box v0.1.35  
gitlab-private.wildberries.ru/cloud/rbac-interceptor v0.0.46  
gitlab-private.wildberries.ru/cloud/token-exchange v0.0.31
```

## Native
### cl-4949-gw-rbac
[MR 1626](https://gitlab-private.wildberries.ru/cloud/cloud-native/-/merge_requests/1626/pipelines)
```
- project: "cloud/sre-tools/ci"  
  ref: cl-4738
```

## Token exchange
### CL-4949 debug log and sanitize subject
https://gitlab-private.wildberries.ru/cloud/token-exchange/-/merge_requests/220

# Sre-tools
### CL-4738
https://gitlab-private.wildberries.ru/cloud/sre-tools/ci/-/tree/cl-4738
MTLS для spicedb\token-exchange
## wbcloud_sre
### CL-4738
https://gitlab-private.wildberries.ru/cloud/sre-tools/wbcloud_sre/-/compare/master...cl-4738
MTLS для spicedb\token-exchange