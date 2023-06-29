# Building and Deploying a Dockerized User Profile App

Welcome to my project documentation, where I will guide you through the process of building and deploying a user profile application using Docker. By containerizing each component of the app, we ensure consistency and portability, making it easier to deploy in various environments. This documentation serves as a comprehensive guide for Junior DevOps Engineers, showcasing their skills in Docker and containerization.

## Introduction

In my quest to learn DevOps, I have been exploring Docker, a powerful tool for containerization. Containers provide an isolated and consistent runtime environment for applications, allowing for easier deployment and scalability. Throughout this project, I have leveraged Docker to set up a user profile app that consists of a front-end, a Node.js backend, and a MongoDB database. All components are containerized, providing a streamlined and reproducible development environment.

## Project Setup

To begin, I utilized a project spearheaded by [Nana Janashia](https://www.linkedin.com/in/nana-janashia/?originalSubdomain=at) available on [GitLab](https://gitlab.com/nanuchi/developing-with-docker). The project showcases a simple user profile app built with pure JavaScript, CSS styles, a Node.js backend using the Express module, and MongoDB for data storage. The entire application is container-based, ensuring consistency and ease of deployment.

## Docker Basics

Before diving into the project implementation, it's important to understand some Docker basics. Docker uses images and containers as its core concepts:

1. **Image**: A lightweight, standalone, and executable software package that includes everything needed to run a piece of software, such as code, runtime, libraries, environment variables, and configuration files.

2. **Container**: An instance of an image that runs as a process on the host operating system. Containers are isolated from each other and provide a consistent runtime environment for applications.

## Development Process

Let's walk through the development process step by step:

1.**Pulling Images**: I started by pulling the necessary Docker images from Docker Hub. This involved pulling the MongoDB image and the MongoDB Express image using the docker pull command. Some of the commands uses here are:

```docker
docker pull node    

docker pull  mongo-express 
```

![List of Pulled Images](/projects_images/1.%20listing%20docker%20images%20after%20pulling.png)

2.**Creating a Docker Network**: To enable communication between containers, I created a Docker network using the docker network create command. This network allows the MongoDB container and the MongoDB Express container to communicate with each other.

```docker
docker network create mongo-network

```

![Creating the docker network](/projects_images/4.created_the_docker_mongo_network.png)

3.**Running the MongoDB Container**: I ran the MongoDB container with the appropriate configuration. Using the docker run command, I specified the necessary environment variables, such as the root username and password, and connected the container to the Docker network.

```docker
docker run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=noble -e MONGO_INITDB_ROOT_PASSWORD=0242ann --name mongodb --net mongo-network mongo


```

### Explanation of the above Command

* **docker run:** This command is used to run a Docker container.

* **-d:** This option stands for "detached mode." It runs the container in the background, allowing you to continue using the terminal without being attached to the container's console.

* **-p 27017:27017:** This option maps the host's port 27017 to the container's port 27017. It allows communication between the host and the container through this port. Port 27017 is commonly used for MongoDB.

* **-e MONGO_INITDB_ROOT_USERNAME=noble:** This option sets an environment variable named MONGO_INITDB_ROOT_USERNAME with the value noble. It specifies the root username for MongoDB. The root user has administrative privileges.

* **-e MONGO_INITDB_ROOT_PASSWORD=0242ann:** Similarly, this option sets the environment variable MONGO_INITDB_ROOT_PASSWORD to the value 0242ann. It specifies the root password for MongoDB. The root user requires this password for authentication.

* **--name mongodb:** This option assigns the name "mongodb" to the running container. The name can be used to reference the container in subsequent Docker commands.

* **--net mongo-network:** This option connects the container to a Docker network named "mongo-network." The network must be created beforehand using the docker network create command. It allows communication between containers within the same network.

* **mongo:** This is the name of the Docker image to run the container from. It refers to the official MongoDB image available on Docker Hub. The container is based on this image and contains the MongoDB database system.
*In summary, this Docker command runs a MongoDB container in detached mode, mapping the host's port 27017 to the container's port 27017. It sets the root username and password for MongoDB, assigns the name "mongodb" to the container, and connects it to the "mongo-network" Docker network.*

![Running the MongoDB container](/projects_images/5.%20running%20the%20mongo%20doker%20container%20and%20naiming%20it%20as%20mongodb.png)
*Due to future challenges in the project, the usenrmane was changed to admin and the password was set to password in order to have a smooth project. The challange entcounted will later be stated in the project*

4.**Running the MongoDB Express Container**: Similarly, I ran the MongoDB Express container and connected it to the Docker network. This container provides a web-based interface for managing the MongoDB database.

```docker
docker run -d -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=noble -e ME_CONFIG_MONGODB_ADMINPASSWORD=0242ann --net mongo-network --name  mongo-express -e ME_CONFIG_MONGODB_SERVER=mongodb mongo-express


```

#### Explanation of the above command

* **-d:** This flag stands for "detached mode." It runs the container in the background, allowing you to continue using the terminal without being attached to the container's console.

* **-p 8081:8081**: This option maps the host port 8081 to the container's port 8081. It enables you to access the containerized application using <http://localhost:8081> in your web browser.

* **-e ME_CONFIG_MONGODB_ADMINUSERNAME=noble**: This option sets an environment variable ME_CONFIG_MONGODB_ADMINUSERNAME to the value noble. This variable is used by the containerized application to specify the MongoDB admin username for Mongo Express, a web-based MongoDB admin interface.

* **-e ME_CONFIG_MONGODB_ADMINPASSWORD=0242ann:** Similarly, this option sets the environment variable ME_CONFIG_MONGODB_ADMINPASSWORD to the value 0242ann. It specifies the MongoDB admin password for Mongo Express.

* **--net mongo-network:** This option connects the container to a Docker network named mongo-network. The network must be created beforehand using the docker network create command. It allows communication between containers within the same network.

* **--name mongo-express:** This option assigns the name "mongo-express" to the running container. The name can be used to reference the container in subsequent commands.

* **-e ME_CONFIG_MONGODB_SERVER=mongodb:** This option sets the environment variable ME_CONFIG_MONGODB_SERVER to the value mongodb. It specifies the hostname or IP address of the MongoDB server that Mongo Express should connect to.

* **mongo-express:** This is the name of the Docker image to run the container from. It refers to the official Mongo Express image available on Docker Hub

*By running this command, you start a Docker container running Mongo Express, which provides a web-based interface for administering a MongoDB server. The container is configured with the specified environment variables, allowing it to connect to the MongoDB server and authenticate with the provided admin credentials. The container's port 8081 is mapped to the host's port 8081, enabling access to the Mongo Express interface from your web browser.*

![Running Mongo Express Image](/projects_images/6.mongo_express.png)

This will be the interface of the mongo-express container service that is connected to the mongodb instance running in the background
![Interface of mongo-express running in the web interface](/projects_images/7.connected_database_successfullyy.png)

5.**Connecting the App to MongoDB**: With the database containers up and running, I connected the user profile app to the MongoDB container. This involved configuring the app to use the appropriate MongoDB connection string.

```js
/ use when starting application locally with node command
let mongoUrlLocal = "mongodb://admin:password@localhost:27017";

// use when starting application as a separate docker container
let mongoUrlDocker = "mongodb://admin:password@host.docker.internal:27017";

// use when starting application as docker container, part of docker-compose
let mongoUrlDockerCompose = "mongodb://admin:password@mongodb";
```

6.**Connecting the App to MongoDB**: With the database containers up and running, I connected the user profile app to the MongoDB container. This involved configuring the app to use the appropriate MongoDB connection string.
With the running of the command

``` js
node server.js
```
The app began running on port 3000 and then connected to the DB

![Running the App with the node server.js Command](/projects_images/10.%20app%20is%20running%20on%20port%203000.png)

Running instance of the App

![Running instance of the app](/projects_images/11.%20app%20runnig.png)

7.**Testing the App:** To ensure the app is working perfectly well, I tested it by editing the edit profile option which generated the below information in the app
![Testing the app](/projects_images/12.testing%20the%20app%20by%20updating.png)

8. **Monitoring the app:** In ordfer to view the details of the containder, I made use of the logs command which gives details about the logs of the containers as shown below:

![Monitoring the App](/projects_images/13.%20monitorinf%20app.png)

## Docker Compose for Simplified Management

To simplify the management of containers and their dependencies, I created a Docker Compose file. The Docker Compose file provides a declarative way to define and run multi-container Docker applications.

**Here's the content of the Docker Compose file:**

```docker
version: '3'
services:
  mongodb:
    image: mongo
    ports:
      - 27017:27017
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=password
    volumes:
      - mongo-data:/data/db

  mongo-express:
    image: mongo-express
    restart: always
    ports:
      - 8080:8081
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
      - ME_CONFIG_MONGODB_ADMINPASSWORD=password
      - ME_CONFIG_MONGODB_SERVER=mongodb
    depends_on:
      - mongodb
    volumes:
      - mongo-data:

volumes:
  mongo-data:
    driver: local

```

**Diving Deeper into the Docker Compose Content**

---

* **version: '3':** This specifies the version of Docker Compose file format being used.

* **services:** This section defines the services to be run as containers.

    ```test
    mongodb: This service is based on the mongo image, which is the official MongoDB image available on Docker Hub. It exposes port 27017 and sets environment variables for the root username and password. The container's data directory is stored in the mongo-data volume.
    ```

    ```test
    mongo-express: This service is based on the mongo-express image, which provides a web-based interface for administering MongoDB. It always restarts if the container stops. It exposes port 8080 and sets environment variables for the admin username, password, and the MongoDB server to connect to. It depends on the mongodb service being up and running. It also uses the mongo-data volume.
    ```

* **volumes:** This section defines the named volume mongo-data that is used by both services to persist data.

    ```text
    mongo-data: This named volume is of type local, meaning it is stored on the local filesystem of the Docker host.
    ```
This Docker Compose file sets up a MongoDB container (mongodb) and a Mongo Express container (mongo-express), connecting them together. The containers use environment variables for authentication and configuration. Data is persisted using a named volume. With this setup, you can easily start and manage both MongoDB and Mongo Express by running docker-compose up.






