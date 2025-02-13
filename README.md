# Docker Containers & Kubernetes Fundamentals

src = https://www.youtube.com/watch?v=kTp5xUtcalw  

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



@4/356
---
EOF
