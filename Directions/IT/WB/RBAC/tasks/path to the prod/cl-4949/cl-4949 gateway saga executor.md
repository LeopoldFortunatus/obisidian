ID саги есть, в бд гейта, таблица saga_state 
Контейнер для всех гейтовых операций общий, front_gateway_1

user id from teleport failing validation
```
{"level":"debug","datetime":"2025-08-14T08:36:28.880Z","logger":"NamespaceService","caller":"handlers/service.go:976","msg":"CreateSagaRelations: sagaID 64a9f6a5-7207-4fa8-9b35-4233a0d118da, namespaceID 858fb543-f793-47b7-a373-4023756644bd, stateID 00a276a6-f724-4803-8e6a-6c3e90c66fe4, cluster az1.g-cl-4949-gw-rbac-1.mr.cloud.devel, networkAddress 192.168.1.0"}
{"level":"warn","datetime":"2025-08-14T08:36:28.950Z","caller":"zap/options.go:212","msg":"finished unary call with code PermissionDenied","grpc.start_time":"2025-08-14T08:36:28Z","system":"grpc","span.kind":"server","grpc.service":"gateway.namespace.v1.grpcserver.Namespace","grpc.method":"CreateNamespace","peer.address":"127.0.0.1:45014","error":"spiceDBSagaCli.CreateSagaRelations err: rpc error: code = PermissionDenied desc = permission denied for relation - {user cloud-teleport-aleksander.rykalin saga saga 64a9f6a5-7207-4fa8-9b35-4233a0d118da}: rpc error: code = PermissionDenied desc = Authorization error: validate update failed to check permission [cloud:cl0#create_saga@user:cloud-teleport-aleksander.rykalin]: failed to check permission [cloud:cl0#create_saga@user:cloud-teleport-aleksander.rykalin]: rpc error: code = InvalidArgument desc = invalid CheckPermissionRequest.Subject: embedded message failed validation | caused by: invalid SubjectReference.Object: embedded message failed validation | caused by: invalid ObjectReference.ObjectId: value does not match regex pattern \"^(([a-zA-Z0-9/_|\\\\-=+]{1,})|\\\\*)$\"","grpc.code":"PermissionDenied","grpc.time_ms":99.443}
{"level":"info","datetime":"2025-08-14T08:36:28.950Z","caller":"zap/options.go:212","msg":"finished client unary call","system":"grpc","span.kind":"client","grpc.service":"gateway.namespace.v1.grpcserver.Namespace","grpc.method":"CreateNamespace","error":"rpc error: code = PermissionDenied desc = spiceDBSagaCli.CreateSagaRelations err: rpc error: code = PermissionDenied desc = permission denied for relation - {user cloud-teleport-aleksander.rykalin saga saga 64a9f6a5-7207-4fa8-9b35-4233a0d118da}: rpc error: code = PermissionDenied desc = Authorization error: validate update failed to check permission [cloud:cl0#create_saga@user:cloud-teleport-aleksander.rykalin]: failed to check permission [cloud:cl0#create_saga@user:cloud-teleport-aleksander.rykalin]: rpc error: code = InvalidArgument desc = invalid CheckPermissionRequest.Subject: embedded message failed validation | caused by: invalid SubjectReference.Object: embedded message failed validation | caused by: invalid ObjectReference.ObjectId: value does not match regex pattern \"^(([a-zA-Z0-9/_|\\\\-=+]{1,})|\\\\*)$\"","grpc.code":"PermissionDenied","grpc.time_ms":117.219}

```
https://band.wb.ru/wbcloud/pl/uajiagyxubfkxc6rgbbj5cngjh

Александр Сизов

[08/14/2025 at 12:11 PM](https://band.wb.ru/wbcloud/pl/6qys497bqjrktpfxiffgai518e)

Тут проблема возможно глубже. Может стоит вобще отказаться от логина и использовать id.

[12:11 PM](https://band.wb.ru/wbcloud/pl/ueypefnmbid4trsuunwzau6swe)

Записал, сегодня обсужу с безопасниками - [https://youtrack.wildberries.ru/issue/CL-4363/Voprosy-po-bezopasnosti](https://youtrack.wildberries.ru/issue/CL-4363/Voprosy-po-bezopasnosti)

![rykalin.a profile image](https://band.wb.ru/api/v4/users/1x8pjr7h7tb6tdwcoimfqgak7o/image?_=1724872337750)

Александр Рыкалин

[08/14/2025 at 12:13 PM](https://band.wb.ru/wbcloud/pl/g5bo8n87ntfkjj9z8d1wxsx6nc)

А давай я тогода, чтобы не буксовать на этом блокере начну писать метод замены user-id в token-exchange. Потом можно будет изменить его логику, а пока сделаю просто с подстановкой dot вместо точки Edited

[12:15 PM](https://band.wb.ru/wbcloud/pl/qhjejejmjbgupfejgi7uas8e5a)

Потому что мне без работы с сагой в cl-4949 не продвинуться

![sizov.aleksandr4 profile image](https://band.wb.ru/api/v4/users/6ppofrj1kj8wtm4nonuqpwp8fc/image?_=1738928085805)

Александр Сизов

[08/14/2025 at 12:16 PM](https://band.wb.ru/wbcloud/pl/shyrnpphftnojdoa71gcptps4a)

Да сделай пока замену в TokenExchange, только напиши что это не нужно сливать.

![rykalin.a profile image](https://band.wb.ru/api/v4/users/1x8pjr7h7tb6tdwcoimfqgak7o/image?_=1724872337750)

Александр Рыкалин

[08/14/2025 at 12:17 PM](https://band.wb.ru/wbcloud/pl/1t7xgsjjzir83xy11rksruw7dr)

ок

## Sanitize subject
https://gitlab-private.wildberries.ru/cloud/token-exchange/-/merge_requests/220
```
Subject: [PATCH] CL-4949 sanitize internal token subject (replace "." -> "-dot-")
---
Index: internal/usecases/usecases.go
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/internal/usecases/usecases.go b/internal/usecases/usecases.go
--- a/internal/usecases/usecases.go	(revision 68eec7e6f6c562f6b2cf1445770a7b98e42c3b8d)
+++ b/internal/usecases/usecases.go	(revision 303a2b04a4c918b5caa238779710fa6c53ffefbf)
@@ -5,6 +5,7 @@
 	"errors"
 	"fmt"
 	"log/slog"
+	"strings"
 	"time"
 
 	"gitlab-private.wildberries.ru/cloud/token-exchange/internal/usecases/cache"
@@ -80,10 +81,15 @@
 	}
 
 	fingerprint := u.internalProvider.GetFingerprint()
+
+	// sanitize subject (replace "." -> "-dot-") CL-4949 DO NOT MERGE
+	rawExternalSubject := exchangeTokenMetrics.ExternalTokenInfo.Subject
+	sanitizedExternalSubject := strings.ReplaceAll(rawExternalSubject, ".", "-dot-")
+
 	subject := fmt.Sprintf(
 		"%s-%s",
 		exchangeTokenMetrics.ExternalTokenInfo.Issuer,
-		exchangeTokenMetrics.ExternalTokenInfo.Subject,
+		sanitizedExternalSubject,
 	)
 	exchangeTokenMetrics.InternalTokenInfo = &domain.InternalTokenInfo{
 		PublicKeyID:     fingerprint,

```
