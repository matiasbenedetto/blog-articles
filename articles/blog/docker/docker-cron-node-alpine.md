# How to set-up a cron job inside a docker container

In this article we will walk trough to the creation of a container and a cronjob to execute tasks periodically.

I will use examples in my usage during the building of a **scrapper**, so consider `scrapper` the root folder of my app.

## Create your crontab file
```
* * * * * export NODE_PATH=/app/scrapper/node_modules/ && cd /app/scrapper && npm run start >> /logs/scrapper.log 2>&1
```

There are a few interesting parts here that I had to research for.
We  need to tell to the cronjob where are our `node_modules` folder, otherwise the system wont find our modules.  We can do this with: `NODE_PATH=/app/scrapper/node_modules/`. After that we move inside our code folder with `&& cd /app/scrapper` and  we run the start script  of our app with `&& npm run start`. Finally we set the output logs to a file with `>> /logs/scrapper.log 2>&1`.

## Setup your image
To mantain the size of the containers to the minimum I gonna use the Alpine version of the Node image.

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
eyJoaXN0b3J5IjpbLTQ2NDkxMTYyMiwtMTM4NjI4MDM3MCwtNT
M3MjI4NjMyLC0xNDc2ODkzNjk5LDczMDk5ODExNl19
-->