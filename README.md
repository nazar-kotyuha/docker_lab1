# Dockerizing Note.js app

## Creating Dockerfile

1. FROM node:16 \
Defining from what image we want to build from. Used the latest LTS version 16 of node available from the Docker Hub.
2. WORKDIR /app \ 
Creating a directory to hold the application code inside the image, this will be the working directory for application.
3. COPY package*.json ./
Installing app dependencies. A wildcard is used to ensure both package.json AND package-lock.json are copied.
4. RUN npm install
This image comes with Node.js and NPM already installed so the next thing we need to do is to install app dependencies using the npm binary.
5. COPY . .
Bundling app's source code inside the Docker image, using the COPY instruction.
6. EXPOSE 80
App binds to port 80 (as given in the task) so using the EXPOSE instruction to have it mapped by the docker daemon.
7. CMD [ "node", "server.js" ]
Defining the command to run app using CMD which defines runtime. Here using 'node server.js' to start server.


## .dockerignore

Creating .dockerignore file to prevent some unwanted files from being copied onto Docker image.


## Building image

In the directory that has recently created Dockerfile, running the following command to build the Docker image. The -t flag lets you tag your image so it's easier to find later using the docker images command:

*docker build . -t kotyuha-nazar/docker-node-lab1*

## Run the image with memory and CPU limits

We can set the resource limits directly using the docker run command. Running image with -d runs the container in detached mode, leaving the container running in the background. The -p flag redirects a public port to a private port inside the container. By default, access to the computing power of the host machine is unlimited. We can set the CPUs limit using the cpus parameter. Also we can set the memory limit using the -m parameter 

Let's constrain our container to use at most two CPUs and, for instance, let's limit the memory that the container can use to 512 megabytes. 

Running the image previously built:

*docker run --cpus=2 -m 512m -p 49160:80 -d kotyuha-nazar/docker-node-lab1*

In the example above, Docker mapped the 80 port inside of the container to the port 49160 on machine.

So result will be discovered at http://localhost:49160/.

## Shut down the image 

In order to shut down started app, running the kill command. This uses the container's ID, which will be got from:

*docker ps*

in column CONTAINER ID

Kill command 

*docker kill <CONTAINER ID>*


## Pushing to Docker.hub

1. Creating a repository
To create a repository, signed into Docker Hub, selected Repositories then Create Repository.
2. Using docker login from the CLI, signed in using Docker ID
*docker loging*
3. Tagged image with created Docker ID using:
*docker tag kotyuha-nazar/docker-node-lab1 nazarkotyuhaa/docker-node-lab1* 
4. Pushed tagged private image to Docker ID namespace
*docker push nazarkotyuhaa/docker-node-lab1*

Result: 
https://hub.docker.com/repository/docker/nazarkotyuhaa/docker-node-lab1/general
