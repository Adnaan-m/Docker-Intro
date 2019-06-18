# Introduction to Docker  

 

## Introduction  
The following ReadME displays information on useful snippets of Bash scripts, Docker commands and processes to create images, containers, dockerfiles and docker-compose files along with what they do. 

## Useful Commands
### Docker commands
>**Docker images** -  *check images created on machine*
>
>**Docker container ls**   -   *Lists running open containers*
>
>**Docker ps** - *Shows running containers*
>
> **Docker exec -it <Container Name> sh** - SSH's into docker dontainer.
> 
> **Docker network inspect** ~Network Name~ - Shows configuration file for network
> 
> **Docker stop $(docker ps -a -q)** - Stop all running containers   
> 
> **Docker rm $(docker ps -a -q)** - delete all running containers  
> 
> **Docker rmi $(docker images -q -a)** - Delete all existing Images
> 
> **docker logs ~Container ID~** - Gives a log of running container.



## Docker Image Process

- Create Application Folder 
> Minimal files Include : something.js , index.html , lookgood.css , **Dockerfile** , **Docker-compose** .

- CD into folder

- Build the image using the Dockerfile configuration. 

```bash
Docker build -t <name of image> . 
``` 
> **-t**   -   *tag option to name image builds*.   

 **Docker image ls** - to view the image.

- Then run a container using the image from the dockerfile. Two examples are given below, one simple and one a little more expanded.   

```bash
Docker run -d <name of image created> 
```   

```bash
Docker run --name <Make up a Name> --network <Network ID> -p <Host Machine Port>:<Container Port> -d <Image Name>
  
>> **--network** Allows containers to talk to each other across all ports.  
>>  **-p** Allows ports to be exposed.  
>> **-d** Runs image in detached mode.  
``` 

> **Docker container ls** OR **docker ps** - to view the running containers.
>> **-a** Views containers that are not in operation anymore.

- The Application should be up and running on web browser (if a web app).  

- If a database container is spun up, you can go inside using the following bash script: 

```
docker exec -it <Container Name> sh
```
>**-it**  Connects to the docker terminal.   


## Dockerfile

The Dockerfile is the configuration file for the images created through the applications. Creating and running a Dockerfile will download all the dependancies and packages required for the containers to work effectively. An example of a simple Dockerfile is shown below.   

``` docker
------------------
FROM node:8-alpine  
WORKDIR /usr/src/app   
ADD . /usr/src/app   
RUN npm install   
EXPOSE 4000   
CMD ["npm", "start"]
------------------

FROM - Sources from pre-created image (This one sourced from DockerHub). Generally the platform you want to run the application on i.e. Ubuntu, RedHat, Alpine etc.
WORKDIR - Creates directory in the container.
ADD - Adds what you want from your local machine to the container in the specified folder. In this case, it is copying everything (.) to the directory created (/usr/src/app).
RUN - Do something command.
EXPOSE - exposes the specified port.
CMD - Command execution within container ["param 1", "param 2"].
```
## Docker-Compose
When you have created images that are fully functional, you can use the "docker-compose.yml" file to quickly deploy images with added configuration. An example is shown below for a nodejs application that is linked to a mongo database.

``` yaml
version: '2'
networks:
  default:
    external:
      name: alpinenjs_bridge

services:
  mongodb:
    image: mongo:4.0.4
    volumes:
      - './data:/data/alpine'
    restart: always
  app:
    build: ./
    image: alpineplatformv2    
    ports:
      - "4000:4000"
    restart: always
    links:
      - mongodb
```

To deploy a docker-compose file, type the following:   

```bash
Docker-compose up
```
