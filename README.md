# Description

This repository contains a Dockerfile intended to be use to build and develop [MySQL](https://github.com/mysql/mysql-server/).


Also, it includes a Makefile to make interactions with the development environment easier.

By default, the environment assumes that MySQL source code is located in `$HOME/projects/mysql-server`. If it is not your case, please adapt that in the Makefile.

The Makefile is self-documented. It means, that by running `make` it will print the help.

```shell
❯ make
build      Build a docker image tagged mysql-devenv (for Debian)
build.ol9  Build a docker image for building tagged mysql-devenv.ol9 (for Oracle Linux 9) suitable for normal builds but also building rpms
shell      Execute bash inside the container in interactive mode
start      Start a docker container in background from mysql-devenv
stop       Stop the docker container
```

## For Mac users

Docker Desktop by default uses "gRPC FUSE" for file sharing implementation. This is *SLOW* and is not handy for compilation processes.

It is highly recommended that you switch to "VirtioFS" ([See this article](https://www.docker.com/blog/speed-boost-achievement-unlocked-on-docker-desktop-4-6-for-mac/)).


## Example worklflow

- Build the development environment

```shell
❯ make build
```

- Start the dev container

```shell
❯ make start
```

- Start a bash session on the container

```shell
❯ make shell
```

## Override default settings:

```shell
# build using OL9 build environment using a different mysql-server path
# variables could be exported once with export DOCKER_FILE=..., PROJECT_DIR=..., IMAGE_TAG=...
DOCKER_FILE=Dockerfile.ol9 PROJECT_DIR=~/src/mysql-server make build
PROJECT_DIR=~/src/mysql-server make start
PROJECT_DIR=~/src/mysql-server make shell
```

- Hack!


## Building MySQL

It is recommended that you create a `build` folder inside the MySQL's source code tree. That folder will contain all the build files.


## Building mysqld (server)

- Attach to the container (`make shell`)
- Go to the directory `/development/mysql/build`
- Run `cmake -DWITH_DEBUG=1 ..` (Adapt this command to your needs).
- Run `make -j4 mysqld` (4 indicates the number of CORES to compile)

It will generate the mysqld binary under `runtime_output_directory`

After that you can run your recently compiled version of MySQL by executing:
```shell
$ ./runtime_output_directory/mysqld <arguments>
```

## Building mysql (client)

- Attach to the container (`make shell`)
- Go to the directory `/development/mysql/build/client`
- Run `make -j4`

It will generate the `mysql` (client) binary under `runtime_output_directory`.

## Building mysqlclient library

- Attach to the container (`make shell`)
- Go to the directory `/development/mysql/build`
- Run `make -j4 mysqlclient`

It will generate the mysql client library required for mysql (client) binary.


# EOF
