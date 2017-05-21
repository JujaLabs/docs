Для нужд геймификации мы используем возможности slack чата, а именно "slash command". Эти команды отличаются от обычных сообщений тем, что они начинаются со слеша (/). К примеру /away. комманда олжна быть одиночным словом и не может быть пустой. Также команды могут содержать параметры. Cтруктура такой команды следующая /command param1 param2. 
Slack чат позволяет создавать собственные команды которые будут дергать нужные нам ендпоинты. Поддерживается только метод POST.
Пример параметров которые отправляются slack-ом
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
Когда сервер получает такой запрос он может его провалидировать по token и дополнительно по team_id.
После обработки комманды на сервере мы можем отправить ответ обратно в slack. Для того чтобы использовать стандартное форматирование возвращается просто строка. Также мы можем кастомизировать ответ подробней https://api.slack.com/docs/message-formatting
все ответы должны иметь HTTP 200 "OK" статус код. В противном случае формируется ошибка.
Подробней про slash commands
https://api.slack.com/slash-commands

Технические характеристики проекта:
JDK 8
SpringBOOT
SpringMVC
jUnit
JBot Framework

Список фичей
### SLB-F15
***SLB-F15 Я как пользователь хочу иметь возможность получить +6 джуджиков за работу в связке. Связка это команда из четырех участников, к которым можно обратиться за помощью, а также она помогает не "отвалится" от своей первоначальной цели. Формирование команды происходит рандомно. 
В конце недели, в субботу, команда должна принять самостоятельное честное решение, соблюдала ли она условия связки и выставить себе +6 джуджиков.***
* [дока по связкам](https://docs.google.com/document/d/190xabylJ7VW00jbxe5CHI_mns9QrGXL_2KHJeJzO6Mo/edit)

* SLB-F15-D1 необходимо проверить token
* SLB-F15-D2 все передаваемые slackName в геймификацию необходимо конвертировать в uuid.
* SLB-F15-D3 номер связки в поле text


* SLB-F15-URL
    ```
    url - "/commands/achieve/team"
    method - POST
    ```
* SLB-F15-REQ
    ```
    token=ly5f23DGgzqHaNlMx48HlnQh
    team_id=T0001
    team_domain=example
    channel_id=C2147483705
    channel_name=test
    user_id=U2147483697
    user_name=Steve
    command=/weather
    text={teamId}
    response_url=https://hooks.slack.com/commands/1234/5678
    ```
* SLB-F15-RSP ["..."] - массив id сохраненных ачивок.
* SLB-F15-S1-REQ ``/team {teamId}``
* SLB-F15-S2-RSP Спасибо, ваша работа в команде учтена.
* SLB-F15-S3-RSP Текст ошибки.
