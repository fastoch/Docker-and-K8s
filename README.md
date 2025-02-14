# Docker Containers & Kubernetes Fundamentals

**Resources**:
- https://www.youtube.com/watch?v=kTp5xUtcalw
- https://github.com/K8sAcademy/Fundamentals-HandsOn

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

In a microservices architecture, functionalities are exposed through APIs (Application Programming Interfaces), which reduces the bound between services.   

In the context of microservices, **loosely coupled systems** refer to an architectural design where individual services operate independently,  
allowing changes in one service without necessitating changes in others.  

This **independence** is crucial for maintaining system **stability** and enhancing development **agility**.  
When services are loosely coupled, they communicate through well-defined interfaces, typically **APIs**, which abstract their internal implementations  
and facilitate interaction without tight dependencies.  




@16/356
---
EOF
