### Popeye BS 🐋

# Step 0

Before starting make sure to stop and rm every containers after each steps.

## What is Docker?

Docker is an open platform launched in 2013 for developing, shipping, and running applications.
Docker goal's is to shorten time between writting the code and testing it (running it in production).

Docker is able to run an app in an "isolated container", those container being hosted, you can run as many as you want simultaneously. This is what you can do with Docker in a Nutshell :

* Develop your applications and its supporting components using containers.
* The container becomes the unit for testing and distributing your application.
* When you are ready, you can deploy your application as a container or oan orchestrated service in your environment (that can be a data center or even a cloud service).

## What is the difference between Virtual Machines and Docker containers?

* Docker container :

A docker container *image* is a lightweight standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings. Those containers images become containers at runtime and in the case of Docker containers, images become containers when run on Docker Engine. Avaiable on Windows and Linux, containerized software will always run the same on both OS regardless of their infrastructure.

* Virtual Machine :

A virtual Machine is a digital version of a computer (different OS). You can have an arch digital computer installed on your windows PC, and you will eventually have two different OS on one computer. An app tested on both OS would not be the same : one OS or the other would have not all tools to make it run for example. That's why with Docker, the expression 'It works on my machine !' does not exist.

## What is the difference between a Docker container and a Docker image?

A docker container is a self-contained, runnable software application or service. On the other hand, a Docker image is the template loaded onto the container to run it, like a set of instructions. You store images for sharing and reuse, but you create and destroy containers over an application's lifecycle.

## How much are two Docker containers separated when running on the same machine?

First, as we previously said, it is possible to run two or more instances of a Dockerfile at the same time. When two Docker containers are running on the same machine, they are isolated from each other at several levels:

* Process isolation: Each Docker container runs as an isolated process on the host operating system. This means that the processes within one container cannot directly interact with or affect the processes in another container.

* Filesystem isolation: Docker containers have their own filesystem, which is isolated from the host filesystem and from other containers' filesystems. Each container has its own root filesystem, allowing different versions of libraries, dependencies, and applications to coexist without conflict.

* Network isolation: By default, Docker containers run in their own network namespace, which means they have their own network interfaces and IP addresses. This allows containers to communicate with each other or with the outside world without interfering with each other's network configuration.

* Resource isolation: Docker allows you to specify resource constraints for containers, such as CPU, memory, and disk space limits. This ensures that one container cannot consume all available resources on the host, which helps maintain system stability and prevents one container from affecting the performance of others.

* Namespace isolation: Docker containers use Linux namespaces to provide isolation for various system resources, including processes, filesystems, network interfaces, and more. Namespaces ensure that each container has its own view of the system, separate from other containers.

Overall, while Docker containers running on the same machine share the underlying kernel and hardware resources, they are effectively isolated from each other to provide a secure and predictable runtime environment.

## In what way will spinach (Docker) helps you to master DevOps?

Docker, a leading containerization platform, has revolutionized the way applications are built, shipped, and deployed. As a DevOps engineer, mastering Docker and understanding best practices for Dockerfile creation is essential for efficient and scalable containerized workflows.

Docker helps you keep your local development environment clean. Instead of having multiple versions of different services installed such as Java, Kafka, Spark, Cassandra, etc., you can just start and stop a required container when necessary.

## Find the official Nginx image and import (pull) it on your machine.

Command: ```docker pull nginx```

Result:

Using default tag: latest
latest: Pulling from library/nginx
e1caac4eb9d2: Pull complete 
88f6f236f401: Pull complete 
c3ea3344e711: Pull complete 
cc1bb4345a3a: Pull complete 
da8fa4352481: Pull complete 
c7f80e9cdab2: Pull complete 
18a869624cb6: Pull complete 
Digest: sha256:c26ae7472d624ba1fafd296e73cecc4f93f853088e6a9c13c0d52f6ca5865107
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest

## List the images you have on your machine.

Command: ```docker images```

Result:

REPOSITORY                             TAG       IMAGE ID       CREATED       SIZE
nginx                                  latest    e4720093a3c1   4 days ago    187MB
hello-docker                           latest    d074e7184fd3   3 weeks ago   141MB
f143mg/hello-docker                    latest    d074e7184fd3   3 weeks ago   141MB
node                                   alpine    29d74145c0e8   4 weeks ago   141MB
ghcr.io/epitech/coding-style-checker   latest    824b8d3e3ee1   5 weeks ago   899MB

## Run an Nginx container.

Command: ```docker run --name my-nginx-container -p 80:80 -d nginx```

Result: ```2485420a9e8f4d09e0ae2977ba08ee85db307244f54497b60b73bb3c1247a760```

## In another terminal window, while your container is running, list the containers running on your machine. Do you see the container ID of your Nginx container?

Command: ```docker ps```

Result:

CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                               NAMES
2485420a9e8f   nginx     "/docker-entrypoint.…"   4 minutes ago   Up 4 minutes   0.0.0.0:80->80/tcp, :::80->80/tcp   my-nginx-contain

## Now, stop your container.

Command: ```docker stop 2485420a9e8f``` (ID)

Result: ```2485420a9e8f```

## Even if your container is stopped, it still exists on your system. Remove it.

Command: ```docker rm 2485420a9e8f``` (ID)

Result: ```2485420a9e8f```

