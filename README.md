# Table of Content
 [Chapter 1](#Chapter-1)<br/>
 [Chapter 2](#Chapter-2)<br/>
 [Docker useful commands](#Docker-useful-commands)<br/>
 [Playing with Ubuntu Image](#Playing-with-Ubuntu-Image)<br/>
 [Attache and detach mode](#Attache-and-detach-mode)<br/>
 [Exploring docker container file system](#Exploring-docker-container-file-system)<br/>
 [Attach to STDIN and to Terminal](#Attach-to-STDIN-and-to-Terminal)<br/>
 [Port mapping](#Port-mapping)<br/>
 [Data persistance](#Data-persistance)<br/>
 [Finding base image of an image](#Finding-base-image-of-an-image)<br/>
 [Environment variable](#Environment-variable)<br/>
 [Command vs Entrypoint](#Command-vs-Entrypoint)<br/>
 [Container Commnication](#Container-Commnication])<br/>
  &nbsp;&nbsp;[--link](#--link])<br/>
  &nbsp;&nbsp;[bridge network](##bridge-network])<br/>
 
 
 
# Chapter 1
1. different tools like mysql, nodejs may require different version of os. - this problem is solved with docker
2. There is also a situation where one tool requires one version of certain library from and another tool another version - this problem is also sloved with docker

3. Every container is an isolated enviourment, with its own networking and own file system. 
4. We can just download the image and use command 'docker run <IMAGE NAME>'
5. docker run <IMAGE NAME>: is a shortcut for commands 'docker container create', 'docker container start'
6. running 'docker run <IMAGE NAME>' multiple times will creates multiple containers
7. in a docker image a process must be running otherwise it will exit immidietly that is why when we run _docker run ubunut_ it will exited immidietly  

- in hypervisor multiple os can be ran even though they do not share the same os kernal, however in Docker multiple container can be run in parellel only when they are based on the same kernal

  for eg. all linux kernal are same, so multiple linux container can be run in parallel on a linux machine, but not on a windows machine.  

- Docker image: is a like a template, which is a read only snapshot. 
- Docker container: is a running instance of an image, which is read and write. 

  docker imagea and container https://phoenixnap.com/kb/docker-image-vs-container
  
# Chapter 2
- Docker is available in two editions: i) docker community edition ii) docker enterprise edition
- For installing docker on windows and mac use 'docker desktop' for windows and mac resp. 

# Docker useful commands
___docker run nginx___:  run the image nginx from host if not present then fetch from the repositry and run. 

___docker pull nginx___: will only pull the image from repositry and store on our machine

___docker ps___: will give information about all the running containers for eg. id, name etc. The id and name are random, given by docker.

___docker ps -a___:  see all the running and previously exited containers 

***docker stop <EITHER CONTAINER NAME OR ID\>*** : stops the running container

___docker rm <NAME OF CONTAINER\>___: permanently removes one or more container and frees the space

___docker images___: list all the available images at host and the size they occupies
        
___docker rmi <NAME OF IMAGE\>___: remove the image, before using thing command make sure no container is running of that image

___docker start <NAME OR ID OF THE CONTAINER\>___:starting an existing container and when we start a container again then port mapping or enviourment variable can 
                                            not be done on that those options are not available on start command. 
                                            For that we have to create a new image from the existing container so our changes are also there and run this new 
                                            image with -p option. As mentioned here: https://stackoverflow.com/questions/19335444/how-do-i-assign-a-port-mapping-            
                                            to-an-existing-docker-container


___docker stop <NAME OR ID OF THE CONTAINER\>___: 

# Playing with Ubuntu Image
___docker run ubuntu___ : when you run this command you will notice with 'ps -a' that it goes into the exited state right away.
                    That is because container are not supposed to run an operating system like virtual machines, they are
                    infact supposed to run process like webservice, database servers and other kind of servers or some
                    computational process. A container lives as long as the process inside container lives.
                    That is why 'docker run ubuntu' is exited immidelty because there is no defalt process running inside it.
                    
___docker run ubuntu seleep 5___: will run the sleep command on ubuntu and keep the container in running state for 5 secs.

___docker run ubuntu bash___: will get you into the bash and then you can run all linux commands, exit will exit the bash and you will back an docker console.

# Attache and detach mode
___docker run <IMAGE-NAME\>___: this will attache the console and print all the output comes from the image. User here will not be able to do anyting else. 
                                Ctrl + c will stop the container. Stdin, stdout and stderr are attached here. 

___docker run -d <IMAGE-NAME\>___: this will run the image in deattached mode control will be returned to the main console. This will command will return the id of the container. Even after closing the terminal containers will still be rnning in background and could be enquired with docker ps. 


___docker attach <ID or NAME OF CONTAINER\>___ : if you want to attach the console to a running container then just use this command. Any command which takes docker ID only few first chars of the long id can be mentioned in the command instead of the whole id.

___docker exec <OPTIONS\> <CONTAINER: ID OR NAME\>___: command : this command will be executed on the running container


# Exploring docker container file system
https://www.baeldung.com/ops/docker-container-filesystem

___docker run -it ubuntu___: shell is alredy started here

___docker run -it jenkins___: this command will start the jenkins but not the bash. So we can not explor through the filesystem

___docker exec -it <ID or NAME OF CONTAINER> /bin/bash___ : once the container is running in dettached mode, then we can use this command to explore through the file system or run any command on bash terminal

___docker export -o hello.tar  <ID or NAME OF CONTAINER\>___ : this will make the filesystem of a stopped container in hello.tar file
docker cp  <ID or NAME OF CONTAINER>:/ ./test : this will copy the the filesystem of a stopped container from / to ./test

___docker run redis___ : this will fetch the latest version of redis and run on docker. The different version of an image can be seen on docker hub or whereever the repository is.
                   
___docker run redis:<TAG\>___  : here the tag version can be mentioned in order to run a specific version or tag. for eg. docker run redis:4.0

# Attach to STDIN and to Terminal
- if we have a .sh script which prints on the terminal "please enter your name:" and then user enters his/her name through  stdin and then the
script prints back the whole "please enter your name:<entered name>".

- In such cases we need to start the container in intractive mode (-i) and with attached terminal (-t)

___docker run -it <NAME OF CONTAINER\>___ 

___docker run -p 38282:8080 --name blue-app -e APP_COLOR=blue -d kodekloud/simple-webapp___


# Port mapping
- Docker Host: its the machine on which docke is running, let say its ip is 192.168.5.10
- When we run a container then every container gets an internal ip (172.17:0.2)
- We run a container 'docker run simplewebapp' which will run on port 5555  
- Most of the container use bridge network adoptor, which can be check by using 'docker inspect <CONTAINER NAME OR ID>'
- By using the container ip from the host machine browser we will be able to access the 'simplewebapp'for eg.: http://172.17:0.2:5555 
- However we will not be able to access 'simplewebapp' by using host machine ip address for eg.: http://192.168.5.10:5555 will return nothing
- In order to access it we need port mapping like this: docker run -p 5555:5555 simplewebapp [docker run -p <HOST PORT>:<CONTAINER PORT> simplewebapp]
- Multiple host ports can be mapped at once 'docker:- run -p 8080:8080 -p 50000:50000 -v /your/home:/var/jenkins_home jenkins'

- ___docker run -p 3306:3306 mysql___

# Data persistance
- Every docker container has its own file system, its same as the host machine.
- If you delete the container then all the data it holds will also be gone
- For example if we run mysql container then it will store the date in container at /var/lib/mysql
- Now if we delete this container all the data stored at /var/lib/mysql will also be gone
- A container supposed to be light weight, so that it can start and stop faster that is why its important to do volume mapping
- To solve this problem we an do directory mapping from host to docker. 
- with docker run -v /opt/datadir:/var/lib/mysql now docker will store the data at host machine at location /opt/datadir
- now we can delete the container and the will still be safe at /opt/datadir.

However:
- If we do not want to mapp the volume then the same container can be started again with command
  ___docker run -ia <CONTAINER: ID OR NAME>___: all the data which was stored here, will still reside inside the container

- ___docker inspect <CONTAINER NAME\>___: this will give all the information about the docker container in json fomat

- ___docker logs <CONTAINER NAME\>___: will show the log output of the application, which was logged at stdout

# Finding base image of an image
___docker run python:3.6 cat /etc/\*release\* ___:  lets says our image is python:3.6

# Environment variable
___docker run -e <NAME-OF-ENVIRONMENT-VARIABLE\>:<VALUE-OF-ENVIRONMENT-VARIABLE\>___: Environment variable can be passed while running an image

___docker inspect <NAME-OF-THE-CONTAINER-OR-ID\>___: inspecting Environment vairable on an running image

___docker run -p 38282:8080 --name blue-app -e APP_COLOR=blue -d kodekloud/simple-webapp___
                 
# Command vs Entrypoint
___CMD___: at the end of the docker image we define a command which starts the program, which runs within the container. This command can be defined into the following two formats:
```
plain format: CMD command param1                CMD sleep 5
json  format: CMD["command", "param1"]          CMD["sleep", "5"]    
```
lets say with the above command we build an image called *ubuntu-sleeper* now when we run *docker run ubuntu-sleeper* then this image will sleep for 5 sec and then exit, but what if we want to to seep for 8 sec. For that we will have to type *docker run ubuntu-sleeper sleep 8*, now our *CMD["sleep", "5"]*  will be replaced with CMD["sleep", "8"] and image will sleep for 8 sec. 

___Entrypoint___: what if we just want pass the parameter do the docker image instead of the whole command. So instead of *docker run ubuntu-sleeper sleep 8* this *docker run ubuntu-sleeper 8*.
Such things can achived with Entrypoint. So the entrypoint will append the parameter to the command, now we will define our entrypoint in the build file like following:
```
plain format: ENTRYPOINT command             ENTRYPOINT sleep 5
json  format: ENTRYPOINT["command"]          ENTRYPOINT["sleep"]    
```
now we can use *docker run ubuntu-sleeper 8* now 8 will be append to *sleep* and finally it will be *sleep 8*.
However if just run *docker run ubuntu-sleeper* then we will get the error "missing operands Try sleep --help for more information". 
To avoid this we can configure some default command. For this we can use the combination of Entrypoint and CMD:

```
ENTRYPOINT["sleep"] 
CMD["5"]
```
now the user can use *docker run ubuntu-sleeper* internally it will take default and it will at the end "sleep 5".

# Container Communication
## -- link
## bridge network

# Docker compose


