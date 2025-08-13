Надо в vmctl настроить jwtexchange чтобы понять как оно работает в гейтвее

Попробовать в пайплайне с обменом токена от auth.service и также с отключенными auth.service интерцепторами.
Подтяруть мастер гейтвея и нейтива.

# Пофиксить ошибку при создании неймспейса
vmcontroller: `does not exist jwtauth context in client method`
```
Aug 13 08:16:34 c1-az1.az1.g-cl-4949-gw-rbac-1.mr.cloud.devel vmcontroller[38167]: {"level":"debug","svc":"pub_api_v2","error":"failed to create namespace relation: failed to create relationship [[{ResourceType:namespace ResourceID:a79a3637cc1965c63f6df12cbd426a4ae7f7ebdd242851c240313094047d4625 Relation:cloud SubjectType:cloud SubjectID:cl0}]]: failed to add relation - {namespace a79a3637cc1965c63f6df12cbd426a4ae7f7ebdd242851c240313094047d4625 cloud cloud cl0}: does not exist jwtauth context in client method - /authzed.api.v1.PermissionsService/WriteRelationships: does not exist jwtauth context: bad data or subject does not exist","time":"2025-08-13T08:16:34Z","message":"retry txn in postgresql: create namespace [jo8ygkbljkh] after 1401 ms"}
```
token-exchange:

## Проект (неймспейс) создается в саге
В саге пустой контекст, надо с этим разобраться
https://gitlab-private.wildberries.ru/cloud/gateway-services/-/blob/master/internal/gateway/namespace/handlers/service.go?ref_type=heads#L957

`func (s *nsService) CreateNamespace(ctx context.Context, request *models.CreateNamespaceRequest)`

### В сагу не передается контекст
```
err = s.sagaExecutor.Add("CreateNamespace", nsSagas.CreateNSTaskState{  
    UserActionLogID: uuid.NewString(),  
    Request: &models.CreateNamespaceRequest{  
       NamespaceID:    id,  
       StateID:        stateID,  
       Name:           request.Name,  
       Cluster:        cluster,  
       UserID:         request.UserID,  
       NetworkAddress: networkAddress,  
    },  
})
```
internal/gateway/namespace/sagas/create_ns_task.go
#### Варианты решения
Прокинуть рользовательский контекст чтобы проверить что работает на этом уровне. Но это костыль.

##### Реализовать saga-executor
Что для этого нуцжно?
- Вспомнить как там вообще все утсроено
- Создавать сагу релейшн из гейтвея, это у нас генериться или врусную пишется? плохо помню