# Teams Slack Bot
Teams slack bot выполняет предобработку команд из slack чата, формирует валидные запросы в микросервис [Teams](https://github.com/JujaLabs/docs/tree/master/architecture/teams) для выполнения этих команд.
Также формирует ответ в slack чат об успешном/неуспешном выполнении команды.

## Описание slack slash command
Для нужд сервиса [Teams](https://github.com/JujaLabs/docs/tree/master/architecture/teams) мы используем возможности slack чата, а именно "slash command". Эти команды отличаются от обычных сообщений тем, что они начинаются с символа слеша '/', например /away. Команда должна быть одиночным словом и не может быть пустой. Также команды могут содержать параметры. Структура такой команды следующая: /command param1 param2. 
Slack чат позволяет создавать собственные команды, которые будут обращаться к соответствующим endpoint мкросервиса. Поддерживаются методы POST или GET.

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
В случае GET запроса заголовок Content-type не устанавливается.

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

**SLB-F1 Я как хранитель хочу иметь возможность создавать связки. Связка это команда состоящая из четырех человек.**

Задачи слак бота:
* SLB-F1-D1 проверить token
* SLB-F1-D2 сохранить заявку в сервисе Teams
* SLB-F1-D3 передать сообщение пользователю об успешном/неуспешном выполнении команды
* SLB-F1-D4 @slack_nick_name1, @slack_nick_name2, @slack_nick_name3, @slack_nick_name4 - - имена пользователей в системе Slack, всегда начинаются с символа '@'
* SLB-F1-D5 один человек может состоять только в одной активной команде

* SLB-F1-CMD 
```
    /teams-add @slack_nick_name_1 @slack_nick_name_2 @slack_nick_name_3 @slack_nick_name_4  
```
* SLB-F1-URL
```
    url - commands/teams
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
    text: Спасибо. Мы добавили новую команду 
    @slack_nick_name_1 @slack_nick_name_2 @slack_nick_name_3 @slack_nick_name_4
    }
    ```
* SLB-F1-RSP-ERR
   ```
    {
    text:{текст ошибки}.
    }
    ```
    
**SLB-F2 Я как хранитель хочу иметь возможность расформировывать связки.**   
 
Задачи слак бота:
* SLB-F2-D1 проверить token
* SLB-F2-D2 сохранить заявку сервисе Teams
* SLB-F2-D3 передать сообщение пользователю об успешном/неуспешном выполнении команды
* SLB-F2-D4 team_id - id команды, которую нужно расформировать

    
* SLB-F2-CMD 
```
    /teams-dismiss team_id
```
* SLB-F2-URL
```
    url - commands/teams/dismiss
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
    text: Команда team_id расформирована.
    }
    ```

* SLB-F2-RSP-EMPTY
    ```
    {
    text: Команды с team_id не существует.
    }
    ```
    
    
* SLB-F2-RSP-ERR
   ```
    {
    text:{текст ошибки}.
    }
    ```
    
**SLB-F3 Я как пользователь хочу иметь возможность получить список всех связок с id и участниками.**

Задачи слак бота:
* SLB-F3-D1 проверить token
* SLB-F3-D2 получить данные из сервиса Teams
* SLB-F3-D3 Вывести полученные данные пользователю
* SLB-F3-D4 @slack_nick_name1, @slack_nick_name2, @slack_nick_name3, @slack_nick_name4 - - имена пользователей в системе Slack, всегда начинаются с символа '@'
* SLB-F3-D5 team_id - id активной команды

* SLB-F3-CMD 
```
    /teams
```
* SLB-F3-URL
```
    url - commands/teams
    method - GET
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
    text: Активные команды: [{id: team_id, members: [@slack_nick_name_1, 
    @slack_nick_name_2, @slack_nick_name_3, @slack_nick_name_4]}, ...]
    }
    ```
* SLB-F3-RSP-EMPTY
    ```
    {
    text: Активные команды отсутствуют
    }
    ```
    
* SLB-F3-RSP-ERR
   ```
    {
    text:{текст ошибки}.
    }
    ```
    
**SLB-F4 Я как пользователь хочу иметь возможность получить список всех участников определенной связки.**

Задачи слак бота:
* SLB-F4-D1 проверить token
* SLB-F4-D2 получить данные из сервиса Teams
* SLB-F4-D3 Вывести полученные данные пользователю
* SLB-F4-D4 team_id - id команды
* SLB-F4-D5 @slack_nick_name1...4 - имена четырех пользователя в системе Slack, всегда начинаются с символа '@'
    
* SLB-F4-CMD 
```
    /teams team_id
```
* SLB-F4-URL
```
    url - commands/team
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
    text:
    Команда: team_id. 
    Участники: 
    @slack_nick_name_1, @slack_nick_name_2, @slack_nick_name_3, @slack_nick_name_4]
    }
    ```
* SLB-F4-RSP-EMPTY
    ```
    {
    text: Команда с team_id отсутствует
    }
    ```
    
* SLB-F4-RSP-ERR
   ```
    {
    text:{текст ошибки}.
    }
    ```
    
**SLB-F5 Я как пользователь хочу иметь возможность получить список всех участников своей активной связки**

Задачи слак бота:
* SLB-F5-D1 проверить token
* SLB-F5-D2 получить данные из сервиса Teams
* SLB-F5-D3 Вывести полученные данные пользователю
* SLB-F5-D4 team_id - id команды
* SLB-F5-D5 @slack_nick_name1...4 - имена четырех пользователя в системе Slack, всегда начинаются с символа '@'
    
* SLB-F5-CMD 
```
    /myteam
```
* SLB-F5-URL
```
    url - commands/myteam
    method - POST
```
* SLB-F5-REQ
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

* SLB-F5-RSP-OK
   ```
    {
    text:
    Команда: team_id. 
    Участники: 
    @slack_nick_name_1, @slack_nick_name_2, @slack_nick_name_3, @slack_nick_name_4]
    }
    ```
* SLB-F5-RSP-EMPTY
    ```
    {
    text: Вы не состоите ни в какой команде
    }
    ```
    
* SLB-F5-RSP-ERR
   ```
    {
    text:{текст ошибки}.
    }
    ```    