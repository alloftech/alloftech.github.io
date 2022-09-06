---
layout: post
title: Handle Several PostgreSQL Versions With Docker
date: 2021-05-24
cover_image: 2021-05-24-feature.jpg
author: Simon Dosda
categories: tooling database
excerpt_separator: <!--more-->
last_modified_at: 2021-06-23
---

> Photo by [Baby Natur](https://unsplash.com/@babynatur).

_[PostgreSQL](https://www.postgresql.org/)_ is a powerful, open-source object-relational database known as the world's most advanced open source database. It is often compared to _[MySQL](https://www.mysql.com/fr/)_, which on its side is known as the world's most popular open-source database.

<!--more-->

These two adjectives describe pretty accurately the difference between the two solutions. While _PostgreSQL_ provides a feature-rich database that can handle complex queries and massive databases, _MySQL_ is much simpler and easy to use.

One of the pain points of _PostgreSQL_, in my opinion, is its installation. Particularly if you need to upgrade it, or worse, to use several versions at the same time. I remember spending quite some time performing these simple tasks, going through multiple documentation every time.

But it is a closed chapter now, thanks to _Docker_!

## Why using Docker to run PostgreSQL?

_Docker_ is an open platform providing the ability to package and run an application in an isolated environment called a _container_. Containers are lightweight and contain everything needed to run an application, so you do not need to rely on what is currently installed on the host.

Which is exactly what we want in order to manage several version of _PostgreSQL_ without having to install them. Note that we will see how to do it for _PostgreSQL_, but you can do the same with any database.

You can find instructions to install _Docker_ [here](https://docs.docker.com/get-docker/). You will also need _psql_ to interact with the database; see [here](https://blog.timescale.com/tutorials/how-to-install-psql-on-mac-ubuntu-debian-windows/) for the installation.

## Run your _PostgreSQL_ container

Once you have installed _Docker_, you can run a container with _PostgreSQL_ using [its official image](https://hub.docker.com/_/postgres).

```bash
docker run --name my-postgres -p 5432:5432 \
  -e POSTGRES_PASSWORD=my-password \
  -e POSTGRES_USER=myself \
  -e POSTGRES_DB=my-db \
  postgres:12
```

This command will start a container using the image `postgres:12` (not specifying the version use the latest one) named `my-postgres` with a port forward from your localhost port `5432` to the container port `5432`. You might want to change your localhost port if you already have _PostgreSQL_ installed on your computer.

We also provide several environment variables to configure our database: `POSTGRES_USER`, `POSTGRES_PASSWORD`, and `POSTGRES_DB`. Defining the user and database names is not mandatory, if you dont they will default to _postgres_.

We can now connect to the database using `psql`.

```bash
psql --host localhost --port 5432 --username myself --dbname my-db
```

You will then be asked for a password, type `my-password`, and you will then access the interactive console.

Our database is currently empty. Let's create a table with a few entries.

```sql
CREATE TABLE persons (id SERIAL PRIMARY KEY, name VARCHAR(100));
INSERT INTO persons (name)
  VALUES ('Sheldon'), ('Leonard'), ('Penny'), ('Howard'), ('Rajesh');
```

We can run a query to check that the table and its lines were created.

```sql
SELECT * FROM persons;

 id |  name
----+---------
  1 | Sheldon
  2 | Leonard
  3 | Penny
  4 | Howard
  5 | Rajesh
(5 rows)
```

Great! We now have a functioning _PostgreSQL_ database without any installation.

## Manage data persistence

We do have a problem, though.

While we can easily stop and start again our container with `docker stop my-postgres` and `docker start my-postgres`, there will be cases we will want to create a new one with the same database, for instance, if our `5432` port is not available.

Creating a new container will not allow us to access the database that we just created, as it is isolated in our container.

The solution to this is to store the database outside of the container, and the easiest way to do so is to use _[docker volumes](https://docs.docker.com/storage/volumes/)._

You can create a new volume with the following command.

```bash
docker volume create my-postgres-db
```

We can then stop and remove our current container, and create a new one, this time linking the database files location to our newly created volume with the command `-v my-postgres-db:/var/lib/postgresql/data`.

```bash
docker stop my-postgres
docker rm my-postgres
docker run --name my-postgres \
  -p 5432:5432 \
  -e POSTGRES_PASSWORD=my-password \
  -e POSTGRES_USER=myself \
  -e POSTGRES_DB=my-db \
  -v my-postgres-db:/var/lib/postgresql/data \
  postgres:12
```

Create some data like we did before and stop the container. You can now remove it and create a new one. When you attach to it, you will see that your data is still here!

If you are curious to know where the database is stored on your computer, you can use the inspect command.

```bash
docker inspect my-postgres-db
```

```json
[
  {
    "CreatedAt": "2021-05-19T22:00:00+02:00",
    "Driver": "local",
    "Labels": {},
    "Mountpoint": "/var/lib/docker/volumes/my-postgres-db/_data",
    "Name": "my-postgres-db",
    "Options": {},
    "Scope": "local"
  }
]
```

## Final thought

I hope I have convinced you that managing several versions of _PostgreSQL_ (or even just one) can be neat and easy thanks to _Docker_.

I am talking about _PosgreSQL_ in this article because it is the SQL database I mostly use, but you can do this with any database. _Docker_ is a fantastic tool when it comes to using software without any need to install it.

Definitely a software worth having in your toolbox!
