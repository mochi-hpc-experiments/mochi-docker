# Mochi Tutorial Docker container

This directory contains instructions for creating a standalone Docker
container called "mochi-tutorial" that can be used for hands on exercises
and general Mochi experimentation.

The image is based on Ubuntu and is pre-configured to include key system
dependencies and a baseline Spack installation.  It also runs an ssh daemon
that that allows access to an account with the user name "mochi" with no
password.

## Installing Docker

Please follow the [Docker installation
instructions](https://docs.docker.com/get-docker/) appropriate for your
platform.

### For Ubuntu users:

If you are running Ubuntu Linux, you can install the version of Docker
provided by Ubuntu by running`sudo apt-get install docker.io`. Interacting
with Docker requires escalated privileges, however. You can accomplish this
by prefixing the Docker commands in this document with `sudo` or you can add
your normal user account to a group that has permission to interact with
Docker as follows:

```
sudo usermod -aG docker $USER
newgrp docker
```

## Getting a mochi-tutorial image

The Mochi tutorial image will consume roughly 800 MiB of disk space once
installed.  It can be downloaded anonymously from a public repository on
Docker Hub, which will take less than a minute on a fast internet
connection.  We also provide instructions for building your own image from
scratch if would like to customize any image settings.

### Option 1 (preferred): download existing image from Docker Hub

The following commands will download the example image from
https://hub.docker.com/repository/docker/carns/mochi-tutorial/general and
then create a generic tag for it with the name "mochi-tutorial" for use
later in these instructions.  Note that you may need to prefix docker
commands with `sudo` if you have not configured your account to have
escalated Docker privileges.


```
docker pull carns/mochi-tutorial:latest
docker tag carns/mochi-tutorial:latest mochi-tutorial
```

### Option 2: building your own image

This directory also contains a Dockerfile that can be modified (if desired)
and used to generate a new image.  Clone this repository to your local
machine (if you have not already) and execute the following command to build
a new image named "mochi-tutorial".

```
git clone https://github.com/mochi-hpc-experiments/mochi-tutorial
cd mochi-tutorial/docker
docker build -t mochi-tutorial .
```

## Starting and logging into the mochi-tutorial container

The following run command will instantiate a new container from the
mochi-tutorial image.  The container will have its name and hostname both
set to "mt1" (short for "mochi tutorial 1").  Note that the container is
configured to run indefinitely in detached mode and allow login for the
"mochi" user with no password.


```
docker run -d -h mt1 --name mt1 mochi-tutorial
```

Once the container is running, you can ssh into the mochi account on it
using the following command:

```
# for bash shell users (most common):
ssh mochi@$(docker exec mt1 hostname -i)

# for fish shell users:
# ssh mochi@(docker exec mt1 hostname -i)
```

**At this point, your Mochi tutorial container is ready, and you may proceed
to the hands-on exercises for your tutorial.**

**TODO: provide links**

## Managing docker containers

Use the following commands to stop and restart the Mochi tutorial container:

```
docker stop mt1
docker start mt1
```

Other helpful commands include:
- `docker ps # view which containers are currently running`
- `docker container ls -a # list all containers, whether running or not`
- `docker images # list available container images`

## updating the mochi-tutorial image on Docker Hub

This section is for Mochi team members only.  The following steps can be
used to generate a new image and update it on Docker Hub.

- create the image following the steps described in this README.md
- log into Docker Hub with `docker login`
- tag the image with a name matching the repository to upload to: `docker
  tag mochi-tutorial carns/mochi-tutorial`
- push the image with a tag name appended: `docker push
  carns/mochi-tutorial:latest`
- log back out of docker hub with `docker logout`

## Running multiple containers simultaneously

This mochi-tutorial image is derived from a more advanced Docker Compose example
provided by Osamu Tatebe of the University of Tsukuba (see
https://github.com/otatebe/mochi-container). Please refer to his example if
you would like to extend your tutorial by running multiple containers
simultaneously and executing distributed services across them.
