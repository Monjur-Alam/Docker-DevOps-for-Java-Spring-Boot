# Docker DevOps for Java Spring Boot
![image](https://user-images.githubusercontent.com/40492085/201487312-88550969-2b32-4ca0-9d53-40a28945aa79.png)
### Docker-command-reference

Pull image

      docker pull image_name

To show image list

      docker images

Tag name change of docker image

      docker tag REPOSITORY:TAG REPOSITORY:NEW_TAG

Search any container in an image

      docker search image-name

To show image history

      docker image history image_id

Docker image inspect (show details)

      docker image inspect image_id

To run a container

      docker run --name container-name -p 7090:80 -d --restart=always nginx

- --name => container name
- -p => port binding
- -d => detached mode (background running not foreground )
- --restart => restart policy and 'always' value start container when docker desktop restart, default value 'no'. 
- nginx => image name

Use your container name instead of demo_name

      docker container exec 

Pause container 

      docker pause container_name

Resume container 

      docker unpause container_name

Inspect container

      docker inspect container_name

Stop container

      docker stop container-name 

Remove container

      docker rm container-name/id 

Remove all stopped container 

      docker container prune

To show the all-container/processes list

      docker ps -a

To show only running/active container 

      docker container ls

To stop a running  container 

- Press ctrl + C

Check all process of a specific container

      docker top container_name

Show all stats of docker

      docker stats

Docker process stats config

      docker run -p 5001:5000 -m 512m --cpu-quota -d image_name           // 100000 = 100%,  50000 = 50%

Docker system details

      docker system df

Building a Docker image Manually 

1. Build a JAR
2. Setup the prerequisites for running the JAR  `-openjdk:8-jdk-alpine`
3. Copy the JAR
4. Run the JAR

Your java project run as -> Maven Build. 
Open terminal and go to the project directory path- 

      cd /Users/macbookpro/eclipse-workspace/docker-crash-course-master/01-hello-world-rest-api

Run docker base image  `--openjdk:8-jdk-alpine`

      docker run -dit openjdk:8-jdk-alpine

Check `.jar` file in temp folder-

      docker container exec goofy_noether ls /tmp

Copy `.jar` file into the container 

      docker container cp target/hello-world-rest-api.jar goofy_noether:/tmp 

Check again `.jar` file in temp folder you will 

      docker container exec goofy_noether ls /tmp

      docker container commit goofy_noether monjur/hello-world-rest-api:manual1 

check images 

      docker images 

Run docker 

      docker run monjur/hello-world-rest-api:manual1
      docker ps -a
      docker container commit --change='CMD ["java","-jar","/tmp/hello-world-rest-api.jar"]'  goofy_noether monjur/hello-world-rest-api:manual2
      docker run -p 8005:8080 -d monjur/hello-world-rest-api:manual2

Use docker file to build docker image 

      FROM openjdk:8-jdk-alpine 
      ADD target/hello-world-rest-api.jar hello-world-rest-api.jar
      ENTRYPOINT ["sh", "-c", "java -jar /hello-world-rest-api.jar"] 
