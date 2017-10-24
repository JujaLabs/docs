# Keepers Slack Bot
Keepers slack bot выполняет предобработку команд из slack чата, формирует валидные запросы в микросервис [Keepers](https://github.com/JujaLabs/docs/tree/master/architecture/keepers) для выполнения этих команд.
Также формирует ответ в slack чат об успешном/неуспешном выполнении команды.

## Описание slack slash command
Для нужд сервиса [Keepers](https://github.com/JujaLabs/docs/tree/master/architecture/keepers) мы используем возможности slack чата, а именно "slash command". Эти команды отличаются от обычных сообщений тем, что они начинаются с символа слеша '/', например /away. Команда должна быть одиночным словом и не может быть пустой. Также команды могут содержать параметры. Структура такой команды следующая: /command param1 param2. 
Slack чат позволяет создавать собственные команды, которые будут обращаться к соответствующим endpoint мкросервиса. Поддерживается метод POST.

## Пример запроса от slack чата
HTTP POST
Content-type = application/x-www-form-urlencoded
```
token=48aQhlnlyHNlMx5f23DGgzqH
team_id=T0001
team_domain=example
channel_id=C2147483705
channel_name=test
user_id=U2147483697
user_name=Steve
command=/weather
text=params
response_url=https://hooks.slack.com/commands/1234/5678
```

## Валидация запросов
Все запросы от slack чата валидируются SlackBot-ом по токену.

## Правила формирования ответа slack чату.
Пользователь должен получить ответ от SlackBot-а об успешном/неуспешном выполнении команды.
Все ответы должны иметь HTTP 200 "OK" статус код. Если мы хотим сообщить дополнительную информацию то должны ее отправить строкой.

Подробней про [slash commands](https://api.slack.com/slash-commands)

## Технические характеристики проекта:
* JDK 8
* SpringBOOT
* SpringMVC
* jUnit
* JBot Framework

## Список фичей

**SLB-F1 Я, как Хранитель, хочу добавить в систему нового Хранителя и сделать его активным**

Задачи слак бота:
* SLB-F1-D1 проверить token
* SLB-F1-D2 сохранить заявку сервисе Keepers
* SLB-F1-D3 передать сообщение пользователю об успешном/неуспешном выполнении команды
* SLB-F1-D4 @slack_nick_name - имя пользователя в системе Slack, всегда начинается с символа '@'
* SLB-F1-D5 direction - название направления Хранителя. Короткая фраза или слово описывающее направление, которым занимается Хранитель

* SLB-F1-CMD 
```
    /keeper-add @slack_nick_name direction 
```
* SLB-F1-URL
```
    url - commands/keeper/add
    method - POST
```

* SLB-F1-REQ
   ```
    Content-type = application/x-www-form-urlencoded
    
    token=...
    team_id=...
    team_domain=...
    channel_id=...
    channel_name=...
    user_id=...
    user_name=...
    command=...
    text=...
    response_url=...
    ```

* SLB-F1-RSP-OK
   ```
    {
    text: Спасибо мы добавили нового Хранителя @slack_nick_name по направлению {direction}
    }
    ```
* SLB-F1-RSP-ERR
   ```
    {
    text:{текст ошибки}.
    }
    ```
    
**SLB-F2 Я, как Хранитель, хочу сделать существующего Хранителя неактивным**   
 
Задачи слак бота:
* SLB-F2-D1 проверить token
* SLB-F2-D2 сохранить заявку сервисе Keepers
* SLB-F2-D3 передать сообщение пользователю об успешном/неуспешном выполнении команды
* SLB-F2-D4 @slack_nick_name - имя пользователя в системе Slack, всегда начинается с символа '@'
* SLB-F4-D5 direction - название направления Хранителя. Короткая фраза или слово описывающее направление, которым занимается Хранитель

    
* SLB-F2-CMD 
```
    /keeper-dismiss @slack_nick_name direction
```
* SLB-F2-URL
```
    url - commands/keeper/dismiss
    method - POST
```
* SLB-F2-REQ
   ```
    Content-type = application/x-www-form-urlencoded
    
    token=...
    team_id=...
    team_domain=...
    channel_id=...
    channel_name=...
    user_id=...
    user_name=...
    command=...
    text=...
    response_url=...
    ```

* SLB-F2-RSP-OK
   ```
    {
    text: Хранитель @slack_nick_name по направлению {direction} сделан неактивным
    }
    ```
    
* SLB-F2-RSP-ERR
   ```
    {
    text:{текст ошибки}.
    }
    ```
    
**SLB-F3 Я, как Хранитель, хочу получить список всех направлений, которые закреплены за активным Хранителем, отличным
 от меня**

Задачи слак бота:
* SLB-F3-D1 проверить token
* SLB-F3-D2 получить данные из сервиса Keepers
* SLB-F3-D3 Вывести полученные данные пользователю
* SLB-F3-D4 В случае отсутствия закрепленных направлений вывести пользователю сообщение, что за указанным @slack_nick_name нет закрепленных направлений.
* SLB-F3-D5 @slack_nick_name - имя пользователя в системе Slack, всегда начинается с символа '@'
* SLB-F3-D6 direction - название направления Хранителя. Короткая фраза или слово описывающее направление, которым занимается Хранитель
    
* SLB-F3-CMD 
```
    /keeper @slack_nick_name
```
* SLB-F3-URL
```
    url - commands/keeper
    method - POST
```
* SLB-F3-REQ
   ```    
    token=...
    team_id=...
    team_domain=...
    channel_id=...
    channel_name=...
    user_id=...
    user_name=...
    command=...
    text=...
    response_url=...
    ```

* SLB-F3-RSP-OK
   ```
    {
    text: Закрепленные направления за Хранителем @slack_nick_name : {direction}, {direction}
    }
    ```
* SLB-F3-RSP-EMPTY
    ```
    {
    text: За Хранителем @slack_nick_name сейчас не закреплено ни одно направление
    }
    ```
    
* SLB-F3-RSP-ERR
   ```
    {
    text:{текст ошибки}.
    }
    ```

**SLB-F4 Я, как Хранитель, хочу получить список всех направлений, которые закреплены за мной**

Задачи слак бота:
* SLB-F4-D1 проверить token
* SLB-F4-D2 получить данные из сервиса Keepers
* SLB-F4-D3 Вывести полученные данные пользователю
* SLB-F4-D4 В случае отсутствия закрепленных направлений вывести пользователю сообщение, что за ним нет закрепленных 
направлений.
* SLB-F4-D5 @slack_nick_name - имя пользователя в системе Slack, всегда начинается с символа '@'
* SLB-F4-D6 direction - название направления Хранителя. Короткая фраза или слово описывающее направление, которым 
занимается Хранитель
    
* SLB-F4-CMD 
```
    /my-directions
```
* SLB-F4-URL
```
    url - commands/myDirections
    method - POST
```
* SLB-F4-REQ
   ```    
    token=...
    team_id=...
    team_domain=...
    channel_id=...
    channel_name=...
    user_id=...
    user_name=...
    command=...
    text=...
    response_url=...
    ```

* SLB-F4-RSP-OK
   ```
    {
    text: Закрепленные направления за Хранителем @slack_nick_name : {direction}, {direction}
    }
    ```
* SLB-F4-RSP-EMPTY
    ```
    {
    text: За Хранителем @slack_nick_name сейчас не закреплено ни одно направление
    }
    ```
    
* SLB-F4-RSP-ERR
   ```
    {
    text:{текст ошибки}.
    }
    ```