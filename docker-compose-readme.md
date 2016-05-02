Docker Compose
==============
Docker Compose is a complement to Docker that allows one to easily manage
multiple containers that comprise a single application. For a development
environment for Sakai, we have three separate containers for Sakai and Tomcat,
MySQL, and an IDE (Eclipse). If any parts do not need to be used, then they can
simply be removed by removing their corresponding sections in
[`docker-compose.yml`](docker-compose.yml).

Running
-------
All of the containers can be run at the same using `docker-compose up`. If the
images are not already built, then they will be built. However, if changes to
Dockerfiles are made and previous images have already been built,
`docker-compose up` will not rebuild the images, and the old images will be
used. `docker-compose build` can be used to rebuild them.

Overrides
---------
An external MySQL server can be swapped in to substitute the instance of MySQL
running in the container that Docker Compose launches. This is done using a
compose override file.An example of this is in
[docker-compose.override.yml.example](docker-compose.override.yml.example). This
is described in more detail in the [Docker documentation for extending compose
files](https://docs.docker.com/compose/extends/).

Source Code
-----------
The Sakai container clones
[sakaiproject/sakai](https://github.com/sakaiproject/sakai) when the container
builds, but a
[volume](https://docs.docker.com/engine/userguide/containers/dockervolumes/) can
be used instead, to link the Sakai source files with the host machine. This will
also save a minute or two in the build time.

Configuration
-------------
Examples of configuration that can be changed is in
[sakai/tomcat](sakai/tomcat). Configuration is added when the image is built,
using [`ADD`](https://docs.docker.com/engine/reference/builder/#add). An example
of this being used for Tomcat's `context.xml` can be found in the [Sakai
Dockerfile](https://github.com/themichaellai/sakai-docker-compose/blob/4a88134126df567cd9f1e72cd4bb2e8a87589e2b/sakai/Dockerfile#L41).
Additional files can be added by adding the files in this repository, and adding
the proper `ADD` command in the corresponding Dockerfile. When this is done,
`docker-compose build` needs to be re-run in order to rebuild the images.

IDE Integration
---------------
We have included a [Dockerfile for an IDE (Eclipse)](eclipse/Dockerfile) that
will launch a full version of Eclipse Mars that should provide source editing
capabilities. However, due to limitations of some of the plugins that come with
Eclipse, features such as deploying to Tomcat from the editor are not possible
when the containers containing Eclipse and Tomcat are separate, because the
Eclipse Tomcat plugin expects Tomcat to be running on `localhost`. Therefore,
the only way to fully integrate Eclipse and Tomcat/Sakai would be to add Eclipse
to the same container as Tomcat. Alternatively, if source code sharing
(mentioned in this same document) is used, then one can get a shell into the
Tomcat/Sakai container by running `docker exec -it sakaidockercompose_sakai_1
bash`, and using this to manually run Maven commands.
