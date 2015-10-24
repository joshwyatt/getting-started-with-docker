## Getting started with Docker

- [ ] Create a local virtualbox machine called `default` using `docker-machine`
- [ ] Use `docker-machine env` to point Docker Machine at `default`
  - [ ] Install `dkme`
 
- [ ] Try out `docker-machine ls`, `docker ps`, `docker ps -a`, `docker images`
  - [ ] Create aliases `dkm`, `dk`, and `dki`
- [ ] Import images as needed using `docker load -i <path to image file>`
 
- [ ] Run a container off of the `node:latest` image, with no commands
- [ ] Run a container off of the `node:latest` image but give it a command
- [ ] Run a container off of the `node:latest` image but give it a persistent command
- [ ] `docker exec -it ... bash` into the running container and add a web server
- [ ] Stop the container and `docker commit` a new image from it
- [ ] Run a container from the new image, issuing a `node index.js` command
- [ ] Install `dkrm` and clean up
 
- [ ] Make a Dockerfile to do the whole image building process we just did manually, with a command
- [ ] Make a better Dockerfile that handles `package.json`
- [ ] Replace with Dockerfile that utilizes `node:onbuild`
 
- [ ] Spin up a web server, exposing ports. How do we know which url to visit?

## Glossary of common commands

`docker-machine ls`: get info on all machines available to Docker Machine
`docker-machine build --driver <driver-name> <options> <machine-name>`: create new host machine available to Docker Machine
`docker-machine env <machine-name>`: get info on how to point Docker Machine at `<machine-name>`

`docker ps`: show all running containers on this machine
`docker ps -a`: show all containers on this machine running or not
`docker ps -aq`: show all ids for containers on this machine running or not
`docker images`: show all images on this machine 
`docker rmi <image-name>`: remove `<image-name>` from machine
`docker stop <container-name>`: stop running `<container-name>`
`docker rm <container-name>`: destroy `<container-name>`
`docker rm -f <container-name>`: stop and destroy `<container-name>`
`docker rm -f $(docker ps -aq)`: stop and destroy all containers on this machine

`docker run [<options>] <image-name> [<command>]`: run a container from `<image-name>` [with `<command>`]
  Options include:
    `--name`: give the container a name
    `--rm`: destroy the container as soon as it stops running
    `-d`: run the container as a background process, or daemon
    `-it`: run the container interactively, running from its command line
    `-p <host-port>:<container-port>`: map the exposed `<container-port>` to `<host-port>`
    `--link <container-to-link-to>:<alias-for-linked-container>`: Create environmental variable linking to `<container-to-link-to`

`docker exec <container-name> <command-name>`: execute `<command-name>` on running `<container-name>`
`docker exec -it <container-name> bash`: go to command line of running `<container-name>`
