# Node Sass not compiling using docker compose

Paradoxically this is not a post about node-sass problems but the use of `.dockerignore` file.  There is nothing wrong with node-sass in docker-as far I know, but I found myself in strange situation trying to build my project using node-sass and getting this error after running `docker-compose build`.


## The problem
I found an error like this building my React app inside a Docker container:
```
Failed to compile.

./styles/components/VideoIframe.scss
Error: Missing binding /app/frontend/node_modules/node-sass/vendor/linux_musl-x64-64/binding.node
Node Sass could not find a binding for your current environment: Linux/musl 64-bit with Node.js 10.x

Found bindings for the following environments:
  - Linux 64-bit with Node.js 10.x

This usually happens because your environment has changed since running `npm install`.
Run `npm rebuild node-sass` to download the binding for your current environment.
```
The error message stated `Node Sass could not find a binding for your current environment` but I had the  module `node-sass` present in my `package.json` and the `npm install` was being run.  So what was the problem?

## The solution
The error was caused because of the presence of the `node_modules` folder in my containerized app. The folder has been copied from my local development folder, so the containerized app was trying to load the node modules installed by the npm of dev computer. As node-sass is compiled for each operative system this error help me to detect a problem in my Docker setup. I had forgotten to include the `.dockerignore` file in the folder root of my project.

So after the creation of my `.dockerignore` with `**/node_modules` inside, and re-running `docker-compose build`. I got my app up and running.

My complete `.dockerignore` now:
```
**/node_modules
.git
.vscode
.env-sample
Dockerfile
.next
.build
```
## With Volumes
If you are using volumes in your docker-compose, for example, to mount your local files during development you will need a way to indicate Docker you want to use the `node_modules` folder of your containerized app and not your local `node_modules` folder. Why? To use the specific npm modules, like the correct compiled version of `node-sass`,  installed in your docker container operative system and not your local ones.

You can do this mounting explitely the `node_modules` folder of your containerized app.
**Example:**
```
frontend:
	volumes:
		- ./frontend:/app/frontend/
		- /app/frontend/node_modules/
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwNDE2Nzc5MSwzMDM1NjY3MTldfQ==
-->