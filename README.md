# REST-API TodoApp

«TodoApp» is a convenient REST API for providing data from the server to the user of a web application or website.

## Installation
To build TodoApp from source, simply run `git clone https://github.com/pyuldashev912/restapi-todoapp` and `cd` into the project source directory. Then run `make`. After this, you should have a binary called `todoapp` in the current directory.
```
$ make
go build -v ./cmd/todoapp
...
```
It is assumed that you are using PostgreSQL. Create a new database using `createdb todoapp`. Add the following application configurations to .env file into the project source directory:
```
BIND_ADDR = ":8080"
LOG_LEVEL = "info"
DATABASE_URL = "host=localhost dbname=todoapp sslmode=disable"
SESSION_KEY = "<generate session key>"
```
Launch the application
```
$ ./todoapp
INFO[0000] Listening...
```

# REST API Endpoints
## Create a user
### Request
`POST /user/create`
```
http POST localhost:8080/user/create name=name email=email password=password
```
### Response
```
{
    "id": int,
    "name": string,
    "email": string,
}
```
## Login
### Request
`POST /user/login`
```
http --session=user POST localhost:8080/user/login email=email password=password
```
### Response
```
{
    "info": string
}
```
## Logout
### Request
`POST /user/logout`
```
http --session=user POST localhost:8080/user/logout
```
### Response
```
{
    "info": string
}
```
## Who am I
### Request
`GET /user/whoami`
```
http --session=user GET localhost:8080/user/whoami
```
### Response
```
{
    "id": int,
    "name": string,
    "email": string,
}
```
## Create a task
### Request
`POST /task/add`
```
http --session=user POST localhost:8080/task/add title="Some task" description="Some text"
```
### Response
```
{
    "id": int,
    "title": string,
    "description": string,
    "done": bool,
    "creation_date": string
}
```
## Get task
### Request
`GET /task/get?id=<task_id>`
```
http --session=user GET localhost:8080/task/get?id=<task_id>"
```
### Response
```
{
    "id": int,
    "title": string,
    "description": string,
    "done": bool,
    "creation_date": string
}
```
## Get done/underdone tasks
### Request
`GET /task/getdone?done=true/false`
```
http --session=user GET localhost:8080/task/getdone?done=true"
```
### Response
```
[
    {
        "id": int,
        "title": string,
        "description": string,
        "done": bool,
        "creation_date": string
    }
    ...
]
```
## Get all tasks
### Request
`GET /task/getall`
```
http --session=user GET localhost:8080/task/getall"
```
### Response
```
[
    {
        "id": int,
        "title": string,
        "description": string,
        "done": bool,
        "creation_date": string
    }
    ...
]
```
## Mark the task as completed
### Request
`PATCH /task/done?id=<task_id>`
```
http --session=user PATCH localhost:8080/task/done?id=<task_id>"
```
### Response
```
{
    "info": string
}
```
## Delete the task
### Request
`DELETE /task/delete?id=<task_id>`
```
http --session=user delete localhost:8080/task/delete?id=<task_id>"
```
### Response
```
{
    "info": string
}
```