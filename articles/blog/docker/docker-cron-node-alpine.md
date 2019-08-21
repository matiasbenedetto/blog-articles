# How to set-up a cron job inside a docker container

In this article we will walk trough to the creation of a container and a cronjob to execute tasks periodically.

# Setup your image
```
FROM node:10-alpine

# copy crontabs for root user
COPY ./scrapper/cron/cronjobs /etc/crontabs/root  

WORKDIR /app/scrapper

COPY ./scrapper/. .

COPY ./scrapper/database/. ./database/.

RUN npm install

  

# start crond with log level 8 in foreground, output to stderr

CMD ["crond", "-f", "-d", "8"]
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzMjUzOTQwMTMsLTE0NzY4OTM2OTksNz
MwOTk4MTE2XX0=
-->