# User service

The service provides the data about course students to users (other services).

Student's data is stored in the PostgreSQL database. The records in the database are synchronized with the source CRM on schedule once per day.
The service provides users a limited RESTful API with ability to read records only but not to add, update and delete them.

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

* USR-D2 The service stores limited information about students that is necessary for the functioning of other services. All operations are logged according to the established level of logging.

* USR-D3 When an error occurs during user's request user should receive sufficient for understanding and correcting information about this error such as: incorrectly set of parameters, absence of mandatory parameters, format parameter mismatch, lack of data etc.

## Features:

### USR-F1 As a user I want to get a list of all students.

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

### USR-F2  As a user I want to get list of students by their uuid

* USR-F2-D1 Input - an uuid array, output - an array of students with the uuid, slack, skype and name fields.

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

### USR-F3 As a user I want to get list of students by their slack name.

* USR-F3-D1 Input - a slackNames array, output - an array of students with the uuid, slack, skype and name fields.

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