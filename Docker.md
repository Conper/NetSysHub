# DOCKER

## What is a container?
A **container** is a lightweight environment that allows applications to run in isolation on the same server, preventing interference between them. Compared to virtual machines, it occupies fewer resources and starts up more quickly.

<img src="https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fapachebooster.com%2Fkb%2Fwp-content%2Fuploads%2F2017%2F09%2Fdocker-container.png&f=1&nofb=1&ipt=73af7d6c7ce1ad69930dbc70a8591bd3cfce81cdd6be1de714e96ce3c36e642f&ipo=images">

## What is an image?

A **Docker image** is a file that contains everything needed to run an application in a container, including code, libraries, and dependencies. It acts as a template for creating containers and is essential in using Docker for developing and deploying applications.

## Virtualization

In Docker, only the application layer is virtualized, meaning it uses the host machine's operating system kernel. This helps save storage space and makes containers lighter.
<br><img src="https://www.informatique-mania.com/wp-content/uploads/2021/02/docker.jpg">

## Docker Desktop

**Docker Desktop** is an application that facilitates the management of Docker containers by providing a graphical interface for creating, running, and managing containers and images, simplifying the development and deployment of applications. Additionally, it includes tools like Docker Compose to define and run multi-container applications and an integrated development environment to ease working with Docker.

