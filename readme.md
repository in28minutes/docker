# Docker in 5 Steps

Let's learn Docker in 5 Easy Steps. 

- Step 00 - Installing Docker
- Step 01 - A simple Docker use case - Run an existing application
- Step 02 - Playing with Docker - Containers and Images
- Step 03 - How does Docker work?
- Step 04 - Manually creating a docker image
- Step 05 - Dockerizing a Spring Boot Application using Dockerfile and Spotify Maven Plugin

### Step 00 - Installing Docker

- https://docs.docker.com/install/

### Step 01 - A Simple Docker User Case - Run an existing application

- https://hub.docker.com/u/in28min

```
docker run -d -p 5000:5000 in28min/todo-rest-api-h2:1.0.0.RELEASE
```

```
docker run -d -p 8761:8761 springcloud/eureka
```

```
docker run -d -e MYSQL_ROOT_PASSWORD=dummypassword -e MYSQL_USER=todos-user -e MYSQL_PASSWORD=dummytodos -e MYSQL_DATABASE=todos --p 3306:3306 mysql:5.7
```

#### Traditional Deployment

![Traditional Deployment](images/docker-traditional-deployment.png)

#### Deployment with Docker

![Docker Deployment](images/docker-zz-deployment.png)


### Step 02 - Playing with Docker - Containers and Images

- Image is static
- Container is dynamic
```
  579  docker run -d -p 5000:5000 in28min/todo-rest-api-h2:1.0.0.RELEASE
  580  docker images
  581  docker container ls
  582  docker container stop sleepy_ramanujan
  583  docker container ls
  584  docker container ls -a
  585  docker container rm 8268f96264f8
  586  docker run -d -p 5000:5000 in28min/todo-rest-api-h2:1.0.0.RELEASE
  587  docker container logs 380a1bb1dbc802dc83c7ab58f4e35cdf93a757bd0e05e74b9bbde7919d4d7d2b
  588  docker images
  589  docker container -aq
  590  docker container ls
  591  docker container ls -aq
  592  docker container ls -l
  593  docker container ls
  594  docker container prune
  595  docker container inspect 380a1bb1dbc8
  596  docker container diff 380a1bb1dbc8
  597  docker image history 
  598* docker image
  599  docker image history f8049a029560
  600  docker image inspect f8049a029560
  601  docker images ls
  602  docker images
  603  docker image rm f8049a029560
```

### Step 03 - How does Docker work?

#### Docker Architecture

![Docker Architecture](images/docker-architecture.png)

### Step 04 - Manually creating a new docker image

```
ID=$(docker run -dit openjdk:8-jdk-alpine)
docker container cp spring-boot-rest-api-playground-h2-0.0.1-SNAPSHOT.jar $ID:/tmp
docker container commit --change='CMD ["java", "-jar","/tmp/spring-boot-rest-api-playground-h2-0.0.1-SNAPSHOT.jar"]' $ID  in28min/spring-boot-manual:v2
docker container run -p 5000:5000 in28min/spring-boot-manual
```

### Step 05 : Containerizing Spring Boot Application using Dockerfile and Spotify Maven Plugin

Run com.in28minutes.rest.webservices.restfulwebservices.RestfulWebServicesApplication as a Java Application.

- mvn package
- docker run in28min/todo-rest-api-h2:0.0.1-SNAPSHOT
- docker run -p 5000:5000 in28min/todo-rest-api-h2:0.0.1-SNAPSHOT
- docker run -p 5000:5000 in28min/todo-rest-api-h2:1.0.0.RELEASE

#### Troubleshooting

- Problem - Caused by: com.spotify.docker.client.shaded.javax.ws.rs.ProcessingException: java.io.IOException: No such file or directory
- Solution - Check if docker is up and running!
- Problem - Error creating the Docker image on MacOS - java.io.IOException: Cannot run program “docker-credential-osxkeychain”: error=2, No such file or directory
- Solution - https://medium.com/@dakshika/error-creating-the-docker-image-on-macos-wso2-enterprise-integrator-tooling-dfb5b537b44e


#### Creating Containers

To test execute API at http://localhost:5000/users/in28minutes/todos.

```
docker login
```

#### Hello World URLS

- http://localhost:5000/hello-world

```txt
Hello World
```

- http://localhost:5000/hello-world-bean

```json
{"message":"Hello World - Changed"}
```

- http://localhost:5000/hello-world/path-variable/in28minutes

```json
{"message":"Hello World, in28minutes"}
```


#### Todo JPA Resource URLs

- GET - http://localhost:5000/jpa/users/in28minutes/todos

```
[
  {
    "id": 10001,
    "username": "in28minutes",
    "description": "Learn JPA",
    "targetDate": "2019-06-27T06:30:30.696+0000",
    "done": false
  },
  {
    "id": 10002,
    "username": "in28minutes",
    "description": "Learn Data JPA",
    "targetDate": "2019-06-27T06:30:30.700+0000",
    "done": false
  },
  {
    "id": 10003,
    "username": "in28minutes",
    "description": "Learn Microservices",
    "targetDate": "2019-06-27T06:30:30.701+0000",
    "done": false
  }
]
```

##### Retrieve a specific todo

- GET - http://localhost:5000/jpa/users/in28minutes/todos/10001

```
{
  "id": 10001,
  "username": "in28minutes",
  "description": "Learn JPA",
  "targetDate": "2019-06-27T06:30:30.696+0000",
  "done": false
}
```

##### Creating a new todo

- POST to http://localhost:5000/jpa/users/in28minutes/todos with BODY of Request given below

```
{
  "username": "in28minutes",
  "description": "Learn to Drive a Car",
  "targetDate": "2030-11-09T10:49:23.566+0000",
  "done": false
}
```

##### Updating a new todo

- http://localhost:5000/jpa/users/in28minutes/todos/10001 with BODY of Request given below

```
{
  "id": 10001,
  "username": "in28minutes",
  "description": "Learn to Drive a Car",
  "targetDate": "2045-11-09T10:49:23.566+0000",
  "done": false
}
```

##### Delete todo

- DELETE to http://localhost:5000/jpa/users/in28minutes/todos/10001


#### H2 Console

- http://localhost:5000/h2-console
- Use `jdbc:h2:mem:testdb` as JDBC URL 