## Postgres - Nest.js - React.js Boilerplate

![docker compose build](https://github.com/nadavpodjarski/postgres-express-react-typescript-boilerplate/workflows/Docker%20compose%20build/badge.svg?branch=master)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
<img src="./loby-img.png" style="max-width:300px; max-height:200px;">

### Introduction

This is a Full-stack Dockerized boilerplate.
A vanilla infrastructure made to simplify the develpoment and deploying processes.

#### Stack

- React.js
- Redux
- Node.js
- Nest.js
- Postgres
- Docker

### Prerequisites

Make sure you have the below installed on your machine.

- [x] **Docker** : https://docs.docker.com/engine/install/
- [x] **Docker-Compose** : https://docs.docker.com/compose/install/
- [x] **Node** : https://nodejs.org/en/

### File strcutre

```
project-name
    |
    |---/ client
            |
            |---/ public
            |---/ src
            |
            .env
            .dockerignore
            .gitignore
            Dockerfile
            Dockerfile.dev
            nginx.conf
            tsconfig.json
            package.json
    |
    |---/ database
            .env
            initdb.sql
            .gitignore
    |---/ server
            |
            |---/src
            |
            ormconfig.json
            .dockerignore
            .gitignore
            Dockerfile
            Dockerfile.dev
            nodemon.json
            package.json
            wait-for-it.sh
    |
    |
    docker-compose.yml
    docker-compose-dev.yml
    .prettierrc
    README.md
```

### Quick start

Clone this repo to your local machine

```
git clone https://github.com/nadavpodjarski/postgres-express-react-typescript-boilerplate.git project-name
```

Before we run our container lets calm down our editor and npm install dependecies locally.
For that let's run the following command

```bash
# install server dependencies in the server directory

npm i
```
```bash
# install client dependencies in the client directory

npm i
```

Now Let's check our Demo, for that run the following command in the school directory

```bash
docker-compose --file docker-compose-dev.yml up
```

it will be served on `http://localhost:3000`

## Client

Client has been created with create-react-app and located in `./school/client`

#### Development

In development mode the client will be run in a container built with `./client/Dockerfile.dev` and will be exposed on port 3000, with docker volumes every change thats been saved will be reflected within the running container.

#### Production

In production mode the client build will be created and will run in a container built with `./client/Dockerfile`.
The client build/static-files will be served with nginx server and will be exposed on port 80.

#### Environment Variables

Environment variables are located in `./client/.env` but can be declared into the dockerfile itself under ENV or in the docker compose file under enviornemt property.
**In order to use docker stack deploy** it's needed to use one of the other options and not env_file.

**note that nginx server has a minimalistic configuration**

## Data-base

Postgres database is created with an official postgres image which can be found in docker hub https://hub.docker.com/_/postgres

Environment variables will be located in the docker-compose file.`
and will contain our database credentials :

```
POSTGRES_USER=admin
POSTGRES_PASSWORD=admin
POSTGRES_DB=pern_db
```

Volumes of our database will be located in `./server/database/data`

> Production volume is located in `./server/data/prod` </br>
> Development volume is located in `./server/data/dev`

## Server

Server is located in `./projec-name/server` using express.

#### Development

In development mode the server will run in a container built with `./server/Dockerfile.dev`.
and will be exposed on port 5500 to the "outside" world, with docker volumes and nodemon, every change thats been saved will be refelected within the running container.

#### Production

In production mode the server will run in a container built with `./server/Dockerfile`.
and be exposed on port 5500 only to the docker composer internal services within the same network.
in our case server and client are on the same network "webapp" , hence only the client can communicate with the server, and will do thatthrough the `/api` location. for more locations, its needed to configure them in the `./client/nginx.conf` file.

#### Database connection

Database connection is handled with ormconfig.json that is located at `./server/ormconfig.json`
and will contain postgres credentials to establish connection to our data-base.
Thanks to https://github.com/vishnubob/wait-for-it for the wait-for-it.sh script, we can set that the server image will run only after getting confirmation that postgres container is available.
by that we won't get connection failures due to bad order of docker composing.

## Docker compose

### Development

To establish a development environment, simply run the following command from the project root folder.

```bash
docker-compose --file docker-compose-dev.yml up
```

On save changes in client and server, containers will be automatically updated, no need to restart any servers.
</br>

### Production

To establish production environment, simply run the following command from the root folder.

```bash
docker-compose up
```

This will creates build for both server and client, will serve client build with nginx server on port 80 and will communicate with server on port 5500 in the location /api.

- If you are making changes within the dockerfiles you will need to rebuild them, for that add the --build flag to the docker compose up command.

## Enjoy
