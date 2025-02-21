# Docker Containers & Kubernetes Fundamentals

**Resources**:
- The course: https://www.youtube.com/watch?v=kTp5xUtcalw
- The repository: https://github.com/K8sAcademy/Fundamentals-HandsOn
- Cloud Native Computing Foundation: https://www.cncf.io/

---

# Setup

In this course, we will use VS Code with the Docker extension.  

We also need to install Docker and Kubernetes (K8s). On Arch Linux, you need to:
- update the system,
- install the docker packages,
- start the Docker service and enable it to run at boot,
- check docker version to confirm proper installation,
- install K8s,
- Verify the installation by checking the version of `kubectl` (the Kubernetes command-line tool),
- start and enable the kubelet service

All of the above steps correspond to the following commands:
```bash
sudo pacman -Syu
sudo pacman -S docker docker-compose docker-buildx
sudo systemctl start docker
sudo systemctl enable docker
docker --version
sudo pacman -S kubernetes kubernetes-server
kubectl version
sudo systemctl start kubelet
sudo systemctl enable kubelet
```

After that, there will be a few additional steps to make sure K8s operates correctly, like:
- configuring several files
- start K8s services
- ensure all services are running

Next, you should start VS Code and clone the following repo: https://github.com/K8sAcademy/Fundamentals-HandsOn  
You can also install Git, configure it with your email and username, and run `git clone <repository-url> <target-directory>`  

---

# Microservices Architecture vs Monolithic Architecture

## Monolithic Architecture

- the whole application is built and deployed as **one big single unit**
- not convenient for scaling the application, because we need to set new servers up, then duplicate the entire project on each server

## Microservices 

We break our big traditional monolithic application into small functional units.  
We segragate functionality into smaller separate services, each with a single responsibility.  
Which allows to scale the application more easily, by scaling each of the services independently from the others.  

This model also enables autonomous development by different teams, using different languages and platorms.  

---

# Microservices Anti Patterns

- too much small units = risk of unnecessary complexity
- securing all microservices can turn out to be complex too
- implementing DevOps practices demands time and resources

---

# Microservices - benefits & drawbacks

## Benefits

- If one service fails, it does not bring down the entire system, making it more resilient to errors
- microservices run on open-source technologies, so there's less vendor lock-in
- smaller pieces of software are easier to understand in most cases
- smaller also means faster to deploy
- microservices are easier to scale, rather than having to scale the whole application each time 

## Drawbacks

- added complexity: is your team properly trained for using Docker & K8s?
- one microservice update can impact many microservices
- multiple databases management can be tricky
- **calls** between microservices will go through **APIs**, which can induce latency issues
- potential transient errors (temporary disruptions) require the use of a **service mesh**
  - A service mesh is an infrastructure layer that manages communication between microservices in a distributed application
- can you system survive if one microservice goes down?
- what about security? can your microservices communicate in a secure manner?

---

# Cloud Native

Within a short time, **cloud native** has become a driving trend in the software industry.
- it's a new way to think about building complex systems (or complex applications)
- it takes full advantage of modern software development practices, technologies, and cloud infrastructure
- it's widely popular in the open-source community

## Loosely Coupled Systems in Microservices

In a microservices architecture, functionalities are exposed through APIs (Application Programming Interfaces),  
which means systems (or sub-systems) are loosely coupled in comparison to the strong bound they share in a monolithic architecture.   

In the context of microservices, **loosely coupled systems** refer to an architectural design where individual services operate independently,  
allowing changes in one service without necessitating changes in others.  

This **independence** is crucial for maintaining system **stability** and enhancing development **agility**.  
When services are loosely coupled, they communicate through well-defined interfaces, typically **APIs**, which abstract their internal implementations  
and facilitate interaction without tight dependencies.  

## Speed & Agility

The end users want:
- instantaneous response 
- most up-to-date features
- no downtime

Businesses want:
- accelerated innovation
- rapid release of features to meet disruption from competitors
- increased confidence due to improved stability/performance

## Cloud Native Architecture characteristics

- infrastructure becomes immutable and disposable, meaning we never perform updates, we just put the infrastructure down and we replace it with a new one
- the provisioning of a new infrastructure happens in minutes, and the existing ones can be destroyed as fast, this process can even be automated

## Roadmap to Cloud Native

- Your team must first learn how to containerize your applications
- then you need to automate deployments through the use of CI/CD (continuous integration/continuous delivery) tools
  - so that changes to your source code automatically result in a new container being built, tested, and deployed to staging, and eventually to production
