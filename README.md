# wow-classic-db-docker

A simple Docker Compose environment for spinning up a WoW Classic (Vanilla) MySQL database.

## Requirements

- Docker
- Docker Compose

## Why Use This Project?

I mainly developed this project as I like to mess with the data WoW Classic databases - extracting information about items and NPCs and learning about the inner workings. It is not really suitable for a production server build, but some tweaks could be made to improve performance and make data persistent using volumes. Please note that this build only includes the "world" database, and the usual "realm", "characters" and "logs" databases are not included. Basically, this is just the data that contains in-game information, and not the database configuration required for hosting a full MaNGOS-style server.

## Quickstart

- Change credentials in the `.env` file if you want
- Change `Dockerfile` option in `docker-compose.yml` to `Dockerfile-cmangos` or `Dockerfile-vmangos`
- Start containers using `docker-compose up --build`

## Selecting the Database Source

There are a bunch of WoW Classic database options available. This projects supports [Continued MaNGOS](https://github.com/cmangos) as it is reliable and robust, and [Vanilla MaNGOS](https://github.com/vmangos) as it has built in support for Vannila patch progression.

In the `docker-compose.yml` file, set the `Dockerfile` to property to the desired source. The options are:

- CMaNGOS version uses `Dockerfile-cmangos`
- VMaNGOS version uses `Dockerfile-vmangos`

Simply change line 9 of the `docker-compose.yml` file to either Dockerfile. For example:

```none
dockerfile: Dockerfile-vmangos
```

## Insert the Database Contents

This process can be either manual or automated. By default, it is set to manual - mainly so the container can be restarted without performing too much repitition of tasks. To manually insert the data, just enter the Docker container and run the initialization script.

```none
docker exec -it wow-classic-db /bin/bash
./init_cmangos.sh
OR
./init_vmangos.sh
```

If you want the container to automatically insert the database when started, uncommnet out the following line in the `Dockerfile-cmangos` file.

```none
# Copy autopopulate script to docker entrypoint to auto insert data
# Comment out if you don't want to auto populate
COPY ./init.sh /docker-entrypoint-initdb.d/init.sh
```
