# 🐳 Docker containers for Raspberry Pi

This repository contains my personal **containers** managed with Docker on my Raspberry Pi setup. Each stack is defined as a single `docker-compose` file for easy deployment and maintenance.

---

## 🚀 Overview

These stacks include self-hosted services, databases, and utilities optimized for ARM architecture (Raspberry Pi 4/5). Managed via **Portainer** for simplicity and centralized control.

---

# Installing Docker on Raspberry Pi

Follow these steps to install Docker and Docker Compose on your Raspberry Pi.

## 🚀 Installation

### 1. Install Docker
```bash
curl -fsSL https://get.docker.com/ -o get-docker.sh
sudo sh get-docker.sh
```

### 2. Add Your User to the Docker Group
```bash
sudo usermod -aG docker ${USER}
```

### 3. Enable Docker to Start on Boot
```bash
sudo systemctl enable docker
```

### 4. Install Docker Compose
```bash
sudo apt-get update && sudo apt-get install -y docker-compose-plugin docker-buildx-plugin
```

### 5. Reboot the System
```bash
sudo reboot
```

### 6. (Optional) Create a Custom Docker Network with Static IPs
```bash
docker network create -d ipvlan \
  --subnet 192.168.1.0/24 \
  --gateway 192.168.1.1 \
  -o parent=eth0 \
  ipvlan
```

> 💡 *This step is optional and allows you to assign fixed IP addresses to your Docker containers.*

---
✅ After rebooting, Docker should be ready to use on your Raspberry Pi!
