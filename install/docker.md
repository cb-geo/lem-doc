# CB-Geo LEM in Docker

An easy way to run CB-Geo LEM is to use our prebuilt, high-performance Docker images. This documentation explains how to get quickly started with using LEM in Docker, as well as how to take advantage of more advanced features of Docker. [CB-Geo welcome docs on Docker](https://doc.cb-geo.com/docker/docker.html) covers further information on starting and stopping containers, sharing volume, etc.

## Prerequisites

* [Docker](https://docs.docker.com/engine/installation/)

## Running LEM in a docker container

* Check if `docker` service is running: `systemctl status docker`. 

* On RedHat based linux distros, you can start the docker service using `systemctl start docker`. 

* Docker requires elevated previleges, ensure you are running docker as a sudoer or root. 

* Pull the latest version of the LEM container with all prerequisites: `docker pull quay.io/cbgeo/lem`. This will download the latest version of the LEM container. 

* Check if the LEM container image is available on your system: `docker images`.

* Run the LEM docker container: `docker run -it quay.io/cbgeo/lem:latest /bin/bash`. This will launch the LEM container. 

* Clone the [LEM repository](https://github.com/cb-geo/lem): `git clone https://github.com/cb-geo/lem.git` or if you have a SSH key `git clone git@github.com:cb-geo/lem.git`. The LEM code will be now available at `/home/cbgeo/research/lem`.

* From the `lem` directory, compile the source code using: `mkdir build && cd build &&  cmake -DCMAKE_BUILD_TYPE=Release ..`

## Sharing files from the host into the container


* To share a folder or a volume between the host (local machine) and the docker container:

`docker run -it -v $(pwd):/home/cbgeo/research/lem-shared/ quay.io/cbgeo/lem:latest /bin/bash`.

> **Note** Users running Linux distributions with SELinux enabled (Redhat, CentOS, Fedora, and others) will need to add the :z option to all subsequent host volume mounts -v, e.g.: 
`docker run -it -v $(pwd):/home/cbgeo/research/lem-shared/:z quay.io/cbgeo/lem:latest /bin/bash`.

