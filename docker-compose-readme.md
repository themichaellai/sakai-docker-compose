Docker Compose
==============
Docker Compose is a complement to Docker that allows one to easily manage
multiple containers that comprise a single application. For a development
environment for Sakai, we have three separate containers for Sakai and Tomcat,
MySQL, and an IDE (Eclipse).

Running
-------
All of the containers can be run at the same using `docker-compose up`. If the
images are not already built, then they will be built.

An external MySQL server can be swapped in to substitute the instance of MySQL
running in the container that Docker Compose launches. An example of this is in
[docker-compose.override.yml.example](docker-compose.override.yml.example).

The Sakai container clones
[sakaiproject/sakai](https://github.com/sakaiproject/sakai) when the container
builds, but a
[volume](https://docs.docker.com/engine/userguide/containers/dockervolumes/) can
be used instead, to link the Sakai source files with the host machine.
