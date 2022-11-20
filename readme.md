## Пример по курсу Архитектура Программных систем


Запускаем в docker-compose кластер Apache Ignite:

- для архитектуры x86 используем образ apacheignite/ignite
- для архитектуры arm applem1support/ignite:2.12.0

```plantuml
@startuml

component browser
component hl_mai_06_cache
database my_sql
database "ignite-node-1"
database "ignite-node-2"

browser -d-> hl_mai_06_cache : http://localhost/author?id=some_id
hl_mai_06_cache -d-> "ignite-node-1" : get key [tcp:10900]
hl_mai_06_cache -d-> "ignite-node-2" : get key [tcp:10900]
hl_mai_06_cache -d-> my_sql : select * from author where id=some_id [tcp:3306]
hl_mai_06_cache -d-> "ignite-node-1" : put key [tcp:10900]
hl_mai_06_cache -d-> "ignite-node-2" : put key [tcp:10900]


@enduml
```

Тестовый запрос для добавления в MySQL и кэш:

http://172.16.191.128/author?add&first_name=*Vasia*&last_name=*P**upkin*&title=*Mr*&email=*vasia@pupkin.com*

Тестовый запрос для поиска в кэше:

http://*localhost*/author?id=*some_id -* поиск записей (шаблон сквозное-чтение)