Installation page: [**Docker Desktop**](https://www.docker.com/products/docker-desktop/)

## Where can I download images?

We can download images from [**Docker Hub**](https://hub.docker.com/), which is a place where Docker container images are stored. This platform allows us to access a wide variety of images for our applications and services.

Here’s the translation to English:

## Docker Commands

### Image Management
List the images we have downloaded on our machine
```
docker images
```
> It will display the repository name, tag, image ID, creation date, and size.

Download an image
```
docker pull mariadb
```
```
docker pull mysql:9.1
```

> [!NOTE]  
> If the version is null, the latest version will be downloaded.
	
Delete an image
```
docker rmi NAME:TAG
```
```
docker rmi IMAGE_ID
```
> [!NOTE]  
> You can also use the following command:  
> `docker image rm NAME:TAG / IMAGE_ID`

### Container Management

Create a container
```
docker create IMAGE_NAME
```
> [!NOTE]  
> It will return the container ID.

Create a container with port mapping.
```
docker create -pHOST_PORT:CONTAINER_PORT --name MyDB IMAGE_NAME
```

Run a container
```
docker start ID/NAME
```

View the status of containers
```
docker ps
```
> It will display the container IDs, the image used, the command each container is running, the creation date, current status, the ports in use, and the container name.
```
docker ps -a
```
> It will also show inactive containers.

Stop a container
```
docker stop ID/NAME
```

Rename a container
```
docker create --name MyDB ID/CURRENT_NAME
```
> [!TIP]  
> Using the names you assign to your containers instead of the ID makes management easier and commands more readable and understandable.

Delete a container
```
docker rm ID/NAME
```

### View the logs of a container
```
docker logs ID/NAME
```
With real-time following
```
docker logs -f ID/NAME
```
With timestamps
```
docker logs --timestamps ID/NAME
```

Here’s the translation to English:

### Docker run

The `docker run` command combines the creation and execution of a container from an image into a single step, simplifying the deployment of applications, unlike `docker create`, which only defines it.

When running `docker run`, Docker follows these steps:

1. **Image Verification**: If the image is not found locally, Docker automatically downloads it from the configured repository (by default, Docker Hub).
2. **Container Creation**: It defines the container with the image and the provided options (ports, volumes, environment variables, etc.).
3. **Container Execution**: It starts the container and runs the command or application specified in the image.
4. **Output and Monitoring**: In attached (foreground) mode, it displays output in the terminal; in detached mode, the container continues running in the background.

This flow facilitates a quick and controlled deployment of applications in containers.

```
docker run mongo
```
To run it in the background
```
docker run -d mysql
```
With more options
```
docker run --name MyDB -p 3306:3306 -e MYSQL_ROOT_PASSWORD=my_password -d mysql
```
> [!NOTE]  
> The `-e` option in Docker is used to set environment variables within the container.

### Container Configuration

When creating a container, it is essential to configure environment variables to customize its operation. For example, in MySQL, the `MYSQL_ROOT_PASSWORD` variable is mandatory to set the password for the root user. These variables allow initializing applications with specific configurations and facilitate container management without the need to restart it.

In Docker Hub, each image details the required and optional environment variables in its documentation. These configurations are crucial to ensure that the application functions according to user needs. Consulting this information is essential for effectively and securely deploying applications in containers.

```
docker create -p27017:27017 --name myMongoDB -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=p4$$w0rd mongo
```

### Creating a Dockerfile

A **Dockerfile** is a text file that contains instructions for building a Docker image. It serves to automate the image creation process, specifying the environment, dependencies, and configurations necessary to run an application.

<font color="red">It must be named **Dockerfile**</font> (without an extension) because it is the standard name that Docker recognizes for automatically building images.

```
FROM node:23

RUN mkdir -p /home/app
COPY . /home/app     # "." Path of the host OS where the source code is located
EXPOSE 3000
CMD ["node", "/home/app/index.js"]
```
* `FROM`: Defines the base image for building the new image.
* `RUN`: Creates the `/home/app` directory in the image to store application files or data that will be used in the container.
> [!NOTE]  
> It creates the directory in the image, NOT on your physical machine.
* `COPY`: Transfers files and directories from the host machine to the container in the image.
* `EXPOSE`: Indicates that the container will listen on a specific port during its execution.
* `CMD ["node", "/home/app/index.js"]`: Sets the default command that will be executed when starting the container, where `node` is the executable and `/home/app/index.js` is the argument passed to the command.
> [!WARNING]  
> When using `CMD`, make sure to specify the absolute path of the file to execute. This ensures that the container can find and execute the file correctly.

For more options: [**Dockerfile**](https://docs.docker.com/reference/dockerfile/)

Create an image
```
docker build -t IMAGE_NAME:TAG /path/to/Dockerfile
```

### Docker Network

The `docker network` command is used to manage networks in Docker, allowing the creation, deletion, and configuration of networks for containers. This command is essential for establishing communication between containers, facilitating their interaction in an isolated and secure environment. By creating custom networks, you can control how containers connect to each other and to the outside world, improving the organization and security of your containerized applications.

List all configured Docker networks
```
docker network ls
```

Create a network
```
docker network create NW_NAME
```

Delete a network
```
docker network rm NW_NAME
```

The **container name** acts as a unique identifier and can be used as a **domain name** to facilitate communication between containers.

Connect the container to a specific network
```
docker create -p 3306:3306 --name mymysql --network NW_NAME -e MYSQL_ROOT_PASSWORD=p4$$w0rd mysql
```

## Docker Compose

**Docker Compose** is a tool that allows you to define and run applications composed of multiple containers using a YAML configuration file (`docker-compose.yml`). It simplifies the management of multiple services, networks, and volumes with a single command, streamlining the configuration and deployment process.

**Advantages over a Dockerfile**:
1. **Orchestration**: Enables management of multiple containers as a single service, making it easier to configure complex applications.
2. **Ease of Use**: Provides a declarative and more accessible format for defining services and their interactions.
3. **Simplified Configuration**: Facilitates the setup of networks and shared volumes among containers without needing to create multiple Dockerfiles.

Example configuration file:

```yml
version: '3.9'
services:
  web:
    build: .
    ports:
      - "8080:80"
    networks:
      webnw:     # Assigns an IP within the custom network
        ipv4_address: 10.10.10.10  
    restart: always
    depends_on:
      - db
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgres://user:password@db:5432/mydatabase
    volumes:
      - .:/usr/src/app

  db:
    image: postgres:latest
    ports:
      - "5432:5432"
    networks:
      webnw:
        ipv4_address: 10.10.10.11
    restart: always
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=mydatabase
    volumes:
      - db_data:/var/lib/postgresql/data

volumes:
  db_data:            # Declares the volume 'db_data'

networks:
  webnw:
    ipam:         # IP Address Management
      driver: default
      config:
        - subnet: "10.10.10.0/24"
```

### Options for the `docker-compose.yml` file

* **`version:`**: Specifies the version of the Docker Compose syntax used. (Currently can be omitted)

* **`services:`**: Defines the different services that make up the application, each running in a separate container.

* **`web:`**: Name of the main service representing the web application.

* **`build: .`**: Indicates that the container should be built from the Dockerfile in the current directory (`.`).

* **`ports:`**: Maps ports between the container and the host machine. In this case, port 8080 of the host machine connects to port 80 of the container.

* **`restart:`** Sets when a container should restart if it stops.

* **`depends_on:`**: Specifies that the `web` service depends on the `db` service, ensuring that it starts after `db` is available.

* **`environment:`**: Defines environment variables for the container.

* **`volumes:`**: Used to define volumes that persist container data on the host system, preventing loss when stopping or removing the container.

* **`db:`**: Name of the service representing the PostgreSQL database.

* **`image: postgres:latest`**: Indicates that the latest PostgreSQL image from Docker Hub will be used.

* **`networks`**:
```yml
networks:
  webnw:
    ipam:
      driver: default
      config:
        - subnet: "10.10.10.0/24"
```
The `networks` section defines a custom network with a default IP driver and a specific subnet to assign IP addresses to connected containers.

These configurations offer flexibility in assigning and managing IP addresses in container environments.

**Types of drivers**:

1. **bridge**: Creates an isolated network for containers that can communicate with each other but not with the outside world unless ports are exposed. (<font color="blue">**Default**</font>)

2. **host**: Allows a container to use the host's network, removing network isolation.

3. **overlay**: Facilitates communication between containers on different hosts, useful for distributed applications.

4. **macvlan**: Assigns a MAC address to a container, allowing it to appear as a physical network device on the local network.

5. **none**: Disables networking for the container, with no access to any network.

**IPAM configurations in Docker**

The `config` section of IPAM allows defining various settings for managing IP addresses in Docker's custom networks. The following configurations are available:

1. **subnet**: Defines the range of IP addresses to be used in the network.

2. **gateway**: Specifies the IP address of the gateway for the network.

3. **ip_range**: Sets a specific range of IP addresses within the subnet for assignment to containers.

4. **auxiliary_addresses**: Allows reserving additional IP addresses for specific uses within the subnet.

5. **ipv6**: Enables IPv6 address management for the network if needed.

**Types of Volumes**

1. **Anonymous Volumes**: 
Automatically created and deleted with the container. Used for temporary data.
   ```yml
   volumes:
     - /data  # Anonymous
   ```

2. **Named Volumes**:
Persist with a specific name. Retain data regardless of the container.
   ```yml
   volumes:
     - db_data:/var/lib/postgresql/data  # Named
     # mysql -> /var/lib/mysql
     # mongo -> /data/db
   ```

3. **Directory Mounts**:
A directory from the host machine is mounted in the container, reflecting real-time changes.
   ```yml
   volumes:
     - ./app:/usr/src/app  # Mount
   ```

### Docker Compose Commands

Start all the containers defined in `docker-compose.yml`, creating networks and volumes as specified. If the images are not available, it automatically downloads them.
```
docker compose up
# docker compose up -d -> Run in the background
```

Stop and remove the created containers, networks, and volumes.
```
docker compose down
```

Stop running containers without removing them.
```
docker compose stop
```

Show the status of the containers managed by Docker Compose.
```
docker compose ps
```

Show the logs of the running services.
```
docker compose logs
```
