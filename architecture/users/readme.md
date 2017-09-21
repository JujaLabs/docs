# User service

The service provides the data about course students for users (other services).

Student's data is stored in the PostgreSQL database. The records in the database are synchronized daily with the source CRM on a schedule.
The service provides users with a limited RESTful API without the ability to delete records.

## Technical characteristics of the project:

* JDK 8
* Spring Boot
* Spring Data JPA
* JUnit
* Spring Test DBUnit
* Gradle
* Logging


## Generals:

* USR-D1 The service receives an incoming request on its URI. Request's parameters are checked for correctness and absence of any unnecessary data (SQL injection, etc.) and a query is performed to the data layer (database)

* USR-D2 The service stores limited information about students, necessary for the functioning of other services. All operations are logged in accordance with the established level of logging.

* USR-D3 The user should be provided with information about errors when executing his request, sufficient for understanding and correcting the error (incorrectly set parameters, absence of mandatory parameter, format parameter mismatch, lack of data on its request, etc).

## Features:

### USR-F1 I, as a user, want to be able to get a list of all students.

* USR-F1-D1 Student information consists of uuid, slack, skype and name

* USR-F1-URL

    ```
    url - "/v1/users"
    method - GET
    ```

* USR-F1-REQ

* USR-F1-RSP

    ```
    [
        {
            "uuid" : "...",
            "slack" : "...",
            "skype" : "...",
            "name" : "..."
        },
        ...
    ]
    ```

### USR-F2 I, as a user, want to be able to get list of students on their uuid

* USR-F2-D1 At the input there is an uuid array, at the output there is an array of students with the uuid, slack, skype and name fields.

* USR-F2-URL

    ```
    url - "/v1/users/usersByUuids"
    method - POST
    ```

* USR-F2-REQ

    ```
    {"uuids":["...","...", ...]}"
    ```

* USR-F2-RSP

    ```
    [
        {
            "uuid" : "...",
            "slack" : "...",
            "skype" : "...",
            "name" : "..."
        },
        ...
    ]
    ```

### USR-F3 I, as a user, want to be able to get list of students on their slack name.

* USR-F3-D1 At the input there is a slackNames array, at the output there is an array of students with the uuid, slack, skype and name fields.

* USR-F3-URL

    ```
    url - "/v1/users"/usersBySlackNames""

    method - POST
    ```

* USR-F3-REQ

    ```
    {"slackNames":["...","...", ...]}"
    ```

* USR-F3-RSP

    ```
    [
        {
            "uuid" : "...",
            "slack" : "...",
            "skype" : "...",
            "name" : "..."
        },
        ...
    ]
    ```