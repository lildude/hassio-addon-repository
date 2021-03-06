# Home Assistant PostgreSQL addon

PostgreSQL is a powerful, open source object-relational database system with over 30 years of active development that has earned it a strong reputation for reliability, feature robustness, and performance.

## Version

This addon uses version 12.2 of PostgreSQL.
It is based on the official Docker Hub build on [Github](https://github.com/docker-library/postgres/tree/master/12/alpine).
This image uses the Home Assistant base image, rather than the native Alpine image used in the official image.

## Configuration

Set the database name, username and password  in the yaml editor:

```yml
username: mydbuser
password: mysecretpassword
database: mydbname
```

## Connecting to the DB

To connect your application to the database, specify the server name as: {slug}-postgres. The port number is the default of 5432.
