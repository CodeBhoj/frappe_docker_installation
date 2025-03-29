
# Frappe Installation Guide on Ubuntu using Docker

This guide provides step-by-step instructions to install Frappe using Docker on an Ubuntu system.

## Install Docker
First, update the package list:
```sh
sudo apt update
```

Then, install Docker:
```sh
sudo apt install docker.io
```

Install Docker Compose to manage multi-container applications:
```sh
sudo apt install docker-compose
```

## Clone Docker Files
Clone the Frappe Docker repository:
```sh
git clone https://github.com/frappe/frappe_docker.git
```

Navigate into the cloned repository:
```sh
cd frappe_docker
```

## Setup DevContainer Configuration
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

Check running Docker containers:
```sh
docker ps
```

## Access Frappe Bench Container
Execute an interactive shell inside the Frappe bench container:
```sh
docker exec -it frappe_docker bash
```

## Install Frappe Bench
Initialize a new Frappe bench instance:
```sh
bench init --skip-redis-config-generation frappe-bench
```

Navigate into the `frappe-bench` directory:
```sh
cd frappe-bench
```

## Configure Redis and Database
Set the database host and Redis configurations in `common-site-config.json`:
```sh
bench set-config -g db_host mariadb
bench set-config -g redis_cache redis://redis-cache:6379
bench set-config -g redis_queue redis://redis-queue:6379
bench set-config -g redis_socketio redis://redis-queue:6379
```

## Create a New Site
Create a new site named `codebhoj.localhost`:
```sh
bench new-site codebhoj.localhost
```

## Create a New App
Create a new Frappe app named `codebhoj`:
```sh
bench new-app codebhoj
```

## Install the App on the Site
Install the `codebhoj` app on the created site:
```sh
bench --site codebhoj.localhost install-app codebhoj
```

## Start Bench
Run the bench server:
```sh
bench start
```

Frappe is now installed and running using Docker on Ubuntu!


