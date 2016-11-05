# Getting Started With Docker

## Installing Docker

Befoe beginning, please follow the [installation instructions](https://docs.docker.com/engine/installation/) provided for your OS by Docker.

## Conceptual Overview of Docker

While not necessary to complete these exercises, you might find it interesting or helpful to read up on the largely conceptual [Docker Overview](https://docs.docker.com/engine/understanding-docker/).

## Running Commands in a Container

*Containers* are run from *images* on a host machine using the following syntax:

`docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`

You can see what images exist on the host machine with `docker images`. If the image you are wanting to run from does not exist locally, Docker will attempt to pull it down from [DockerHub](https://hub.docker.com/).

You can see what containers are running on the host machine with `docker ps`. You can see all containers on a host machine, running and stopped, with `docker ps -a`.

- [ ] Look at what images, if any, are currently stored on the host machine you are using
- [ ] Run a container using the `alpine` image, issuing the command `echo hello world`
- [ ] Look again at what images are currently stored on the host machine
- [ ] Observe if there are any running or any stopped containers on the host machine
- [ ] Run a container using the `alpine` image that uses `pwd` to print the default directory inside the container
- [ ] Run a container using the `alpine` image that uses `ls` to observe the contents of the default directory inside the container
- [ ] Run a container using the `alpine` image that uses `cat` to observe the contents of the `/etc/hosts` file inside the container

When you want to issue commands that require piping or redirection, you should use the `sh -c "<commands>"` syntax rather than issuing commands directory.

- [ ] Run a container using the `alpine` image that uses `grep` to print the lines of `/etc/hosts` containing `127`

## Provisioning a TTY to Interact Inside a Container

Using the options `-i` and `-t`, you can provision an **i**nteractive **T**TY. Used in conjunction with a command that results in a command line, such as `sh` (which you can expect to be available on most all containers) or `bash`, or `python` or `node` or `redis-cli` or `mongo`, you can interact with the command line inside of a container.

- [ ] Run a container using the `alpine` image that provisions an interactive TTY and invokes the `sh` command

## Container Cleanup

You might notice (using `docker ps -a`) that containers are starting to pile up. You can remove containers with `docker rm [CONTAINER-NAME-OR-ID]`. To remove stopped containers in mass consider the one-liner `docker rm $(docker ps -aq)`

- [ ] Clean up your stopped containers, first one at a time and then in mass

## Committing Modified Containers to Build a New Image

While containers are run from images, new images are built by taking snapshots of a given container. When you make modifications inside of a container, you can *commit* those changes to a new image using the syntax `docker commit [CONTAINER-NAME-OR-ID] [NEW-IMAGE-TAG]`. Also, when running a container, you can use the `--name [NAME]` option to give the container a name of your choosing, rather than Docker's randomly generated one.

- [ ] Run a container from the `alpine` image that uses `touch` to create a new file within it. Give the container the name `modified`
- [ ] Commit the `modified` container to a new image called `my-new-image`. Confirm the image is now present on your host machine
- [ ] Run a container from the `my-new-image` image that issues a command confirming the file you `touch`ed exists

## Running a Persistent Command Inside a Container

Docker containers will stop as soon as the command that was issued within them completes. So far all the commands we have issued have been one off commands. By issuing a persistent command, the container will continue to run. Often when running containers with persistent commands, you'll want to use the `-d` flag to run it in the background.

You can look at the logs for a running or stopped container with `docker logs [CONTAINER]`. To follow the logs of a container add the `-f` flag.

- [ ] Run a command from the `alpine` image, in the background, with the name `pinger`, that uses `ping google.com` as its command
- [ ] Confirm that the container is running
- [ ] Look at the logs for the `pinger` container
- [ ] Follow the logs for the `pinger` container

## Issuing Commands in a Running Container

Especially with running containers, you might wish to execute a command within it while it is running. This is done with `docker exec [OPTIONS] CONTAINER COMMAND [ARGS]`

- [ ] Issue a one off command inside your running `pinger` container, such as `uname`

## Stopping and Removing a Running Container

Before you can remove a running container you must stop it with `docker stop CONTAINER`. Alternatively you can pass the `-f` flag into `docker rm`.

  - [ ] Stop and remove the `pinger` container

## Building a New Image With a Dockerfile

Typically, we build new images by leaving instructions for building them within a special file called `Dockerfile`, which always contains instructions for an image to build this new image `FROM`, and then often, a default `CMD` for a container run from this new image to use. Once we have a `Dockerfile`, we build a new image with `docker build -t IMAGE-TAG PATH-TO-DOCKERFILE`

You will use the following contents for a `Dockerfile`.
```
FROM alpine
CMD ["ping", "google.com"]
```

- [ ] Using this `Dockerfile`, `build` a new image with the tag `pinger`.
- [ ] Run a container named `my-pinger` using the newly built `pinger` image. You do not need to issue a command since the `Dockerfile` for `pinger` gave a default
- [ ] Confirm your container is running. Look at its logs and then clean it up.
- [ ] Remove the newly built `pinger` image using `docker rmi IMAGE`

## Building an Image With Application Code

Typically you'll want to build a new image that contains application code. Write the following python script, which you will be building into an image:

hello.py
```
print("hello")
```

In order to run python code inside a container, we should build our images with an image that already has python installed.

- [ ] Interact with a python interpreter inside a container with python installed by issuing `docker run -it jfloff/alpine-python:3.4 python`

In order to house our application code inside an image we will modify the Dockerfile with instructions for placing our application code within the image we are building.

Dockerfile
```
FROM python:3.5
RUN mkdir app
ADD hello.py /app
WORKDIR /app
CMD ["python", "hello.py"]
```

docker build -t hello-py .
docker run hello-py
[change the file, rebuild and run]

## Mounting a Volume Into a Container

docker run -v $PWD:/app h ello-py
[change the file and run]

## Using Docker Compose to Build and Image and Run a Container

docker-compose.yml
```
version: "2"

services:
  greeter:
    build: .
``

docker-compose up

docker-compose.yml
```
version: "2"

services:
  greeter:
    build: .
  volumes:
    - ./:/app
```

## Communicating Between Containers Using Docker Compose

docker-compose.yml
```
version: "2"

services:
  ping:
    image: pinger
  pong:
    image: pinger
```

docker-compose up -d
docker ps
docker-compose logs -f

docker exec -it <ping> sh
ping pong
