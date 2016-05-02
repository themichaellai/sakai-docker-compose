Docker Compose Continuous Integration w/ Jenkins
============================

Continuous Integration is complement that allows you to rebuild images whenever you push a new change to the main github repository for sakai. After a push or change is made, Jenkins will run Docker Compose to create a new docker image for sakai.

Running Instructions to Setup Docker CI
----------------------------------------------
The Docker container is set off by changes pushed to the main sakai repository 
[sakaiproject/sakai](https://github.com/sakaiproject/sakai), Jenkins default builds this current repository's Dockerfile on changes.



Make sure you have Java-8 installed or oracle Jdk-8 above
Create new Dockerfile: Add in below:

```
FROM jenkins:1.596
 
USER root
RUN apt-get update \
      && apt-get install -y sudo \
      && rm -rf /var/lib/apt/lists/*
RUN echo "jenkins ALL=NOPASSWD: ALL" >> /etc/sudoers
RUN apt-get update
RUN apt-get install -y libapparmor-dev
 
USER jenkins
COPY plugins.txt /usr/share/jenkins/plugins.txt
RUN /usr/local/bin/plugins.sh /usr/share/jenkins/plugins.txt
RUN sudo apt-get update
RUN sudo apt-get install -y libapparmor-dev
```


then run `touch plugins.txt`

Edit the file and add 

scm-api:latest
git-client:latest
git:latest
greenballs:latest


Create the docker-image by running `docker build -t myjenk`.

Then run 
```bash
docker run -d -v /var/run/docker.sock:/var/run/docker.sock \
                -v $(which docker):/usr/bin/docker -p 5050:8080 myjenk
```

Go to localhost:5050

Manage Jenkins -> Manage Plugins and download Git, Github Plugin and add your repo
Manage Jenkins -> Configure Security -> Enable Security -> Jenkin's own user database & Anyone can do anything

Install latest version of ngrok and extract it. Change into the folder that it's extracted out in and run:
`./ngrok http 5050`, mark down the new address that is created for your localhost:5050

Manage Jenkins -> Fill out Git user.name and user.email that you registered in the command line through 

git config --global user.email = email@example.com
git config --global user.name = "Random Name"


Make sure you have the Address of Jenkins as the ngrok address 

On build job, make sure to add the proper Github repo of the main sakai that is being changed that you want to continuously integrate under Github Porject and under Git URL. MAKE SURE TO ADD a / AT THE END OF ADDRESS so it's like https://github.com/sakai.git/ <- last slash is important

Check "Trigger builds remotely" and add LONG_RANDOM_TOKEN as authentication token

Add execute shell commands under Build where the github link below is your repo with the Dockerfile



```bash
rm -rf sakai-docker-compose
git clone https://github.com/themichaellai/sakai-docker-compose.git 
sudo docker-compose up
rm -rf sakai-docker-compose
```

Go to Github, go to Settings tab under the main Github Repo, and click on Webhooks & Services and then add Webhook and then add the payload URL as Ngrokwebaddress.ngrok.io/job/JOBNAME/build?token=LONG_RANDOM_TOKEN

Change content type to application/x-www-form-urlencoded

Press Send me everything.

and check Active then update webhook. If everything went well you should have a working CI!
Optional: 
==========
Dockerhub
----------
Make a dockerhub account and push the docker images to your account to keep track of Docker images without manually removing them

Security
----------
Manage Jenkins -> Enable Security -> Pick Jenkins own user database -> Check Allow users to sign up 
Under Authorization pick Matrix-based security and add users, define securities and restrictions. Finally click save at the bottom.
When you send the authentication in github with the webhook, you need to add USERNAME:PASSWORD@hostname/
Press Save and you're done -> CAREFUL, if you uncheck authorization in the matrix you'll effectively lock yourself out so uncheck the authorization button last when you're setting up user configuration
