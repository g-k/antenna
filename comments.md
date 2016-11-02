At WillKG's suggestion, to support working on this offline, I'm using a branch and embedding comments directly in the code, which can be reformatted later.

```
$ brew install docker
$ docker -v
Docker version 1.12.2, build bb80604
$ brew install docker-compose
$ docker-compose -v
docker-compose version 1.8.1, build 878cff1
$ make build
/usr/local/bin/docker-compose build deploy-base
WARNING: The ANTENNA_ENV variable is not set. Defaulting to a blank string.
Building deploy-base
ERROR: Couldn't connect to Docker daemon - you might need to run `docker-machine start default`.
make: *** [build] Error 1
$ make -v
GNU Make 3.81
Copyright (C) 2006  Free Software Foundation, Inc.
This is free software; see the source for copying conditions.
There is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE.

This program built for i386-apple-darwin11.3.0

# aha, need to start the service
$ brew services start docker-machine

# not enough, needs a default image. maybe this will work?
$ docker-machine create --driver virtualbox default
Running pre-create checks...
(default) You are using version 4.3.28r100309 of VirtualBox. If you encounter issues, you might want to upgrade to version 5 at https://www.virtualbox.org
(default) No default Boot2Docker ISO found locally, downloading the latest release...
(default) Latest release for github.com/boot2docker/boot2docker is v1.12.3
(default) Downloading /Users/lonnen/.docker/machine/cache/boot2docker.iso from https://github.com/boot2docker/boot2docker/releases/download/v1.12.3/boot2docker.iso...

(default) 0%....10%....20%....30%....40%....50%....60%....70%....80%....90%....100%
Creating machine...
(default) Copying /Users/lonnen/.docker/machine/cache/boot2docker.iso to /Users/lonnen/.docker/machine/machines/default/boot2docker.iso...
(default) Creating VirtualBox VM...
(default) Creating SSH key...
(default) Starting the VM...
(default) Check network to re-create if needed...
(default) Found a new host-only adapter: "vboxnet1"
(default) Waiting for an IP...
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with boot2docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env default

$ docker-machine env
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.100:2376"
export DOCKER_CERT_PATH="/Users/lonnen/.docker/machine/machines/default"
export DOCKER_MACHINE_NAME="default"
# Run this command to configure your shell:
# eval $(docker-machine env)

$ eval $(docker-machine env)
$ make build
...
$ make run
...
web_1          | [2016-11-01 22:46:39 +0000] [5] [INFO] Shutting down: Master
web_1          | [2016-11-01 22:46:39 +0000] [5] [INFO] Reason: Worker failed to boot.
antenna_web_1 exited with code 3

#fuuu. this is easily reproducible. Probably need to dig into the error message
```
