Требования:
- Проверку проводить в тестовых mr-ах.
- Не нужно делать проверку для всех объектов, достаточно на - Namespace и VM.
- Проверка должна включать взаимодействией сервисов:
    - Gateway
    - VMController
    - TokenExchange
    - SpiceDB
- Также следует проверить работу саг. Возможно это не получиться, т.к. имя сервиса берется из mtls-а.
- Также дложен быть обмен внешнего jwt на внутренний с помощью TokenExchange.  
    Пример получения внешнего токена - `curl --insecure --location 'https://g-cl-2823.mr.cloud.devel/fakejwt' --header 'x-clouduser: логинПользователя`

---

- В Gateway сделать новую ветку от - [https://gitlab-private.wildberries.ru/cloud/gateway-services/-/merge_requests/1399/pipelines](https://gitlab-private.wildberries.ru/cloud/gateway-services/-/merge_requests/1399/pipelines)  
    Доработки:
    
    - отключить mtls (спросить у [@Nikita Lisovskiy](https://youtrack.wildberries.ru/users/lisovskiy.nikita2) )
    - добавить интерсептор для обмена токена (спросить у [@Nikita Lisovskiy](https://youtrack.wildberries.ru/users/lisovskiy.nikita2)) Еще есть старая интеграция Gateway с TokenExchange в ней есть правильные клюи что откуда берется - [https://gitlab-private.wildberries.ru/cloud/gateway-services/-/merge_requests/1032](https://gitlab-private.wildberries.ru/cloud/gateway-services/-/merge_requests/1032)
- В VMController сделать новую ветку от - [https://gitlab-private.wildberries.ru/cloud/cloud-native/-/merge_requests/1539/pipelines](https://gitlab-private.wildberries.ru/cloud/cloud-native/-/merge_requests/1539/pipelines)