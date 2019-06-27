# ![Open Health Information Exchange Mediator Logo](./startUpImages/openhimLogoGreen.svg)

## **Scaffold OpenHIM Mediator Tutorial**

**TLDR; Watch Tutorial Setup on [YouTube](https://www.youtube.com/watch?v={vid-ID})**

## Useful Links

http://openhim.org/

https://github.com/jembi/openhim-mediator-bootstrap-scaffold

https://www.jembi.org/

## Introduction

> Tutorial Purpose: Create a Scaffold OpenHIM Mediator and Register it with your local OpenHIM instance.

This mediator is purely for demonstration purposes and is in no way production ready. However, the mechanisms used in this mediator can easily be used in your own OpenHIM mediator projects. The repository is also useful to read through as there are detailed comments describing important aspects of an OpenHIM Mediator.

The advantage of using the OpenHIM mediator framework over another stand alone service is that OpenHIM mediators are registered and tracked by your OpenHIM instance. This allows administrators to **view the health status** of the mediator, to easily setup **routing** and **logging** to the registered mediator and to **provide new configuration** settings to the mediator all from the OpenHIM Console.

## Prerequisites

* Node and NPM

This tutorial assumes you have already successfully setup the OpenHIM.

Having a basic understanding of ExpressJS and Javascript ES6 syntax is advised.

---

## OpenHIM Mediator Setup

### Step 1 - Creating project skeleton

In a terminal, create a directory for your project. Move into that directory and and run `npm init` and fill in the details you want:

```sh
mkdir openhim-mediator-bootstrap-scaffold
cd openhim-mediator-bootstrap-scaffold
npm init
```

Within the same terminal install the following packages to compile the ES6 code that will be in our project.

```sh
npm install babel-env babel-cli
```

> Do not use babel-node in your production mediator. It uses a lot of memory and has a high start up cost as it compiles your project on the fly.

Open up your preferred Integrated Development Environment(IDE) and create an `index.js` file. Next, edit the `package.json` file that was created by the npm init step and replace the `scripts` field with the following:

```json
"scripts": {
  "start": "babel-node index.js"
},
```

Next, create a file titled `Dockerfile`. Within it place the following YAML script:

```yaml
FROM node:10-alpine
WORKDIR /app

COPY . /app

RUN npm install

CMD npm start
EXPOSE 3000
```

In your IDE move back to the `index.js` file. Here enter teh following script to setup a basic express server listening on port 3000.

```js
'use strict'

import express from 'express'

const app = express()

app.all('*', (req, res) => {
    res.send('Hello World')
})

app.listen(3000, ()=> {
    console.log('Server listening on port 3000...')
})
```

Then, open a terminal in the project directory and run the following commands:

```sh
docker build -t scaffold .

docker run --rm -p 3000:3000 scaffold
```

You should see the terminal output **Server listening on port 3000...**

Test that the mediator is responding correctly by navigating to `localhost:3000` on a browser where you should see **Hello World**

### Step 2 - Registering the Mediator with the OpenHIM

In a terminal install the npm OpenHIM Mediator Utils package. This utils package enable quick mediator setup as it handles OpenHIM authentication, registering mediator, fetching mediator configuration from the OpenHIM, and creating the mediator heartbeat emitter.

```sh
npm install openhim-mediator-utils
```

In your IDE, create a new file called `mediatorConfig.json`. Within this file place the following JSON configurations:

```json
{
  "urn": "urn:mediator:tutorial_scaffold",
  "version": "1.0.0",
  "name": "Scaffold Mediator",
  "description": "Tutorial Scaffold Mediator",
  "defaultChannelConfig": [
    {
      "name": "Bootstrap Scaffold Mediator",
      "urlPattern": "^/scaffold$",
      "routes": [
        {
          "name": "Bootstrap Scaffold Mediator Route",
          "host": "scaffold",
          "path": "/",
          "port": "3000",
          "primary": true,
          "type": "http"
        }
      ],
      "allow": ["admin"],
      "methods": ["GET", "POST"],
      "type": "http"
    }
  ],
  "endpoints": [
    {
      "name": "Bootstrap Scaffold Mediator Endpoint",
      "host": "scaffold",
      "path": "/",
      "port": "3000",
      "primary": true,
      "type": "http"
    }
  ]
}
```

Open `index.js` and import the registerMediator function from `openhim-mediator-utils` as well as the `mediatorConfig.json` file. Next declare a new object openhimConfig with the details below and instantiate the `registerMediator` function with `openhimConfig`, `mediatorConfig`, and a callback

```js
import {registerMediator} from 'openhim-mediator-utils'
import mediatorConfig from './mediatorConfig.json'

// Express Server Code

const openhimConfig = {
    username: 'root@openhim.org',
    password: 'password',
    apiURL: 'https://openhim-core:8080',
    trustSelfSigned: true
}

registerMediator(openhimConfig, mediator, err => {
    if (err) {
        console.error('Failed to register mediator. Check your Config:', err)
        process.exit(1)
    }
})
```

Rebuild the scaffold docker image to include the new changes. Then look up the name of the docker bridge network over which your running openhim instance should be communicating. And finally, start the container including the *network* flag.

> The docker network name is made of the directory name where the docker-compose script was run appended with **openhim**. In this case `tutorial_openhim`

```sh
docker build -t scaffold .

docker network ls

docker run --network tutorial_openhim --rm -p 3000:3000 scaffold
```

Check that your mediator has registered correctly by navigating to the OpenHIM Console Mediator Page on `https://localhost:9000/#!/mediators`

![Mediator Registered](./startUpImages/registeredMediator.png)

### Step 3 - Adding Mediator Heartbeat