## There exists a nice option to automatically remove a container when it is stopped:

We can use the --rm option with the docker run command to automatically remove a container when it's stopped. This option ensures that the container is removed as soon as it stops, either by itself or by being manually stopped by the user.

Command: ```docker run --rm --name my-nginx-container -p 80:80 -d nginx```

By including the --rm option, Docker will automatically remove the container named my-nginx-container once it stops running. We don't need to explicitly run docker rm to remove it.

## Bad news! The developper told you that their application is only compatible with Nginx 1.18. Anyway, run a container with this specific version.

Command: ```docker run --name my_nginx_container -d -p 80:80 nginx:1.18```

Result:

Unable to find image 'nginx:1.18' locally
1.18: Pulling from library/nginx
f7ec5a41d630: Pull complete 
0b20d28b5eb3: Pull complete 
1576642c9776: Pull complete 
c12a848bad84: Pull complete 
03f221d9cf00: Pull complete 
Digest: sha256:e90ac5331fe095cea01b121a3627174b2e33e06e83720e9a934c7b8ccc9c55a0
Status: Downloaded newer image for nginx:1.18
59f7e0fe11f449d1467d79706852d868c1a946838818dffa67bdc9cb29dffc5b

## Nginx is a web server, which you can access from your usual browser. However, it is not possible yet to access it from your host, as its port is not published. How to fix that?

When we run the Nginx container using the docker run command, we can use the -p option to specify port mapping. The syntax is -p host_port:container_port.

Command: ```docker run --name my-nginx-container -p 8080:80 -d nginx```

Result: ```08d2fd4d4ac13947d5190e920b3d4050bd093beb1e7a8308770aa646d55119a1```

## Nginx listens on its port 80. After publishing the port, you should be able to access the Nginx welcome page at http://localhost:xxx, where xxx is the host port you chose to publish the container’s port 80 to.

The port we chose is ```8080```

Web adress: ```http://localhost:8080```

![Alt text](/assets/localhost.png)

## Can you find out which day it is in the container? Execute the command date into a running Nginx container.

Command: ```docker exec my-nginx-container date```

Result: ```Mon Feb 19 12:20:16 UTC 2024```

## Because you absolutely love reading lines of information written on your screen, find how to display the logs of your container. (Bonus point if you find how to display them in real time.)

Command: ```docker logs my-nginx-container```

Result:

/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2024/02/19 12:11:35 [notice] 1#1: using the "epoll" event method
2024/02/19 12:11:35 [notice] 1#1: nginx/1.25.4
2024/02/19 12:11:35 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14) 
2024/02/19 12:11:35 [notice] 1#1: OS: Linux 6.7.4-100.fc38.x86_64
2024/02/19 12:11:35 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1024:1024
2024/02/19 12:11:35 [notice] 1#1: start worker processes
2024/02/19 12:11:35 [notice] 1#1: start worker process 29
2024/02/19 12:11:35 [notice] 1#1: start worker process 30
2024/02/19 12:11:35 [notice] 1#1: start worker process 31
2024/02/19 12:11:35 [notice] 1#1: start worker process 32
2024/02/19 12:11:35 [notice] 1#1: start worker process 33
2024/02/19 12:11:35 [notice] 1#1: start worker process 34
2024/02/19 12:11:35 [notice] 1#1: start worker process 35
2024/02/19 12:11:35 [notice] 1#1: start worker process 36
2024/02/19 12:11:35 [notice] 1#1: start worker process 37
2024/02/19 12:11:35 [notice] 1#1: start worker process 38
2024/02/19 12:11:35 [notice] 1#1: start worker process 39
2024/02/19 12:11:35 [notice] 1#1: start worker process 40
172.17.0.1 - - [19/Feb/2024:12:14:13 +0000] "GET / HTTP/1.1" 200 615 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:122.0) Gecko/20100101 Firefox/122.0" "-"
2024/02/19 12:14:13 [error] 29#29: *1 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file or directory), client: 172.17.0.1, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "localhost:8080", referrer: "http://localhost:8080/"
172.17.0.1 - - [19/Feb/2024:12:14:13 +0000] "GET /favicon.ico HTTP/1.1" 404 153 "http://localhost:8080/" "Mozilla/5.0 (X11; Linux x86_64; rv:122.0) Gecko/20100101 Firefox/122.0" "-"

#### Bonus

We just have to add the flag -f or --follow before our container.

Command: ```docker logs -follow my-nginx-container``` or ```docker logs --follow my-nginx-container```

# Step 1

Write a file named Dockerfile at the root of your repository which respects the following specifications:

* Based on the official debian:bookworm-slim image

* Installs Node.js and its associated package management tool with apt-get.

* Copies the source code of the application into the image.

* Installs the application’s dependencies with Node.js’ package management tool.

* Exposes port 3000.

* Sets the command (not the entrypoint, beware) to start the application so that the application is run when running a container based on the image.

![Alt text](/assets/dockerfile.png)

We adapt the conditions to our Fedora OS.

![Alt text](/assets/jigglypuff.png)

# Step 2

We write the ```docker-compose.yml``` file according to the steps:

![Alt text](/assets/yml_file.png)

And we then compose the command ```docker-compose up```

![Alt text](/assets/shiny.png)

You've now finished your introduction to Docker !
 