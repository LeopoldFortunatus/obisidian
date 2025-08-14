user id from teleport failing validation
```
validate update failed to check permission [cloud:cl0#create_saga@user:cloud-teleport-aleksander.rykalin]: failed to check permission [cloud:cl0#create_saga@user:cloud-teleport-aleksander.rykalin]: rpc error: code = InvalidArgument desc = invalid CheckPermissionRequest.Subject: embedded message failed validation | caused by: invalid SubjectReference.Object: embedded message failed validation | caused by: invalid ObjectReference.ObjectId: value does not match regex pattern \"^(([a-zA-Z0-9/_|\\\\-=+]{1,})|\\\\*)$\"","grpc.code":"PermissionDenied","grpc.time_ms":117.219}

```