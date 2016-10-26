## Dockerizing a Node.js web app
The goal of this example is to show you how to get a Node.js application into a Docker container. The guide is intended for development, and not for a production deployment. The guide also assumes you have a working Docker installation and a basic understanding of how a Node.js application is structured.

In the first part of this guide we will create a simple web application in Node.js, then we will build a Docker image for that application, and lastly we will run the image as a container.

Docker allows you to package an application with all of its dependencies into a standardized unit, called a container, for software development. A container is a stripped-to-basics version of a Linux operating system. An image is software you load into a container.

[Dockerizing a Node.js web app](https://nodejs.org/en/docs/guides/nodejs-docker-webapp/)

### Create the Node.js app
First, create a new directory where all the files would live. In this directory create a `package.json` file that describes your app and its dependencies:
`
{
  "name": "docker_web_app",
  "version": "1.0.0",
  "description": "Node.js on Docker",
  "author": "huangxubo23",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.14.0"
  }
}
`

Then, create a `server.js` file that defines a web app using the Express.js framework:
`
'use strict';

const express = require('express');

// Constants
const PORT = 8081;

// App
const app = express();
app.get('/', (req, res) => {
    res.send('Hello world');
});

app.listen(PORT);
console.log('Running on http://localhost:' + PORT);
`

### Creating a Dockerfile
Create a docker file called `Dockerfile`:
`
FROM node:latest

\# Create app directory
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

\# Install app dependencies
COPY package.json /usr/src/app/
RUN npm install

\# Bundle app source
COPY . /usr/src/app

EXPOSE 8081
CMD ["npm" "start"]
`

### Building your image
Go to the directory that has your `Dockerfile` and run the following command to build the Docker image. The `-t` flag lets you tag your image so it's easier to find later using the `docker images` command
`
$ docker build -t docker-node-demo .
`
Then use `docker images` command, and your image will now be listed by Docker.