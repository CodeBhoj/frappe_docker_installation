# Frappe Installation Guide on Ubuntu using Docker

This guide provides step-by-step instructions to install Frappe using Docker on an Ubuntu system.

## 1. Install Docker and Docker Compose

First, update the package list:

```sh
sudo apt update
```

Then, install Docker:

```sh
sudo apt install -y docker.io
```

Install Docker Compose to manage multi-container applications:

```sh
sudo apt install -y docker-compose
```

Check the installed versions:

```sh
docker --version
docker-compose --version
```

## 2. Configure Docker Permissions

If you encounter a permission error while running Docker commands, you need to add your user to the `docker` group.

Check if the `docker` group exists:

```sh
cat /etc/group | grep docker
```

If your username is not listed, add yourself to the `docker` group:

```sh
sudo usermod -aG docker $USER
```

Apply the new group changes:

```sh
newgrp docker
```

Verify that Docker is running:

```sh
docker ps
```

## 3. Clone Frappe Docker Repository

Clone the Frappe Docker repository:

```sh
git clone https://github.com/frappe/frappe_docker.git
```

Navigate into the cloned repository:

```sh
cd frappe_docker
```

## 4. Setup DevContainer Configuration

Copy the example development container configuration:

```sh
cp -R devcontainer-example .devcontainer
```

Navigate into the `.devcontainer` directory:

```sh
cd .devcontainer
```

Start the Docker containers in detached mode:

```sh
docker-compose up -d
```

## 5. Access Frappe Bench Container

Execute an interactive shell inside the Frappe bench container:

```sh
docker exec -it <frappe_container_name> bash
```

To find the container name, use:

```sh
docker ps
```

## 6. Install Frappe Bench

Initialize a new Frappe bench instance:

```sh
bench init --skip-redis-config-generation --frappe-branch version-14 frappe-bench
```

_(Replace `version-14` with the Frappe version you want to install.)_

Navigate into the `frappe-bench` directory:

```sh
cd frappe-bench
```

## 7. Configure Redis and Database

Set the database host and Redis configurations in `common-site-config.json`:

```sh
bench set-config -g db_host mariadb
bench set-config -g redis_cache redis://redis-cache:6379
bench set-config -g redis_queue redis://redis-queue:6379
bench set-config -g redis_socketio redis://redis-socketio:6379
```

## 8. Create a New Site

Create a new site named `codebhoj.localhost`:

```sh
bench new-site codebhoj.localhost --admin-password admin --db-root-password root
```

## 9. Create a New App

Create a new Frappe app named `codebhoj`:

```sh
bench new-app codebhoj
```

## 10. Install the App on the Site

Install the `codebhoj` app on the created site:

```sh
bench --site codebhoj.localhost install-app codebhoj
```

## 11. Start Bench

Run the bench server:

```sh
bench start
```

### **Frappe is now installed and running using Docker on Ubuntu!** 🎉

