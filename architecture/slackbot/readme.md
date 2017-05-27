# SlackBot
SclackBot выполняет предобработку команд из slack чата. Проверяет, обрабатывает переданные данные из slack чата и формирует правильные запросы в другие микросервисы для успешного выполнения этой команды. Также формирует ответ в slack чат об успешном/неуспешном выполнении команды.
## Описание slack slash command
Для нужд геймификации мы используем возможности slack чата, а именно "slash command". Эти команды отличаются от обычных сообщений тем, что они начинаются со слеша (/). К примеру /away. комманда должна быть одиночным словом и не может быть пустой. Также команды могут содержать параметры. Cтруктура такой команды следующая /command param1 param2. 
Slack чат позволяет создавать собственные команды которые будут дергать нужные нам ендпоинты. Поддерживается только метод POST.
## Пример запроса от slack чата
HTTP POST
Content-type = application/x-www-form-urlencoded
```
token=ly5f23DGgzqHaNlMx48HlnQh
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

Подробней про slash commands
https://api.slack.com/slash-commands

## Технические характеристики проекта:
* JDK 8
* SpringBOOT
* SpringMVC
* jUnit
* JBot Framework

## Список фичей

### SLB-F1
***Я как пользователь хочу получить джуджики за [дейли](http://bit.ly/juja-ABD4536) репорт***

Задачи слак бота:
* SLB-F1-D1 проверить token
* SLB-F1-D2 сохранить ачивку
* SLB-F1-D3 передать сообщение пользователю об успешном/неуспешном выполнении команды

* SLB-F1-CMD 
```
    /daily description
```

* SLB-F1-URL
```
    url - commands/daily
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
    text:Спасибо, ваш дейлик принят.
    }
    ```
* SLB-F1-RSP-ERR
   ```
    {
    text:{текст ошибки}.
    }
    ```
    
### SLB-F2
***Я как пользователь хочу иметь возможность поблагадарить другого пользователя джуджиком. Эта ачивка называется "спасибка"***
    
    Задачи слак бота:
* SLB-F2-D1 проверить token
* SLB-F2-D2 сохранить ачивку
* SLB-F2-D3 передать сообщение пользователю об успешном/неуспешном выполнении команды

*SLB-F2-CMD 
```
    /thanks @slack_name description
```

* SLB-F2-URL
```
    url - commands/thanks
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
    text:Спасибо, ваша спасибка для {@slack_nick_name} принята.
    }
    ```
* SLB-F2-RSP-ERR
   ```
    {
    text:{текст ошибки}.
    }
    ```
    
### SLB-F3
***Я как пользователь хочу иметь возможность отметить победителей турнира codenjoy"***
    
    Задачи слак бота:
* SLB-F3-D1 проверить token
* SLB-F3-D2 сохранить ачивку
* SLB-F3-D3 передать сообщение пользователю об успешном/неуспешном выполнении команды

* SLB-F3-CMD 
```
    /codenjoy -1th @slack_name1 -2th @slack_name2 -3th @slack_name3
```

* SLB-F3-URL
```
    url - commands/codenjoy
    method - POST
```
* SLB-F3-REQ
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
* SLB-F3-RSP-OK
   ```
    {
    text:Спасибо, мы поблагодарили всех участников: {@slack_nick_name1}, {@slack_nick_name2}, {@slack_nick_name3}.
    }
    ```
* SLB-F3-RSP-ERR
   ```
    {
    text:{текст ошибки}.
    }
    ```
    
### SLB-F4
***Я как пользователь хочу иметь возможность получить джуджики за пройденное интервью"***
    
    Задачи слак бота:
* SLB-F4-D1 проверить token
* SLB-F4-D2 сохранить ачивку
* SLB-F4-D3 передать сообщение пользователю об успешном/неуспешном выполнении команды

* SLB-F4-CMD 
```
    /interview description
```

* SLB-F4-URL
```
    url - commands/interview
    method - POST
```
* SLB-F4-REQ
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
* SLB-F4-RSP-OK
   ```
    {
    text:Спасибо, вы получили джуджики за пройденное интервью.
    }
    ```
* SLB-F4-RSP-ERR
   ```
    {
    text:{текст ошибки}.
    }
    ```
    
### SLB-F5
***SLB-F5 Я как пользователь хочу иметь возможность получить джуджики за работу в связке.***
* [дока по связкам](https://docs.google.com/document/d/190xabylJ7VW00jbxe5CHI_mns9QrGXL_2KHJeJzO6Mo/edit)

 Задачи слак бота:
*SLB-F5-D1 проверить token
*SLB-F5-D2 сохранить ачивку
*SLB-F5-D3 передать сообщение пользователю об успешном/неуспешном выполнении команды

*SLB-F5-CMD 
```
    /team
```

* SLB-F5-URL
    ```
    url - "/commands/team"
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
    command=/team
    text={teamId}
    response_url=...
    ```
    
*SLB-F5-RSP-OK
   ```
    {
    text:Спасибо, вы получили джуджики за участие в комманде.
    }
    ```
*SLB-F5-RSP-ERR
   ```
    {
    text:{текст ошибки}.
    }
    ```
