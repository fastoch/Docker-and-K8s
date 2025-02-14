# Docker Containers & Kubernetes Fundamentals

**Resources**:
- https://www.youtube.com/watch?v=kTp5xUtcalw
- https://github.com/K8sAcademy/Fundamentals-HandsOn

---

# Setup

In this course, we will use VS Code with the Docker extension.  

We need to install Docker and Kubernetes (K8s) on Arch Linux. For that, we will:
- update the system,
- install the docker packages,
- start the Docker service and enable it to run at boot,
- check docker version to confirm proper installation,
- install K8s,
- Verify the installation by checking the version of `kubectl` (the Kubernetes command-line tool),
- start and enable the kubelet service
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

# 


@11/356
---
EOF
