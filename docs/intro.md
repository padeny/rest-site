---
sidebar_position: 1
---

# Introduction

**Rest** serves a fully RESTful API from any SQL database, PostgreSQL, MySQL and SQLite are supported, databases that are wire compatible with PostgreSQL or MySQL , e.g. CockroachDB and TiDB should also be supported now, more databases might be added in the future.

## Getting Started
### Start Rest in Docker
run server and connect to an existing database
``` bash
# connect to postgres
docker run -p 3000:3000 restgo/rest -db.url "postgres://user:passwd@localhost:5432/db"

# connect to sqlite file with volume
docker run -p 3000:3000 -v $(pwd):/data restgo/rest -db.url "sqlite:///data/my.db"
```

### Use API
Assume there is a `todos` table in the database with `id`, `title` fields:

``` bash
# Create a todo item
curl -XPOST "localhost:3000/todos" -d '{"title": "setup api server", "done": false}'

# Read
curl -XGET "localhost:3000/todos/1"

# Update
curl -XPUT "localhost:3000/todos/1" -d '{"title": "setup api server", "done": true}'

# Delete
curl -XDELETE "localhost:3000/todos/1"
```

check [Installation](./tutorials/installation) page for more ways to install Rest.

## Motivation

Rest came out as a side project when implementing a toy database like SQLite, the idea was inspired by an interesting project [PostgREST](https://postgrest.org/en/stable/) which "is a standalone web server that turns your PostgreSQL database directly into a RESTful API". Golang has the `database/sql` package which provides a generic interface around SQL and there a bunch of [drivers](https://github.com/golang/go/wiki/SQLDrivers) available, so the idea of Rest is to implement a similar api server that serves a fully RESTful API from any SQL database.

## Features
- Various SQL database (PostgreSQL, MySQL, SQLite)
- Fully RESTful API
- Row level policies
- Authentication and Authorization
- Built in Golang with built-in concurrency and a robust standard library
  - `net/http` for the api server
  - `database/sql` for the generic SQL interface
  - `encoding/json` for json operations
  - large ecosystem in database and cloud

## Comparison with other tools

### PostgREST

[PostgREST](https://postgrest.org/en/stable/index.html) is a standalone web server that turns your PostgreSQL database directly into a RESTful API. It's the most similar tool with Rest, it binds to PostgreSQL to leverage powerful PG features, e.g. using PostgreSQL roles concept to provide authentication. Rest aims to serve API from any SQL database and the design principle is to support various SQL database features while using standard SQL tables for application logic.

The idea of Rest is inspired by PostgREST, and it's a great alternative if you are using PostgreSQL now.

### Hasura & Directus

[Hasura](https://hasura.io/) and [Directus](https://directus.io/) are very similar, they are both written in Node.js, provide REST and GraphSQL interface from different databases, has sophisticated authorization mechanism and UI for database management. Besides the open source code there are also companies provide cloud environment to help you run the server. While having more functions they are also more complicated to get start to use.

Rest aims to keep the usage as simple as possible to let users start a fully RESTFul API server in a minute. There might be more features come to Rest, but the design principle should always to keep it simple and stupid.