- you need to use an orchestrator (Kubernetes, also referred to as "K8s", is the market-leading orchestration solution)
- observability: you need to pick solutions for monitoring, logging and tracing, so you can understand what's happening in your **K8s clusters**
- use **services meshes** to provide more functionalities inside your cluster

---

# Containers Concepts

- A container is a unit of software/deployment. It contains everything needed for your application to run
- containers are typically lightweight, which makes them easier and faster to deploy than a complete monolithic application
- containers use fewer resources, therefore you can deploy many of them on the same server
- when using CI/CD techniques (automation), containers are even faster to deploy
- you can run containers **anywhere**, you don't need to worry about compatibility since containers are self-sufficient (autonomous)
- containers are isolated from each other and from the host system, which is better for security and for limiting the impact in case of failure
  - if one container fails, it will not take the whole system down with it

## VM vs Container

Containers and Virtual Machines (VMs) are both virtualization methods that isolate computing environments,  
but they differ in their approach, scale, and portability.  

**VMs virtualize the hardware, while containers virtualize the operating system.**  

Unlike VMs, containers don't have to boot, because they use the host kernel, which means they can start much faster.  
Containers also use much less memory and storage space, since there's no OS.

### VMs

**VMs virtualize physical machines**, allowing efficient use of hardware resources.  
A **hypervisor**, a virtualization software, is installed on a physical server (host computer) to emulate multiple guest operating systems.  
**Each VM runs a complete operating system with its own kernel**.  
The hypervisor coordinates resource sharing, enabling VMs to run in isolation on the same hardware.

### Containers

**Containers package software with all its dependencies**, allowing it to run consistently across different environments.  
They virtualize the operating system, **allowing applications to run independently on any platform**.  
Instead of a hypervisor, containers use a **container engine** (like Docker) that acts as an intermediary between the containers and the operating system.  
Because **containers share the host OS kernel**, they are more lightweight than VMs.  

## Summary

VMs have a larger footprint and are slow to boot, but despite the rise of containers, they still have important use cases.  
VMs and containers can be seen as complementary technologies, as containers can even run within VMs...  

Containers are lightweight, can start in seconds, offer portability and scalability, which makes them a better choice for Developers and DevOps teams in general.  

## Containers are made of layers

- base OS
- customizations
- your application

The `docker pull` command retrieves and downloads a container image.  
Each layer of this image is downloaded individually.  
Docker uses a local cache to store the containers' images, and if a layer already exists on the host system, it will not be downloaded again.  

One of the goal when creating new container images is to create them with the smallest number of layers possible.  
Later on, we'll see techniques on how to achieve that.  

**Can we write on these layers?**
No, except for the top one. The lower layers are read-only.  

## Container registry

A container registry is a centralized container repository where you deploy the container images you create.  
Think GitHub but for containers.  

The most popular one is certainly the **Docker Hub** - https://hub.docker.com  
It provides public and private repositories.  

All major Cloud providers such as AWS, GCP, or Azure have container registry services.  

## Orchestrator

An orchestrator allows us to manage, scale, and monitor the containers that we run on our servers.  
You can install your own orchestrator, or you can use a managed cluster offered by a Cloud provider.  

Before we can talk about K8s and other orchestrators, we need a better knowledge of containers...

---

# What is Docker?

initial release was in 2013

Docker is the company behind the eponymous containerization platform.  
Docker maintains the "moby" project, which is an open-source container runtime.  

The Docker application provides: 
- a container runtime that runs on Mac, Windows and Linux
- a command-line tool to create and manage containers
- a 'dockerfile' format for building container images

---

# installing Docker

## Windows Subsystem for Linux

if you're running Windows 10 or 11, enable WSL2, so you can run a Linux system within Windows.  
You can easily find detailed tutorials on the Web.

  - once WSL2 has been enabled, open Windows Terminal, and run `wsl -l --online` to see all available distros
  - install your distro of choice, for example: `wsl --install Ubuntu`
  - `wsl -l -v` to see your newly installed distro and the wsl version it uses
  - `wsl -d Ubuntu` to start your Linux distro
  - `sudo apt update` to refresh the list of available packages and their versions (from the configured repositories)
  - `sudo apt upgrade` to update your Linux system

## install Docker on your WSL machine

We are now ready to install Docker on your WSL Ubuntu machine.  

First, let's install a few prerequisite packages which let apt use packages over HTTPS:  
`sudo apt install apt-transport-https ca-certificates curl software-properties-common`

