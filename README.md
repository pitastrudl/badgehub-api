# BadgeHub API

> Node.js REST service for the BadgeHub

## - Development -

## Install

Make sure [Docker](https://www.docker.com/get-started/) is installed and running and Node.js and npm are installed.

In the project directory, type

```bash
npm install
```

This will install `node_modules`.

## Run

Before running, copy the `.env.example` into `.env`

```bash
cp .env.example .env
```

and fill out the details.

Then start the Docker containers by typing

```bash
docker compose up --detach
```

The first time Node.js might not start.

```bash
docker logs badgehub-api-node-1
```

If the logs say something like

```text
node:internal/modules/run_main:115
    triggerUncaughtException(
    ^
Error [TransformError]:
```

Then node_modules have to be install from inside the container. type:

```bash
docker exec -it badgehub-api-node-1 npm install
```

and restart the service:

```bash
docker compose restart
```

Then visit [http://localhost:8001/](http://localhost:8001/) for the development BadgeHub homepage.

Visit [http://localhost:8002/](http://localhost:8002/) for the pgAdmin interface.
Use password `badgehub` to connect to the BadgeHub database server.

Use the [OpenAPI (Swagger) documentation](/openapi) to interact with the REST API.

## Development

After setting up the development container, you can start it with

```bash
docker compose up --detach
```

And to stop BadgeHub

```bash
docker compose down
```

Or, to stop BadgeHub and delete all volumes (to start fresh)

```bash
docker compose down --volumes
```

## Database schema

At the moment, this is the database schema:

[BadgeHub Schema](https://drawsql.app/teams/badge-team/diagrams/simplified-database)

## - Production -

In production, use the production docker compose file `docker-compose-production.yml`.
The `NODE_ENV` environment file is set to `production`, there's no watcher and
PM2 is used to run Node.js multithreaded.

The first time, a Docker container is created. Make sure the `dist` directory
contains the latest build to be copied to the container.
Also the `public` directory and `package.json` and `package-lock.json` will
be copied.

To start:

```bash
docker compose --file docker-compose-production.yml up --detach
```

Then visit [http://localhost:9001/](http://localhost:9001/) for the production BadgeHub homepage
and [http://localhost:9002/](http://localhost:9002/) for PG_Admin, the UI for the database.

To wind down:

```bash
docker compose --file docker-compose-production.yml down
```

## Tools used

- [Express](https://expressjs.com/), a framework for Node.js
- [tsoa](https://tsoa-community.github.io/docs/) for generating a swagger file from code
- [tsx](https://tsx.is/) for watching TypeScript files in Node.js
- [PM2](https://pm2.keymetrics.io/) for managing Node.js processes
-
