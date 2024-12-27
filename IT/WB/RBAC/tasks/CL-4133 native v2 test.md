Задача CL-4133 в ветке cl-4133:
Добавить тесты на v2 vmcontroller в sandbox-е

Я уже начал делать тесты в test/integration/native_v2_test.go
но там часть тестов v1 а другая часть v2
Сейчас нужно разобраться хотябы с хелпер методами createNamespaceVmControllerV2 и deleteNamespaceVmControllerV2

Для deleteNamespaceVmControllerV2 есть проблема при генерации метода ApiDeleteNamespaceRelation.

Проблема с `{$.ServiceName | Title}}{{.MethodName }}Relation` (например `ApiDeleteNamespaceRelation(ctx context.Context, response *emptypb.Empty)`)
Нужно в этот метод передавать данные для полей:
```
subjectIDValue  string  
relationValue   string  
resourceIDValue string
```
Это какое-то время делалось через response. Но в v2 response в некоторых методах Empty. Попытался передавать через `request_field` но сейчас я в сомнениях, кажется что не всегда будет удобно передавать request на тот уровень когда у нас отрабатывает создание\удаление релейшенов. Возможно нужно создавать кастомные реквест для каждого запроса данные котрого будут заполняться самостоятельно.