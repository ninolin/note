# Dockerfile

## Write dockerfile

application is packaged in image by dockerfile and generate a new image

Dockerfile example:
```
FROM node:8.11.3-stretch
ENV NODE_ENV production
WORKDIR /usr/src/app
COPY ["package.json", "package-lock.json*", "npm-shrinkwrap.json*", "./"]
RUN npm install --production --silent && mv node_modules ../
COPY . .
CMD npm start
```

* FROM: from image name
* ENV: docker file variable
** two format: [key] [value]; [key1]=[value1] [key2]=[value2]
* WORKDIR: RUN, CMD, ENTRYPOINT, COPY, ADD cmd execute folder
* COPY: COPY["from path1","from path2",...,"to path"]
* RUN: run cmd when docker creating
* COPY: frompath topath
* CMD: run cmd when docker has been created

## Build dockerfile

use dockerfile to build new image
```
docker built -t [new image name:tag] [Dockerfile path]
docker build -t helloworld .
```



