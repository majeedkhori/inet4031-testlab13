# Docker Lab: Containerizing a Three-Tier Application
**INET 4031 - Introductions to Systems**

This lab introduces Docker and Docker Compose by having you containerize a
real, multi-service application. You will package three components: Apache,
Flask, and MariaDB. These will be packaged into separate containers and wired together so they function as a complete application.

The application code and scaffolding are provided. Your job is to complete the Dockerfiles, verify the stack runs correctly, and document your work below.

> **Directions and explanations for this lab are on the original inet-4031-testlab12 repository Wiki.**
> Refer to the Wiki pages at https://github.com/Programming-Mellow/inet4031-testlab12 for step-by-step instructions.

---

# Project Overview

This project is a three-tie containerized web application:

- Frontend (Apache Web Server): Serves static content and sends requests to Flask.
- Backend (Flask): Handles site logic and API requests.
- MariaDB (Database): Stores ticket data.

When you open the site in a browser, Apache recevies the request, sends it to Flask, and Flask communicates with the datbase as needed.

# Prerequisites

- Install Visual Studio Code
- Install Remote - SSH on VS Code
- Create/configure a Linux Ubuntu VM
- Install Docker and Docker Compose
- Install Git

# Getting Started

To run the project:

```bash
# Clone the repo
git clone https://github.com/Programming-Mellow/inet4031-testlab12.git

# Go into the folder
cd inet4031-testlab12

# Start up with Docker Compose
sudo docker compose up --build -d

# Confirm containers are running/healthy
sudo docker compose ps
```
# Configuration

This project uses a .env file for database configuration. The .env file contains database name, database user, database password, and root password for MariaDB. These values allow Flask to connect to the database.

Add .env to a .gitignore file. Do NOT upload your .env file to GitHub as it contains sensitive info.

# Verification

To confirm everything is working properly:

Run the test script:

```bash
sudo ./check-lab.sh
```

# Kubernetes Lab: Running the Application with k3s/Desired State (Lab 13)

In this lab, the application was migrated from Docker Compose to Kubernetes using k3s. Instead of manually managing containers, Kubernetes uses a desired state model where it continuously ensures that the correct number of Pods, Services, and storage resources are running.

The same three-tier application was reused from Lab 12, but each component was converted into Kubernetes resources:  

Kubernetes automatically handles restarting failed containers, networking between services, and maintaining application state.

---

# Deployment Instructions

To deploy the application to Kubernetes:

```bash
kubectl apply -f k8s/
```

To check that everything is running:

```bash
kubectl get pods -n ticket-app
kubectl get services -n ticket-app
kubectl get pvc -n ticket-app
```
To watch pods start up in real time:

```bash
kubectl get pods -n ticket-app -w
```
To access the dashboard:

Naviagate to your browser and enter "http://<VM-IP>:30080", replacing <VM-IP> with YOUR VM's IP address.

# What Changed

- Docker Compose runs containers manually on a single machine.
- Kubernetes continuously enforces a desired state instead of running one-time commands.
- If a container or Pod fails, Kubernetes automatically recreates it.
