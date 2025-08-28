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

#### Реализовать saga-executor
Что для этого нуцжно?
- Создавать сагу релейшн из гейтвея, это у нас генериться или врусную пишется? плохо помню
- Вызвать при создании саги CreateSagaRelations (rbac-interceptor/pkg/sagas/client.go) Done
- Как создать контекс при выполнении саги чтобы она выполнялась под сервисом?
	- Как сейчас устроена сервиная аутентификация?
##### user id из телепорта не проходит валидацию в spicedb
`[cloud:cl0#create_saga@user:cloud-teleport-aleksander.rykalin]: rpc error: code = InvalidArgument desc = invalid CheckPermissionRequest.Subject` Fixed (https://gitlab-private.wildberries.ru/cloud/token-exchange/-/blob/303a2b04a4c918b5caa238779710fa6c53ffefbf/internal/usecases/usecases.go#L86)

##### При выполнении саги надо получать серисный токен от token-exchange
#wb_token_exchange 
###### Условия
- есть MTLS к token-exchnage
- в клиентском сертификате прописано имя сервиса которое добавлено в список [ENABLED_TLS_SERVICE_NAME](https://gitlab-private.wildberries.ru/cloud/token-exchange/blob/75e5d5bba20abc35799e2e589fd034372fb48248/internal/configs/config.go#L80) в token-exchange
###### Клиентский интерспетор в нейтив клиентах
Надо добавить наши интерцепторы в билдеры [клиентов](https://gitlab-private.wildberries.ru/cloud/gateway-services/-/blob/c2adf1a4a4f46cb85ab8a05ee816c29c5cf1353b/cmd/gwadmin/main.go#L653) (например: `clientNamespace := preparedClient.NewNamespaceClientFabric(nativeClients)`) котрые затем передаются в [sagas.TaskBuilder](https://gitlab-private.wildberries.ru/cloud/gateway-services/-/blob/c2adf1a4a4f46cb85ab8a05ee816c29c5cf1353b/cmd/gwadmin/main.go#L690)

Когда запрос выполняется внутри клиента можно контекст обернуть в метод GenerateServiceJWT который посылает по MTLS запрос на токен в TokenExchange
Пример:
```
package main

import (
    "context"

    "gitlab-private.wildberries.ru/cloud/token-exchange/pkg/interceptors/servicejwt"
)

func main() {
    client := VMControllerClient() // Есть все интерсепторы, включая servicejwt

    ctx := context.Background()
    client.GetVMs(ctx) // Без генерации jwt

    client.GetVMs(servicejwt.UseServiceToken(ctx)) // C генерацией сервисного jwt
}
```

Клиентский интерсептор, который должен быть включен - https://gitlab-private.wildberries.ru/cloud/token-exchange/-/tree/master/pkg/interceptors/servicejwt?ref_type=heads

Получение клиентских интерсепторов - [https://gitlab-private.wildberries.ru/cloud/rbac-interceptor/blob/1f85ff5b904793dc50578f9a8a0378c8cb8ee54b/pkg/builders/builder.go#L107](https://gitlab-private.wildberries.ru/cloud/rbac-interceptor/blob/1f85ff5b904793dc50578f9a8a0378c8cb8ee54b/pkg/builders/builder.go#L107)


UseServiceToken: https://gitlab-private.wildberries.ru/cloud/token-exchange/blob/e0dc01b2b22db67a2e577905e8a6f9e36e62f19f/pkg/interceptors/servicejwt/clientinterceptor.go#L84

Два условия:


https://gitlab-private.wildberries.ru/cloud/token-exchange/blob/75e5d5bba20abc35799e2e589fd034372fb48248/internal/configs/config.go#L80

## настройка в гейтвее
добавить клиентский интерсептор в нейтив клиент
pkg/gateway/client/prepared/v2/client.go:23
```
NewNativeClient(ctx context.Context, opts NewClientConfig) (map[clusterName]core_client.ApiClient, error)
```

надо добавить клиентский интерцептор в generateRBACinterceptors
cmd/gwadmin/main.go:801

по идее они тут уже включены
```
WithClientInterceptors(  
    rbacBuilders.NewClientInterceptorsConfig(rbacInterceptorsSoftError).  
       WithLogger(slogLoggerClient),  
).
```
cmd/gwadmin/main.go:855