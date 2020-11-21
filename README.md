# Chapter 1
1. different tools like mysql, nodejs may require different version of os. - this problem is solved with docker
2. There is also a situation where one tool requires one version of certain library from and another tool another version - this problem is also sloved with docker

3. Every container is an isolated enviourment, with its own networking and own file system. 
4. We can just download the image and use command 'docker run <IMAGE NAME>'
5. docker run <IMAGE NAME>: is a shortcut for commands 'docker container create', 'docker container start'
6. running 'docker run <IMAGE NAME>' multiple times will creates multiple containers

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











                    

