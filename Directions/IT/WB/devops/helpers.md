# Очистка стенда 
create stand failed
по идеи да. у меня в директории лежит /home/sivcov.aleksey/lib.sh, в нем сверху в переменных исправить имя стенда и запустить

```
root@cloud-runner-3:/home/sivcov.aleksey# cat lib.sh 
#!/bin/bash

# Параметры удаления (можно задать через переменные окружения)
CLUSTER_NAME="${CLUSTER_NAME:-g-cl-4664-new2}"
```
и после него уже можно выполнить в пайпе destroy_stand и перезапустить пайп