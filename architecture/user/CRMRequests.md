## Выбрать всех студентов:
 
`GET http://123.123.123.123/x2engine/index.php/api2/Contacts/?c_isStudent=1`
 
Результат (если нужны поля email, skype и т.д. опущены):
```
[
{
"c_slack": "@stepan",
 	"c_isStudent": "1",
 	"id": "337",
 	"name": "Стивен <stephen@gmail.Com>", 
"c_uuid": "123e4567-e89b-12d3-a456-426655440000"
},
{
"c_slack": "@viktor",
 "c_isStudent": "1",
 "id": "341",
 "name": "Виктор Пупкин",
 "c_uuid": "123e4567-e89b-12d3-a456-426655440023"
},
{
"c_slack": "@den", 
"c_isStudent": "1", 
"id": "943", 
"name": "Denis Tantsev",
 "c_uuid": "8a1258b1-7504-4324-95ee-b0fc2a610bd0"
}
]
 ```
 
## Выбрать студента по UUID:
 
`GET http://123.123.123.123/x2engine/index.php/api2/Contacts/?c_isStudent=1&c_uuid=123e4567-e89b-12d3-a456-426655440000'`
 
Результат:
 ```
[
{
"c_slack": "@stepan",
 "c_isStudent": "1",
 "id": "337", 
"name": "Стивен <stephen@gmail.Com>", 
"c_uuid": "123e4567-e89b-12d3-a456-426655440000"
}
]
 ```
 
## Выбрать студента по Slack:
 
`GET http://123.123.123.123/x2engine/index.php/api2/Contacts/?c_isStudent=1&c_slack=@den'`
 
Результат:
 ```
{
"c_slack": "@den", 
"c_isStudent": "1",
 "id": "943",
 "name": "Denis Tantsev", 
"c_uuid": "8a1258b1-7504-4324-95ee-b0fc2a610bd0"
}
 ```
 
## Выборка всех хранителей:
 
`GET http://123.123.123.123/x2engine/index.php/api2/Keepers/?c_isActive=1`
 
Результат:
```
[
{
"c_description": "Назначаю сего товарища хранителем ля-ля-ля и вот ему за это", 
"c_isActive": "1", 
"c_from": "Big Boss",
 "c_contact": "Denis Tantsev_943"
},
{
"c_description": "А этот господин будет хранителем ту-ту-ту", 
"c_isActive": "1", 
"c_from": "Big Boss", 
"c_contact": "Стивен <stephen@gmail.Com>_337"
}
]
 ```
c_contact состоит из: Contact.name_Contact.id. Например: contact.name = “Denis Tantsev” contact.id=943, тогда c_contact=Denis Tantsev_943
 
Чтобы найти UUID хранителя по c_contact: выделить Contact.id из c_contact и выполнить:
 
`GET http://123.123.123.123/x2engine/index.php/api2/Contacts/?c_isStudent=1&id=943`
 
Результат:
 ```
[
{
"c_slack": "@den", 
"c_isStudent": "1", 
"id": "943", 
"name": "Denis Tantsev",
 "c_uuid": "8a1258b1-7504-4324-95ee-b0fc2a610bd0"
}
]
 ```
Соответственно и в обратную сторону - чтобы найти записи в таблице Keepers, соответствующие пользователю с UUID, сначала получаем по UUID его Contact, склеиваем c_contact=<name>_<id>, и делаем запрос к Keepers:
 
`GET http://123.123.123.123/x2engine/index.php/api2/Keepers/?c_isActive=1&c_contact=Denis Tantsev_943`
