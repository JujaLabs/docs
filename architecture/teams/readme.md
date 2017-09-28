# Teams
JuJa Team consists of four users with various experience in Java. Goal is in mutual support in study process to the
end of the course. 

## Technical characteristics of the project:

* JDK8
* Mongo
* SpringBoot
* SpringData
* SpringMVC
* jUnit
* FakeMongo 
* LordOfTheJars

## Common:
**TMF-D1** Keeper - special type of user, which helps to develop course and help other users.

**TMF-D2** The Team consists of four users. Every user can be only in one Team.

**TMF-D3** Participants are selected manually.

**TMF-D4** If the activity is absent Team must be deactivated. Max team lifetime is one month.

**TMF-D5** Successfull operation on Team in void methods sends the response with HTTP status 200 OK.  

### TMF-F1
***TMF-F1 As a keeper I want to activate new Teams that consist of four users***

* TMF-F1-D1 The operation is available only for Keeper of Teams Direction (TMF-D1).
* TMF-F1-D2 Keeper points four users for future Team.
* TMF-F1-D3 It's necessary to check if the users are already in another team.
* TMF-F1-D4 Number of the Team is incremental number from Database.
* TMF-F1-D5 Result of the command is the Team entity

* TMF-F1-URL
```
    url - "/teams"
    method - POST
```
* TMF-F1-REQ
```
{
  "from": "uuid-from",
  "members": [
    "uuid1",
    "uuid2",
    "uuid3",
    "uuid4"
  ]
}
```
* TMF-F1-RSP - Activated Team entity
```
   {
     "id": .... ,
     "from":....,
     "members":[.... , .... , .... , ....],
     "activateDate": .... ,
     "deactivateDate": ....
   }  
```
### TMF-F2
***TMF-F2 As a keeper I want to deactivate Teams.***

* TMF-F2-D1 The operation is available only for Keeper of Teams Direction (TMF-D1).
* TMF-F2-D2 To deactivate Team - the keeper points uuid of any member of Team, he wants to deactivate.
* TMF-F2-D3 Result of the command is the Team entity with updated deactivate date.

* TMF-F2-URL
```
    url - "/teams"
    method - PUT
```
* TMF-F2-REQ
```
    {
        "from" : "uuid-from",
        "uuid" : "uuid1"
    }
```
* TMF-F2-RSP - Deactivated Team entity
```
   {
     "id": .... ,
     "from":....,
     "members":[.... , .... , .... , ....],
     "activateDate": .... ,
     "deactivateDate": ....
   }  
```
### TMF-F3
***TMF-F3 As a user I want to get all active Teams.***

* TMF-F3-D1 The operation is available for any user.
* TMF-F3-D2 Result of the command is list of active Teams entities.

* TMF-F3-URL
```
    url - "/teams"
    method - GET
```
* TMF-F3-REQ
```
   {}
```
* TMF-F3-RSP
```
    [
        {
            "id": .... ,
            "from":....,            
            "members":[.... , .... , .... , ....],
            "activateDate": .... ,
            "deactivateDate": ....
        },
        {
            ....
        },
        ....   
    ]
```
### TMF-F4
***TMF-F4 As a user I want to get active Team of certain user.***

* TMF-F4-D1 The operation is available for any user.
* TMF-F4-D2 Result of the command is the Team entity.  
* TMF-F4-D3 The user points uuid of any member of Team, he wants to get.

* TMF-F4-URL
```
    url - "/teams/users/{uuid}
    method - GET
```
* TMF-F4-REQ
```
    {}
```
* TMF-F4-RSP
```
   {
     "id": .... ,
     "from":....,     
     "members":[.... , .... , .... , ....],
     "activateDate": .... ,
     "deactivateDate": ....
   }  
   
 ```
