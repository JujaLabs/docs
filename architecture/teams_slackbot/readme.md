# Teams Slackbot
Teams slack bot performs processing of commands from slack chat. If commands and token are valid Slack bot generates requests to 
Teams microservice [Teams](https://github.com/JujaLabs/docs/tree/master/architecture/teams).
It generates response to slack in case of successful command execution or error.

## Description of slack slash command
For the [Teams](https://github.com/JujaLabs/docs/tree/master/architecture/teams) purposes we use slack chat ability. 
It calls "slash command". This commands starts with "/" symbol ( "/away" for example). 
Command must be one not empty word. It can have parameters ("/command param1 param2" for example). 
Slack lets developers to create commands, which refer to any endpoint of microservices. Slash
commands support only POST method.

## Slack chat request example
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

## Request validation
All requests from slack are validated by unique token.

## Rules of response to slack chat.
User must receive the response from slackbot in case of successful command execution or error.
Slack chat has 3000 ms time out for response. That's why Slackbot sends two responses to Slack: Instant and Delayed.
All responses must contain HTTP 200 "OK" status code. All additional information must be in text line.

Read more  about [slash commands](https://api.slack.com/slash-commands)

## Technical characteristics of the project:
* JDK 8
* SpringBOOT
* SpringMVC
* jUnit
* JBot Framework

## Abilities:
***SLB-F1 As a keeper I want to activate new Teams, consist of four users.***

Slackbot tasks:
* SLB-F1-D1 Validate token;
* SLB-F1-D2 Send request to Teams Service to save new Team and receive response (Team entity);
* SLB-F1-D3 Send response to slack user in case of successful command execution or error;
* SLB-F1-D4 @slack_name1, @slack_name2, @slack_name3, @slack_name4 - Slack user names, always 
starts with '@' symbol;
* SLB-F1-D5 Every user can only be in one Team.

* SLB-F1-CMD 
```
    /teams-activate @slack_name_1 @slack_name_2 @slack_name_3 @slack_name_4  
```
* SLB-F1-URL
```
    url - commands/teams/activate
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

* SLB-F1-RSP-INS-OK
   ```
    {
    text: Thanks, Activate Team job started!
    }
   ```
* SLB-F1-RSP-DEL-OK
   ```
    {
    text: Thanks, new Team for '@slack_name1 @slack_name2 @slack_name3 @slack_name4' activated!
    }
   ```
* SLB-F1-RSP-ERR
   ```
    {
    text:{error message}.
    }
   ```
    
**SLB-F2 As a keeper I want to deactivate Teams.**   
 
Slackbot tasks:
* SLB-F2-D1 Validate token;
* SLB-F2-D2 Send request to Teams Service to deactivate Team of user and receive response (Team entity);
* SLB-F2-D3 Send response to slack user in case of successful command execution or error;
* SLB-F2-D4 @slack_name - user's name in Slack, which Team we want to deactivate. Always starts with "@".
* SLB-F2-D5 @slack_name1, @slack_name2, @slack_name3, @slack_name4 - Slack user names, always 
starts with '@' symbol;
 
* SLB-F2-CMD 
```
    /teams-deactivate @slack_name
```
* SLB-F2-URL
```
    url - commands/teams/deactivate
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

* SLB-F2-RSP-INS-OK
   ```
    {
    text: Thanks, Deactivate Team for user '@slack_name' job started!
    }
   ```
* SLB-F2-RSP-DEL-OK
   ```
    {
    text: Thanks, Team '@slack_name1 @slack_name2 @slack_name3 @slack_name4' deactivated!
    }
   ```

* SLB-F2-RSP-EMPTY
    ```
    {
    text: You cannot get/deactivate team if the user not a member of any team!
    }
    ```
       
* SLB-F2-RSP-ERR
   ```
    {
    text:{error message}.
    }
    ```
    
**SLB-F3 As a user I want to get members of active Team of certain user.**

Slackbot tasks:
* SLB-F3-D1 Validate token;
* SLB-F3-D2 Send request to Teams Service to get Team of user and receive response (Team entity);
* SLB-F3-D3 Send response to slack user in case of successful command execution or error;
* SLB-F3-D4 @slack_name - user's name in Slack, which Team we want to get. Always starts with "@".
* SLB-F3-D5 @slack_name1, @slack_name2, @slack_name3, @slack_name4 - Slack user names, always 
starts with '@' symbol;
    
* SLB-F3-CMD 
```
    /teams @slack_name
```
* SLB-F3-URL
```
    url - commands/teams
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

* SLB-F3-RSP-INS-OK
   ```
    {
    text: Thanks, Get Team for user '@slack_name' job started!
    }
   ```
* SLB-F3-RSP-DEL-OK
   ```
    {
    text:Thanks, Team for '@slack_name' is '@slack_name1 @slack_name2 @slack_name3 @slack_name4'!
    }
   ```
* SLB-F3-RSP-EMPTY
    ```
    {
    text: You cannot get/deactivate team if the user not a member of any team!
    }
    ```
* SLB-F3-RSP-ERR
   ```
    {
    text:{error message}.
    }
    ```

 **SLB-F4 As a user I want to get members of my active Team.**
 
 Slackbot tasks:
 * SLB-F4-D1 Validate token;
 * SLB-F4-D2 Send request to Teams Service to get my Team and receive response (Team entity);
 * SLB-F4-D3 Send response to slack user in case of successful command execution or error;
 * SLB-F4-D4 @slack_name1, @slack_name2, @slack_name3, @slack_name4 - Slack user names, always 
 starts with '@' symbol;
     
 * SLB-F4-CMD 
 ```
     /myteam
 ```
 * SLB-F4-URL
 ```
     url - commands/myteam
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
 
* SLB-F4-RSP-INS-OK
   ```
    {
    text: Thanks, Get My Team for user '@slack_name' job started!
    }
   ```
* SLB-F4-RSP-DEL-OK
   ```
    {
    text:Thanks, Team for '@slack_name' is '@slack_name1 @slack_name2 @slack_name3 @slack_name4'!
    }
   ```

 * SLB-F4-RSP-EMPTY
     ```
     {
    text: You cannot get/deactivate team if the user not a member of any team!
     }
     ```
     
 * SLB-F4-RSP-ERR
    ```
     {
    text:{error message}.
     }
     ```    
     
