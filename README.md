# Docker Containers & Kubernetes Fundamentals

**Resources**:
- https://www.youtube.com/watch?v=kTp5xUtcalw
- https://github.com/K8sAcademy/Fundamentals-HandsOn
- https://www.cncf.io/

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





@29/356 (8%)
---
EOF
