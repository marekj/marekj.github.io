---
layout: post
title: Notes to self: First steps with Postgresql on Mac OS X
---

First steps with postgres on Mac OS X. Prototyping with Sequel ruby gem. Accessing remote postgres example

## install

Use homebrew:

`brew cask install postgres`

When you start it asks you to move to `/Applications`. Do it.

Start with spotlight: `postgres` and you will see a nice GUI panel. Alternative start on cli with  `open /Applications/Postgres.app` (postgres stars by default as a local unix domain socket)

## connect and poke around

Open terminal and type `psql -l` to list the databases existing on this local cluster (your host machine is now a cluster of databases)

By default you will have a db created for you with your username owned by that username. (it's a default database for the user who installed it (You). You can set owner of a db manually. Look at Group Roles and Login Roles to understand this a bit more)

`psql` opens that default db with your username. (Also  while in pslq you can type `\l` to list all databases in local machine cluster)

## where is my data?

`psql# show data_directory;` (this shows all `psql# show all;` always terminate with ; to execute)

On my mac it's
`~/Library/Application Support/Postgres/var-9.5`

`PGDATA` env variable should hold the value of data_directory (aka ConfigDir) but it's not set on my machine, no need.

This is your database storage location, database cluster, catalog cluster. Collection of databases managed by single instance of a server

`postgres` is a database for utilities in that cluster
`template1` is a template from which new db will be created

## processes

`htop` shows you postgres running processes (tree view). Default port is 5432. Notice postgres started with `-D <data_directory>`

`<data_directory>/postmaster.pid` file holds the id of process

## Connect to databases

With Postico.app, pgAdmin3 and `psql`

`brew cask install postico`

`brew cask install pgadmin3`

`psql` opens prompt and connects to the database named after your `whoami` (exit with `\q`)

## executables

Look at `/usr/local/Cellar/postgresql/9.5.1/bin` to find many tools there. Not only psql.


## docs

Great docs : http://www.postgresql.org/docs/9.5/static/index.html

Modify `pg_hba.conf` to set client authentication http://www.postgresql.org/docs/9.5/static/auth-pg-hba-conf.html
(HBA stands for host-based authentication) by default you can only connect to it from the same host (no remote connections) notice local and host is set to 'trust' but I think on linux it's set to peer by default.

## Postgres on Linux

See info here: https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps

ssh to the machine running postgresql use `sudo su - postgres` user and `psql` there. (you will connect as user postgres)

How to develop locally against a database on a remote server? (since you don't have remote connection and I guess you should not allow it)

setup ssh tunnel to remote server and connect to your postgresql with localhost


`ssh  -L 63333:localhost:5432 <remote_user>@<on_remote_hostanme_with_postgres>`

Then you can connect to it as if it was running on hostname localhost

`psql -h localhost -p 63333 -U postgres` (or the username you have for your app)


## Sequel ruby gem

Install the gem and connect to db with `sequel` binary provided by the gem.

`sequel postgres://<whoami>:@localhost:5432/<dbname>`
