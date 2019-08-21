# How to set-up a cron job inside a docker container

In this article we will walk trough to the creation of a container and a cronjob to execute tasks periodically. I will use examples in my use-case

# Create your crontab file
```
* * * * * export NODE_PATH=/app/scrapper/node_modules/ && cd /app/scrapper && npm run start >> /logs/scrapper.log 2>&1
```

# Setup your image
To mantain the size of  the containers to the minimum size possible I gonna use the Alpine version of the Node image.

My `Dockerfile` looks like this:
```
FROM node:10-alpine
# copy crontabs for root user
COPY ./scrapper/cron/cronjobs /etc/crontabs/root  
WORKDIR /app/scrapper
COPY ./scrapper/. .
RUN npm install  
# start crond with log level 8 in foreground, output to stderr
CMD ["crond", "-f", "-d", "8"]
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3OTQ2ODcyNjQsLTE0NzY4OTM2OTksNz
MwOTk4MTE2XX0=
-->