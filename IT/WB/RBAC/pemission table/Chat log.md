Saved

![sizov.aleksandr4 profile image](https://band.wb.ru/api/v4/users/6ppofrj1kj8wtm4nonuqpwp8fc/image?_=1738928085805)

Александр Сизов

[02/27/2025 at 2:23 PM](https://band.wb.ru/wbcloud/pl/4ghtmjhwep8m3bfrkoywersysy)

Всем привет! Рассписали роли и что они могут делать в виде табилц:

[https://gitlab-private.wildberries.ru/cloud/rbac-interceptor/-/merge_requests/271/diffs](https://gitlab-private.wildberries.ru/cloud/rbac-interceptor/-/merge_requests/271/diffs)

@Андрей Мякоткин @Георгий Поляков @Руслан Латыпов @Геннадий Тарада @Евгений Малахов @Богдан Савин прошу ознакомиться и дать обратную связь.

@Александр Рыкалин fyi Edited

![](https://band.wb.ru/static/emoji/1f44d.png)1

45 replies

![tarada.gennadiy profile image](https://band.wb.ru/api/v4/users/g94n6bzpei885f1cnq39u7eyuh/image?_=1721895799980)

Геннадий Тарада

[02/27/2025 at 2:25 PM](https://band.wb.ru/wbcloud/pl/qf6u17qujjnyzbfrzpknd79oxc)

сразу там можно добавлю плюсов? Edited

![latypov.ruslan2 profile image](https://band.wb.ru/api/v4/users/51wza6rtz3fwde9baw6i3d3g9y/image?_=1731083254094)

Руслан Латыпов

[02/27/2025 at 2:26 PM](https://band.wb.ru/wbcloud/pl/yskf4aq9mbn8ib84ed464ajn1c)

а че меня тегать, если там нет пгаасных методов?

![sizov.aleksandr4 profile image](https://band.wb.ru/api/v4/users/6ppofrj1kj8wtm4nonuqpwp8fc/image?_=1738928085805)

Александр Сизов

[02/27/2025 at 2:27 PM](https://band.wb.ru/wbcloud/pl/seyc5smkrtdrpnepxnrsaq495y)

Геннадий Тарада

сразу там можно добавлю плюсов?

Да, можно прям в mr-е

![tarada.gennadiy profile image](https://band.wb.ru/api/v4/users/g94n6bzpei885f1cnq39u7eyuh/image?_=1721895799980)

Геннадий Тарада

[02/27/2025 at 2:27 PM](https://band.wb.ru/wbcloud/pl/6p3gicyxdbbwpfmrt9kebupnor)

хм, admin и devops чем отличаются?

![latypov.ruslan2 profile image](https://band.wb.ru/api/v4/users/51wza6rtz3fwde9baw6i3d3g9y/image?_=1731083254094)

Руслан Латыпов

[02/27/2025 at 2:28 PM](https://band.wb.ru/wbcloud/pl/hdby7oaietb33pnm7sf86awxpw)

админы пишут баш-скрипты, а девопсы - плейбуки!

![](https://band.wb.ru/static/emoji/1f601.png)1

![sizov.aleksandr4 profile image](https://band.wb.ru/api/v4/users/6ppofrj1kj8wtm4nonuqpwp8fc/image?_=1738928085805)

Александр Сизов

[02/27/2025 at 2:29 PM](https://band.wb.ru/wbcloud/pl/cpkkofhp8id8bk3imfutzkk14h)

Геннадий Тарада

хм, admin и devops чем отличаются?

admin - это роль в неймспейсе. Т.е. его действия ограничены конкретным неймспейсом.

[2:30 PM](https://band.wb.ru/wbcloud/pl/iizpfo7jcifj8e1n8xhsa3qs4r)

devops - выполняет действия над любой сущностью облака.

[2:31 PM](https://band.wb.ru/wbcloud/pl/eg5epxsdejfz3rccfi13stuwrr)

Возможно стоит пересмотреть нейминг, что бы не было путаницы.

![tarada.gennadiy profile image](https://band.wb.ru/api/v4/users/g94n6bzpei885f1cnq39u7eyuh/image?_=1721895799980)

Геннадий Тарада

[02/27/2025 at 2:38 PM](https://band.wb.ru/wbcloud/pl/78okst9wmjfoumitg6nemokw6e)

спасибо!

![malahov.evgeniy9 profile image](https://band.wb.ru/api/v4/users/93qrfmbhabnm3e5ip1ifronfmw/image?_=1725015069761)

Евгений Малахов

[02/27/2025 at 2:46 PM](https://band.wb.ru/wbcloud/pl/tejewpti7jybjjn5i77bw3znme)

@Александр Сизов а ты уверен, что нужно мешать здесь функционал облака (для внешних пользователей) и наши сервисные/служебные?

[2:47 PM](https://band.wb.ru/wbcloud/pl/d7nn7cpok78qxyssuzmw3hobtw)

в таблице нет роли "creator"

[2:48 PM](https://band.wb.ru/wbcloud/pl/x11smpkj87b3t81owyxk9ch14a)

pgaas, кажется должен быть здесь

[2:49 PM](https://band.wb.ru/wbcloud/pl/txbb5r1xjpfydji1u687e55b7y)

для служебного пользования (админка) нет (или не увидел) доступа на чтение

[2:50 PM](https://band.wb.ru/wbcloud/pl/siqda3d8cpdtzxqz11mcjocopw)

не для девопсов

![sizov.aleksandr4 profile image](https://band.wb.ru/api/v4/users/6ppofrj1kj8wtm4nonuqpwp8fc/image?_=1738928085805)

Александр Сизов

[02/27/2025 at 2:50 PM](https://band.wb.ru/wbcloud/pl/9w6h4z8popbyfcftzjtywbqzie)

Что за `creator`?

![malahov.evgeniy9 profile image](https://band.wb.ru/api/v4/users/93qrfmbhabnm3e5ip1ifronfmw/image?_=1725015069761)

Евгений Малахов

[02/27/2025 at 2:50 PM](https://band.wb.ru/wbcloud/pl/pstzeqbcajypuqpxafqemoib6y)

[https://docs.wb-cloud.ru/access-control.html#access-control-roles](https://docs.wb-cloud.ru/access-control.html#access-control-roles)

![](https://band.wb.ru/static/emoji/1f44d.png)1

[2:51 PM](https://band.wb.ru/wbcloud/pl/txy314oy378cxc54gxcokao8ty)

Разрешения: Создавать и изменять ресурсы, но не удалять их

![sizov.aleksandr4 profile image](https://band.wb.ru/api/v4/users/6ppofrj1kj8wtm4nonuqpwp8fc/image?_=1738928085805)

Александр Сизов

[02/27/2025 at 3:00 PM](https://band.wb.ru/wbcloud/pl/i1eonswinb8j8rcodmzywgfjnc)

Вобще со служебными пользователями (devops-ы, безопасники, pm-ы) более сложная ситуация.

Если к ним применять такуюже модель, когда есть роль и от роли доступны определенные права, то теряется гибкость - допустим кто-то может создавать образы, а кто-то нет, кто-то может регистрировать новых пользователей для работы через yubikey, а кто-то нет.

Т.е. тут появляется граздо больше ролей.

В целом служебных пользователей не так много и возможно стоит каждому выставлять нужные пермишены. Но тут можно столкнуться с подводными камнями - выдать права на создание какой-нибудь сущности, а на чтение не дать.

А возможно стоит заменить роли на группы. Группа объеденяет набор пермишенов (доступов). Пользователю можно задать несколько групп.

![malahov.evgeniy9 profile image](https://band.wb.ru/api/v4/users/93qrfmbhabnm3e5ip1ifronfmw/image?_=1725015069761)

Евгений Малахов

[02/27/2025 at 3:03 PM](https://band.wb.ru/wbcloud/pl/xpqwo1gey7nidqhwdxhefoexaw)

> А возможно стоит заменить роли на группы. Группа объеденяет набор пермишенов (доступов). Пользователю можно задать несколько групп.

я бы сюда целился

[3:03 PM](https://band.wb.ru/wbcloud/pl/udxzwogr1ifi7bufqzzimo11dh)

имхо, по пермишнам очень быстро сдуемся

[3:04 PM](https://band.wb.ru/wbcloud/pl/g6zy8b4373d3mk4fnzaipp97dy)

у тебя в рбак для обычного нашего функционала облака должна такая же модель появиться в будущем

[3:05 PM](https://band.wb.ru/wbcloud/pl/jzbic1wj87f38kgdspnpgjwqzr)

типа в проекте есть глобальный админ, он все может

[3:05 PM](https://band.wb.ru/wbcloud/pl/bc484isoat8zimx943tm93e7so)

+ добавляется гибкая, типа админ не всего проекта, а админ конкретно виртуалок

[3:05 PM](https://band.wb.ru/wbcloud/pl/f4eh88ixo7dntkoumcthaatwde)

либо пгааса

[3:05 PM](https://band.wb.ru/wbcloud/pl/sh9cshqy87g4pkhdq4qjpnq77h)

типа у меня в команде есть несколько ролей: dba, netops, devops

[3:06 PM](https://band.wb.ru/wbcloud/pl/wn5gprgtk3dpdqckfm4e1khp8h)

dba - админит кластера пг, нетопс - сетевые сервисы девопс - виртуалки и прч.

![savin.bogdan profile image](https://band.wb.ru/api/v4/users/hpk7iaeg5bbydg7qiehhqq76ze/image?_=1721897072240)

Богдан Савин

[02/27/2025 at 4:35 PM](https://band.wb.ru/wbcloud/pl/acweq44y93bz5kp19gsiyje8dc)

Поддерживаю предыдущего оратора - группы рулят ))

![rykalin.a profile image](https://band.wb.ru/api/v4/users/1x8pjr7h7tb6tdwcoimfqgak7o/image?_=1724872337750)

Александр Рыкалин

[02/28/2025 at 12:09 PM](https://band.wb.ru/wbcloud/pl/37pwax94k3nhbeoc4p3mgkn7se)

Геннадий Тарада

хм, admin и devops чем отличаются?

админ это пользовательская роль админа неймспейса, devops это собственно сервисные админы Edited

[12:12 PM](https://band.wb.ru/wbcloud/pl/p4m8iwn57i85zf81pqo9athmir)

там ещё в легенде написано, что devops, service выдется на уровне всего облака, а admin,editor,viewer на уровне неймспейса Edited

![rykalin.a profile image](https://band.wb.ru/api/v4/users/1x8pjr7h7tb6tdwcoimfqgak7o/image?_=1724872337750)

Александр Рыкалин

[02/28/2025 at 12:24 PM](https://band.wb.ru/wbcloud/pl/8pqto61yz3nudrnm99831jpato)

Евгений Малахов

> А возможно стоит заменить роли на группы. Группа объеденяет набор пермишенов (доступов). Пользователю можно задать несколько групп.

я бы сюда целился

Я что-то не понимаю чем, в этом контексте группа отличается от роли. Тем что роль на пользователя одна, а групп может быть несколько?

![sizov.aleksandr4 profile image](https://band.wb.ru/api/v4/users/6ppofrj1kj8wtm4nonuqpwp8fc/image?_=1738928085805)

Александр Сизов

[02/28/2025 at 12:24 PM](https://band.wb.ru/wbcloud/pl/4wz34z741pnepn83k3iu5ktash)

Да

![rykalin.a profile image](https://band.wb.ru/api/v4/users/1x8pjr7h7tb6tdwcoimfqgak7o/image?_=1724872337750)

Александр Рыкалин

[02/28/2025 at 12:33 PM](https://band.wb.ru/wbcloud/pl/tkwcsugy678sbmnzz4pw4znxaw)

Ну тогда надо либо прописать группы заранее, с наборами пермишенов. Либо сделать конструктор групп, что, конечно на порядок сложнее

[12:35 PM](https://band.wb.ru/wbcloud/pl/sek8eyntj7gqje4ec9smc3866e)

И надо ещё понять на каких уровнях действуют группы. Сейчас есть группы\роли уровня неймспейса и уровня облака. Возможно надо будет добавить ещё какие-то уровни, типа уровень pgaas

![sizov.aleksandr4 profile image](https://band.wb.ru/api/v4/users/6ppofrj1kj8wtm4nonuqpwp8fc/image?_=1738928085805)

Александр Сизов

[02/28/2025 at 12:42 PM](https://band.wb.ru/wbcloud/pl/4x45ywyptfnatpo4c8agbf4k6c)

Pgaas в рамках namespace-а. Если есть служебные методы, то они уже в рамках облака.

![rykalin.a profile image](https://band.wb.ru/api/v4/users/1x8pjr7h7tb6tdwcoimfqgak7o/image?_=1724872337750)

Александр Рыкалин

[03/04/2025 at 11:57 AM](https://band.wb.ru/wbcloud/pl/fhnx1iq49byuxn1jijiie9db8y)

@Евгений Малахов @Богдан Савин @Александр Сизов обновил таблицу, добавил creator. Для всех групп принадлжащих nemsapce сделал приставку ns_ для понятности. В начале описана легенда, какая группа чему соотвествует а потом Generalized table в которой как раз расписаны общие пользовательские и системные методы, которые я разделил для удобства. Уточнение по группам- на данный момент все групы неймспейса имеют те же права что прописаны в гейтвее - [https://gitlab-private.wildberries.ru/cloud/gateway-services/blob/master/pkg/gateway/options/predifined_roles.go#L25](https://gitlab-private.wildberries.ru/cloud/gateway-services/blob/master/pkg/gateway/options/predifined_roles.go#L25) То есть по сути это роли, но на уровне spicedb мы их реализуем как группы. В гейтвее также пондобиться преобразование т.к. сейчас, насколько я понимаю пользователю может быть назначена только одна какая-то роль @Андрей Мякоткин FYI Edited

![malahov.evgeniy9 profile image](https://band.wb.ru/api/v4/users/93qrfmbhabnm3e5ip1ifronfmw/image?_=1725015069761)

Евгений Малахов

[03/04/2025 at 12:04 PM](https://band.wb.ru/wbcloud/pl/p9zcskar6385bnb8g5qhqf5zmc)

> В гейтвее также пондобиться преобразование т.к. сейчас, насколько я понимаю пользователю может быть назначена только одна какая-то роль

это пока избыточно. Нам сначала надо будет гранулярные роли продумать, типа на ВМки, сети, кластера ПГ. До этого нет смысла делать, потому что:

1. viewer - самое дно, только смотрит
2. creator - тот же вьювер но с возможность создавать объекты
3. editor - creator с возможностью изменения и удаления
4. admin - может все, включая управление самим проектом

![rykalin.a profile image](https://band.wb.ru/api/v4/users/1x8pjr7h7tb6tdwcoimfqgak7o/image?_=1724872337750)

Александр Рыкалин

[03/04/2025 at 12:05 PM](https://band.wb.ru/wbcloud/pl/3c6k7ncxdfnyzdbqxu87nmkdhe)

Ок, давайте я тогда придумаю гранулярные группы, которые уже булут объединяться в роли гейтвея. А вы эти группы отревьюите

![malahov.evgeniy9 profile image](https://band.wb.ru/api/v4/users/93qrfmbhabnm3e5ip1ifronfmw/image?_=1725015069761)

Евгений Малахов

[03/04/2025 at 12:06 PM](https://band.wb.ru/wbcloud/pl/a3utrmkkcprujcj3ogspqumqyw)

я бы начал с админки тренироваться ) а гейт отложил до:

- обкатки групп на админке
- запуска текущего функционала спайсдб на проде

![rykalin.a profile image](https://band.wb.ru/api/v4/users/1x8pjr7h7tb6tdwcoimfqgak7o/image?_=1724872337750)

Александр Рыкалин

[03/04/2025 at 12:25 PM](https://band.wb.ru/wbcloud/pl/jn9y7pn74f8fxkw6tbfb5877gh)

А у нас админка разве планируется раньше в проде быть со spicedb чем гейтвей?

[12:29 PM](https://band.wb.ru/wbcloud/pl/ifwqobher3nbprfjdt15gmeyjh)

Просто чтобы нам вести разработку надо понимать, хотя бы в общих чертах какие группы будут везде. Дальше мы это можем проверять в песочнице и потихоньку внедрять, но полную таблицу мне бы хотелось иметь уже сейчас, чтобы в общем понимать куда двигать схему Edited

![sizov.aleksandr4 profile image](https://band.wb.ru/api/v4/users/6ppofrj1kj8wtm4nonuqpwp8fc/image?_=1738928085805)

Александр Сизов

[03/04/2025 at 12:50 PM](https://band.wb.ru/wbcloud/pl/pgdht76ykjya7f9we1yfy85ajc)

Если мы говорим об админке, то в ней есть раздел по управлению образами. И его права явно пересекается с пользовательской историей.

Т.е. на стороне адмики мы можемем проверить права. Ни их еще нужно проверить на стороне сервиса образов. А там есть и пользовательские запросы.

Я бы тоже начинал проработку прав с gateway.

![malahov.evgeniy9 profile image](https://band.wb.ru/api/v4/users/93qrfmbhabnm3e5ip1ifronfmw/image?_=1725015069761)

Евгений Малахов

[03/04/2025 at 12:54 PM](https://band.wb.ru/wbcloud/pl/hkesin3r6jgopqn9knmj8xcncy)

> А у нас админка разве планируется раньше в проде быть со spicedb чем гейтвей?

Я просто думаю, что админка будет раньше чем гейт с гранулярными правами (ролевкой)

[12:56 PM](https://band.wb.ru/wbcloud/pl/u1rakp1zwb8hzgf1dby7954o8o)

проработать можно, конечно, но смотрите не утоните в этом. Но как основу и заложиться на светлое будущее - вполне себе

[12:57 PM](https://band.wb.ru/wbcloud/pl/qfc9gaw9aina7xohit8npergec)

если про гейт говорить, то тут можно взять опыт яндекса, но на минималках [https://yandex.cloud/ru/docs/compute/security/?utm_referrer=https%3A%2F%2Fconsole.yandex.cloud%2Ffolders%2Fb1gvve6r8avts3nvj066%3Fsection%3Dresource-acl](https://yandex.cloud/ru/docs/compute/security/?utm_referrer=https%3A%2F%2Fconsole.yandex.cloud%2Ffolders%2Fb1gvve6r8avts3nvj066%3Fsection%3Dresource-acl)

[

Документация Yandex Cloud | Yandex Compute Cloud | Управление доступом в Compute Cloud

![Документация Yandex Cloud | Yandex Compute Cloud | Управление доступом в Compute Cloud](https://yandex.cloud/api/image-generator/generate?url=https:%2F%2Fyandex.cloud%2Fru%2Fimage-generator%2Fru%2Fdocs%2Fcompute%2Fsecurity&width=1280&height=640)



](https://yandex.cloud/ru/docs/compute/security/?utm_referrer=https%3A%2F%2Fconsole.yandex.cloud%2Ffolders%2Fb1gvve6r8avts3nvj066%3Fsection%3Dresource-acl "Документация Yandex Cloud | Yandex Compute Cloud | Управление доступом в Compute Cloud")

![savin.bogdan profile image](https://band.wb.ru/api/v4/users/hpk7iaeg5bbydg7qiehhqq76ze/image?_=1721897072240)

Богдан Савин

[03/04/2025 at 1:00 PM](https://band.wb.ru/wbcloud/pl/zin54jia8t8ofywnbc8bxto8gw)

Мне кажется для mvp надо сфотографироваться на админке, т.к. проработка пользовательской истории с гейтом добавит нам геморроя. Лучше постараться сфокусироваться на ролях админки с управлением образами, а группы и роли в specedb проработать уже потом. Сейчас мы переносили роли из облака as is, что, возможно, не оптимально.