After that, run the following commands:
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o docker.gpg
sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
```

**Explanation**:
- the first line downloads Docker's GPG public key from the specified URL and saves it to a file named docker.gpg
- the second line converts the downloaded 'docker.gpg' file from its ASCII armored format to a binary format and saves it as 'docker-archive-keyring.gpg' in the '/usr/share/keyrings/' directory.
  - `gpg --dearmor` converts the ASCII armored key file to a binary format suitable for use by APT (Advance Package Tool)
  - The `-o` option is for specifying the output file location
  - `/usr/share/keyrings/`is the standard location for storing APT repository keys
- the `echo` line is a bit more complex:
  - adds a new entry to your APT sources list, pointing to the official Docker repository.
  - The entry includes your system's architecture, the Ubuntu codename, the repository URL, and the path to the GPG key used to verify the packages.
  - The `tee` command is used to write the entry to the docker.list file, and any output to the terminal is suppressed.
- the last line updates the APT package lists, making APT aware of the packages available in the newly added Docker repository.

These 4 commands work together to securely add the official Docker repository to your Ubuntu system.  
This allows you to then use `apt install docker-ce` (or similar) to install Docker Engine and related components from the official source, ensuring you get verified and up-to-date packages.

Finally, install Docker:  
`sudo apt install docker-ce`  

Docker should now be installed, the daemon started, and the process enabled to start on boot.  
Check that it’s running: `sudo systemctl status docker`  
The output should be showing that the service is active and running.  

---

# Docker CLI

## Run docker commands without sudo

- add your user to the docker group: `sudo usermod -aG docker $USER`
  - `-a` ensures the user is added to the specified group without being removed from other groups they already belong to
- Log out and log back in for the group changes to take effect

## First useful commands

```bash
docker version
docker info
docker login -u fastoch
```

**Note**:  
When running `docker info`, you will get some warnings.  
These warnings are not errors and can typically be ignored for most Docker users.   
They indicate that your system's kernel does not support certain block I/O (blkio) throttling features for Docker containers.  
The "blkio" subsystem is used in larger Docker production environments to control I/O limits on block devices.   
For standard Docker usage, especially for beginners or non-production environments, these warnings do not impact functionality.

## Other important commands

```bash
docker pull [imageName]
docker run [imageName]
docker run -d [imageName]
docker image inspect [imageName]
```
- The first lets you pull an image from a registry
  - The image is downloaded from Docker Hub unless another registry is explicitly specified
  - If no tag is specified, Docker uses the `:latest` tag by default
  - You can also specify a particular version or tag: `docker pull ubuntu:24.04`
- The second command lets you run a container built from the specified image
- The third command does the same but runs the container in **detached** mode, so you can keep using your terminal
- The last one can be very useful for debugging purposes

## Yet more docker commands

```bash
docker ps
docker ps -a
docker start [containerName]
docker stop [containerName]
docker rm [containerName]
```
- The first cmd lists running containers
- The seconde one lists running and stopped containers
- The third one starts the specified container
- The fourth one stops the specified running container
- the last one removes the specified containers from the Docker host (it doesn't remove the corresponding images)

**Note**:  
- You need to stop a container before you can remove it
- You can also remove all containers with `docker rm $(docker ps -a -q)`

## imageName vs containerName

- imageName = name of the image you've pulled from the container registry
- containerName = name of the running container (built from that image)

## Practical example

Let's run an Nginx container (a very popular Web server)
```bash
docker run --publish 80:80 --name webserver nginx
```
- we specify the image name as found in the Docker registry (imageName = nginx)
- we specify a name for the running instance (containerName = webserver)
- we use the `--publish` flag to map the host port to the container listening port

## The `-it` options

We can run a Docker container in **interactive** mode with a terminal attached.  
For example, we can start an Ubuntu container and give it a bash shell to interact with:
```bash
docker run -it ubuntu /bin/bash
```
`-i`: Keeps STDIN open even if not attached, allowing interactive input  
`-t`: Allocates a pseudo-TTY, providing a terminal interface

The `-it` options are commonly used for development, testing, and troubleshooting purposes, as they provide immediate feedback and allow for real-time interaction with the containerized environment.

## Attach a shell to a running container

```bash
docker container exec -it [containerName] bash
```

## List docker images & delete a Docker image

```bash
docker images
docker rmi [imageName]
```

The `rmi` command will delete all the layers of the specified image.

## Remove all images not in use by any containers

```bash
docker system prune -a
```

## Run a Web server inside a Docker container

```bash
docker run -d -p 8080:80 --name webserver nginx
```

When you run this command, Docker will:
- Pull the Nginx image from the Docker Hub if it's not already present on your system.
- Create a new container based on this image.
- Start the container in detached mode, meaning it runs in the background.
- Set up port forwarding from the host's port 8080 to the container's port 80.
- Name the container "webserver".
- The Nginx web server inside the container will then be accessible from your host machine at http://localhost:8080

## Restart an existing container and attach a shell to it

After that, if we have stopped the container, we can start it back up via `docker start [containerName]`, the container name being 'webserver' in our example.  

And then, we can attach a bash shell to it via `docker container exec -it webserver bash`.  

When running the last command, the prompt will change, going from `[userName]@[localhost]` to `root@[containerID]`.  

Attaching a shell to a container is super **useful** for troubleshooting purposes.  

You need to run `exit` to exit the container and go back to your local host.  

---

## Docker CLI - Building containers

To build an image using a dockerfile located in the same folder:  
```bash
docker build -t [name:tag] .
```

To build an image using a dockerfile located in a different folder: 
```bash
docker build -t [name:tag] -f [fileLocation]
```

To tag an existing image:
```bash
docker tag [imageName] [name:tag]
```

Taggings have 2 parts: a name and a tag. The tag is usually used to specify the version number.  

---

# What is a dockerfile?

It's a text file listing the steps to build an image. Here's a very basic example:
```dockerfile
FROM nginx:alpine
COPY . /usr/share/nginx/html
```

- The FROM command specifies the **base image**. When building a docker image, you always start from something already existing.
  - In this case, we start with an image of the nginx web server running inside Alpine, a lightweight and security-focused Linux distribution
- The COPY command copies everything from the current folder to a folder inside the container

## Building the image

```bash
docker build -t webserver-image:v1 .
```
We're running the command from the folder where the dockerfile is, hence the dot to specify the file location.  

## Running the container

```bash
docker run -d -p 8080:80 webserver-image:v1
```

## A more complex dockerfile

The following dockerfile is used to create an image from which the container will run a Node.js application:

```dockerfile
FROM alpine
RUN apk add -update nodejs nodejs-npm
COPY . /src
WORKDIR /src
RUN npm install
EXPOSE 8080
ENRTYPOINT ["node", "./app.js"]
```

- First line specifies the base image = Alpine Linux
- Line 2 = install Node and npm using the Alpine Linux package manager
- line 3 = copy the current folder contents (local host) to /src folder (container)
- line 4 = define the working directory as /src, equivalent to run `cd /src`
- line 5 = use npm (node package manager) to install dependencies required for our Node.js app
- line 6 = tells the container to listen on port 8080
- line 7 = when the container starts, it will run the Node.js runtime, then the app.js file in the current directory will be executed
  - This setup is common for Node.js applications, where app.js often serves as the main entry point for the application

---

# Tagging

Using the `docker tag` command, we name an already existing image using a name and (optionnally) a tag.  
If we don't specify a repository, it will default to Docker Hub.  

Later on, we'll see how to push custom images to different repositories.  

---

# VS Code

## What is VS Code?

Let's see how Visual Studio Code can help us build images and run containers.  
VS Code is a text/code editor, it's free and open-source software (FOSS), it runs on Windows, Mac and Linux.  

## VS Code + WSL2

To use Visual Studio Code with the Windows Subsystem for Linux: 
- install Visual Studio Code in Windows
- Install the Remote Development extension pack in VS Code
  - This includes the WSL extension, which enables seamless integration between VS Code and WSL
- Click the green Remote Window button in the bottom-left corner of VS Code.
- Choose "Remote-WSL: New Window" from the pop-up menu.
- A new VS Code instance will open, showing "WSL: Ubuntu" (or your Linux distro) in the status bar

This setup allows you to:
- Develop in a Linux-based environment while using Windows.
- Use Linux-specific tools and utilities.
- Run and debug Linux-based applications from VS Code.
- Take advantage of VS Code features like IntelliSense, debugging, and Git integration.

By combining WSL2 and VS Code, you can enjoy a seamless development experience that leverages the strengths of both Windows and Linux.

## Open your project folder

- Go to File -> Open Folder and navigate to your project directory within the WSL 2 filesystem (usually found in /home/yourusername/)

## Use Docker inside VS Code

Once you have installed VS Code, press **Ctrl + Shift + X** and install the Docker extension.  
Then, press **Ctrl + Shift + P** to open the **Command Palette** and type 'docker add' > select 'add docker files to workspace'.  





@56/356 (15%)
---
EOF
