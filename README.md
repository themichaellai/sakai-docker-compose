Docker Final Project
===================
_Shelley Vohr, Michael Lai, and Brian Zhou_

#### What's Docker?

Docker is a system allows you to package an application with all of its dependencies into a standardized unit for software development. Docker containers wrap up a piece of software in a complete filesystem that contains everything it needs to run: code, runtime, system tools, system libraries. It's anything you can install onto a server, and is lightweight, open, and secure. It's standalone, and thus will always run the same no matter the environment it's in. 

#### What Did We Work On?

All three of us collaborated to build the initial Dockerfile responsible for running Eclipse, and then each took a subtask to create the final project.

#####**Michael**

Split the initial Dockerfile into Docker-Compose files.

#####**Shelley**

Integrated Eclipse IDE into the Docker-Compose files.

#####**Brian**

Created Continuous Integration system with Jenkins that built Sakai image whenever new changes were pushed to the main repo. 

#### How Does it Run?
Read the files in the below links to run both discrete parts of the project.

[Continuous Integration](https://github.com/themichaellai/sakai-docker-compose/blob/master/docker-continuous-integration-readme.md)

[Docker Compose & Eclipse](https://github.com/themichaellai/sakai-docker-compose/blob/master/docker-compose-readme.md)




