# How to set up a initial database, user and password in Postgres and Docker Compose

In this post I will show you how to set-up a initial database, user and password in Postgres and Docker Compose.

Maybe you are getting an error trying to connect to the database running in the container or maybe you are wondering wich are the database credentials from your newly created database.

The key to succes connecting to the database in your container is set enviroment variables in your `docker-compose.yml` file.

```
    version: "3"
    services:
    db:
        image: "postgres:11"
        container_name: "Messages-Postgres"
        ports:
        - "5432:5432"
        environment:
        - POSTGRES_DB=c2c
        - POSTGRES_PASSWORD=123456789
        - POSTGRES_USER=c2c
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbNTM1MzI5NzY5LC02MzE3MjU5NTddfQ==
-->