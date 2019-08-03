# Node Sass not compiling using docker compose

Paradoxically this is not a post about node-sass problems but the use of `.dockerignore` file.  There is nothing wrong with node-sass in docker-as far I know, but I found myself in a nasty situation trying to build my project using node-sass and getting this error after running `docker-compose build`.


## The problem
```
Failed to compile.

./styles/components/VideoIframe.scss
Error: Missing binding /app/frontend/node_modules/node-sass/vendor/linux_musl-x64-64/binding.node
Node Sass could not find a binding for your current environment: Linux/musl 64-bit with Node.js 10.x

Found bindings for the following environments:
  - Linux 64-bit with Node.js 10.x

This usually happens because your environment has changed since running `npm install`.
Run `npm rebuild node-sass` to download the binding for your current environment.

> Build error occurred
Error: > Build failed because of webpack errors
    at build (/app/frontend/node_modules/next/dist/build/index.js:6:847)
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! cines-frontend@1.0.0 build: `next build`
npm ERR! Exit status 1
npm ERR! 
npm ERR! Failed at the cines-frontend@1.0.0 build script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2019-08-03T18_17_14_954Z-debug.log
ERROR: Service 'frontend' failed to build: The command '/bin/sh -c npm run build' returned a non-zero code: 1
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


<!--stackedit_data:
eyJoaXN0b3J5IjpbMzAzNTY2NzE5XX0=
-->