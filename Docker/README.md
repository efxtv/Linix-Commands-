## 🐳 Docker Basics Course (Command-Based)

A beginner-friendly guide to Docker commands for managing containers, images, CPU/RAM usage, and cleanup.

---

## 📌 1. What is Docker?

Docker is a tool that lets you run applications in isolated environments called **containers**.

---

## ⚙️ 2. Check Docker Installation

```bash
docker --version
docker info
````

---

## 📦 3. Working with Images

### 🔽 Pull an Image

Download an image from Docker Hub:

```bash
docker pull nginx
docker pull ubuntu:22.04
```

### 📜 List Images

```bash
docker images
```

### ❌ Remove Image

```bash
docker rmi nginx
```

Force remove:

```bash
docker rmi -f nginx
```

---

## 🚀 4. Running Containers

### ▶️ Run Container

```bash
docker run nginx
```

Run in background:

```bash
docker run -d nginx
```

Run with name:

```bash
docker run -d --name my-nginx nginx
```

Run with port mapping:

```bash
docker run -d -p 8080:80 nginx
```

---

## 📋 5. Managing Containers

### 📜 List Running Containers

```bash
docker ps
```

All containers:

```bash
docker ps -a
```

### ⏹️ Stop Container

```bash
docker stop my-nginx
```

### ▶️ Start Container

```bash
docker start my-nginx
```

### 🔄 Restart

```bash
docker restart my-nginx
```

### ❌ Remove Container

```bash
docker rm my-nginx
```

Force remove running container:

```bash
docker rm -f my-nginx
```

---

## 🧠 6. CPU & RAM Control

### 🔧 Limit CPU

```bash
docker run -d --cpus="2.0" nginx
```

Update CPU:

```bash
docker update --cpus 2 my-nginx
```

### 🧠 Limit Memory (RAM)

```bash
docker run -d --memory="512m" nginx
```

Update memory:

```bash
docker update --memory 1g my-nginx
```

### 💡 Combined Example

```bash
docker run -d \
  --cpus="2" \
  --memory="1g" \
  --name limited-container \
  nginx
```

---

## 📊 7. Monitor Usage

### 📈 Live Stats (CPU, RAM)

```bash
docker stats
```

---

## 📂 8. Logs & Terminal Access

### 📜 View Logs

```bash
docker logs my-nginx
```

### 🖥️ Access Container Terminal

```bash
docker exec -it my-nginx bash
```

---

## 🧹 9. Cleanup (Very Important)

### Remove Stopped Containers

```bash
docker container prune
```

### Remove Unused Images

```bash
docker image prune
```

### Remove Everything (⚠️ Careful)

```bash
docker system prune -a
```

---

## 💾 10. Volumes (Basic)

### Create Volume

```bash
docker volume create mydata
```

### Use Volume

```bash
docker run -d \
  -v mydata:/data \
  nginx
```

---

## 🌐 11. Network (Basic)

### List Networks

```bash
docker network ls
```

### Create Network

```bash
docker network create mynet
```

---

## 🔁 12. Updating Containers

### Change CPU/RAM

```bash
docker update \
  --cpus 4 \
  --memory 2g \
  my-nginx
```

---

## 🧪 13. Useful Quick Commands

| Task                  | Command                          |
| --------------------- | -------------------------------- |
| Stop all containers   | `docker stop $(docker ps -q)`    |
| Remove all containers | `docker rm $(docker ps -aq)`     |
| Remove all images     | `docker rmi $(docker images -q)` |

---

## Basics of DOCKER **Part 2**👇

---


## 🏷️ 1. Rename a Container

Change container name without recreating it:

```bash
docker rename old-name new-name
````

Example:

```bash
docker rename my-app my-nginx
```

---

## 🔍 2. Inspect Container (Deep Details)

Get full JSON details (IP, mounts, configs):

```bash
docker inspect my-nginx
```

---

## 🧾 3. View Running Processes Inside Container

```bash
docker top my-nginx
```

---

## 📂 4. Copy Files Between Host & Container

### Host → Container

```bash
docker cp file.txt my-nginx:/tmp/
```

### Container → Host

```bash
docker cp my-nginx:/tmp/file.txt .
```

---

## 🧱 5. Create Image from Container (Commit)

Save current container state as image:

```bash
docker commit my-nginx my-image:v1
```

---

## 💾 6. Backup & Restore Images

### Save image to file

```bash
docker save -o myimage.tar my-image:v1
```

### Load image from file

```bash
docker load -i myimage.tar
```

---

## 🔄 7. Restart Policy (Auto Start Containers)

```bash
docker run -d \
  --name auto-app \
  --restart unless-stopped \
  nginx
```

---

## 🌐 8. Bind Container to Specific IP

```bash
docker run -d -p 127.0.0.1:8080:80 nginx
```

---

## 🧹 9. Clean Unused Volumes

```bash
docker volume prune
```

---

## 🧹 10. Clean Unused Networks

```bash
docker network prune
```

---

## 🏷️ 11. Tag (Rename) an Image

```bash
docker tag nginx my-nginx:v1
```

---


## 🐳 Docker Basics Course – Part 3 (Build, Share & Control Basics)

---

## 🧱 1. Build Image from Dockerfile

Create your own image.

### Example Dockerfile

```Dockerfile
FROM nginx
COPY . /usr/share/nginx/html
````

### Build Image

```bash
docker build -t my-custom-nginx .
```

---

## 📁 2. Use .dockerignore

Exclude files from build.

### Example

```
node_modules
.git
*.log
```

---

## 🌍 3. Environment Variables

Pass variables into container:

```bash
docker run -d \
  -e MY_NAME=tadd \
  -e ENV=production \
  nginx
```

---

## 📄 4. View Container Changes

See file changes made inside container:

```bash
docker diff my-nginx
```

---

## 📤 5. Export Container (Backup as File)

```bash
docker export my-nginx > container.tar
```

---

## 📥 6. Import Container as Image

```bash
docker import container.tar my-imported:v1
```

---

## 🔐 7. Login to Docker Hub

```bash
docker login
```

---

## 📤 8. Push Image to Docker Hub

```bash
docker push username/my-nginx:v1
```

---

## 📥 9. Pull Private Images

```bash
docker pull username/my-nginx:v1
```

---

## 🧪 10. History of Image Layers

```bash
docker history nginx
```

---

## ⚙️ 11. Change Container User

Run container with specific user:

```bash
docker run -d --user 1000 nginx
```

---

## 📛 12. Set Hostname

```bash
docker run -d --hostname my-server nginx
```

---

## 🔗 13. Link Containers (Basic Method)

```bash
docker run -d --name db nginx
docker run -d --link db:web nginx
```

---

## 🧠 14. Limit Process Count

Prevent abuse:

```bash
docker run -d --pids-limit 100 nginx
```

---

## 🐳 Docker Basics Course – Part 4 (Docker Compose Basics)

---

## 📦 1. What is Docker Compose?

Docker Compose lets you define and run multiple containers using a single YAML file.

---

## 📁 2. Create docker-compose.yml

Example file:

```yaml
version: "3.9"

services:
  web:
    image: nginx
    ports:
      - "8080:80"

  app:
    image: ubuntu
    command: sleep 3600
````

---

## ▶️ 3. Start Services

```bash id="c1x8dj"
docker compose up
```

Run in background:

```bash id="0z3r6y"
docker compose up -d
```

---

## ⏹️ 4. Stop Services

```bash id="jv2r8m"
docker compose down
```

---

## 🔄 5. Restart Services

```bash id="6d2x1c"
docker compose restart
```

---

## 📜 6. List Services

```bash id="3v7nqp"
docker compose ps
```

---

## 📄 7. View Logs (All Services)

```bash id="r4k8yt"
docker compose logs
```

Follow logs:

```bash id="5h2mns"
docker compose logs -f
```

---

## 🔧 8. Rebuild Services

```bash id="9k1wza"
docker compose up --build
```

---

## 🧱 9. Build Inside Compose

Example:

```yaml
services:
  web:
    build: .
    ports:
      - "8080:80"
```

---

## 🌍 10. Environment Variables in Compose

```yaml
services:
  app:
    image: ubuntu
    environment:
      - ENV=production
      - NAME=tadd
```

---

## 📂 11. Volumes in Compose

```yaml
services:
  web:
    image: nginx
    volumes:
      - ./data:/usr/share/nginx/html
```

---

## 🌐 12. Networks in Compose

```yaml
services:
  app:
    image: ubuntu

  db:
    image: nginx

networks:
  default:
    name: my-network
```

---

## ⚙️ 13. Scale Services

Run multiple containers:

```bash id="2p8kfw"
docker compose up --scale web=3
```

---

## 🧹 14. Remove Everything (Compose Only)

```bash id="8f1zxo"
docker compose down -v
```


---

## 🐳 Docker Basics Course – Part 5 (Real-World Basics & Practical Usage)

---

## 📦 1. Run Temporary Container (Auto Delete)

Useful for testing:

```bash
docker run --rm ubuntu echo "Hello Docker"
````

---

## 📂 2. Mount Current Directory (Quick Dev Setup)

Run container using current folder:

```bash
docker run -d \
  -v $(pwd):/app \
  -w /app \
  ubuntu sleep 3600
```

---

## 🧪 3. Run One-Time Commands

Instead of full container:

```bash
docker run ubuntu ls /
```

---

## 🔄 4. Attach to Running Container

Connect to container output:

```bash
docker attach my-nginx
```

Detach safely:

```
Ctrl + P + Q
```

---

## 📛 5. Assign Container Labels

Useful for organization:

```bash
docker run -d \
  --label project=demo \
  --label env=dev \
  nginx
```

---

## 🔍 6. Filter Containers Using Labels

```bash
docker ps --filter "label=project=demo"
```

---

## 📊 7. Show Port Mapping

```bash
docker port my-nginx
```

---

## 🧠 8. Pause & Unpause Container

Pause processes:

```bash
docker pause my-nginx
```

Resume:

```bash
docker unpause my-nginx
```

---

## 🔌 9. Disconnect Container from Network

```bash
docker network disconnect bridge my-nginx
```

---

## 🔗 10. Connect Container to Network

```bash
docker network connect bridge my-nginx
```

---

## 📁 11. Create Read-Only Container

Prevent file changes:

```bash
docker run -d --read-only nginx
```

---

## 🧱 12. Set Ulimits (System Limits)

```bash
docker run -d \
  --ulimit nofile=1024:2048 \
  nginx
```

---

## 📜 13. Show Disk Usage

```bash
docker system df
```

---

## 🐳 Docker Basics Course – Part 6 (Advanced Commands)

---

## 🧩 1. Create Container Without Running

Create a container but do not start it:

```bash
docker create --name my-container nginx
````

---

## ▶️ 2. Start Container and Attach Output

Start and attach at the same time:

```bash
docker start -a my-container
```

---

## ⏸️ 3. Wait for Container to Stop

Useful in scripts:

```bash
docker wait my-container
```

---

## 📤 4. Rename Volume

Docker doesn’t support direct rename, but you can copy:

```bash
docker run --rm \
  -v old-volume:/from \
  -v new-volume:/to \
  alpine sh -c "cp -a /from/. /to"
```

---

## 📛 5. Remove Volume

```bash
docker volume rm mydata
```

---

## 🔗 6. Inspect Volume Details

```bash
docker volume inspect mydata
```

---

## 🌐 7. Inspect Network Details

```bash
docker network inspect bridge
```

---

## 📡 8. Remove Network

```bash
docker network rm mynet
```

---

## 📦 9. Show Docker System Info (Formatted)

```bash
docker info --format '{{json .}}'
```

---

## 🧪 10. Show Only Container IDs

```bash
docker ps -q
```

---

## 🔍 11. Filter Images

```bash
docker images --filter "dangling=true"
```

---

## 📜 12. Show Only Image IDs

```bash
docker images -q
```

---

### DOCKER **Part 7**

---

## 📦 1. Context Management (Multiple Docker Environments)

List available contexts:

```bash
docker context ls
````

---

### ➕ Create New Context

```bash
docker context create my-context
```

---

### 🔄 Switch Context

```bash 
docker context use my-context
```

---

## 🏗️ 2. Build with BuildKit (Faster Builds)

Enable advanced builder:

```bash 
DOCKER_BUILDKIT=1 docker build .
```

---

## 📤 3. Push All Tags of Image

```bash 
docker push --all-tags username/my-nginx
```

---

## 📥 4. Search Images from CLI

```bash 
docker search nginx
```

---

## 🔐 5. Logout from Docker Hub

```bash 
docker logout
```

---

## 🧾 6. Show Image Digests

```bash 
docker images --digests
```

---

## 📦 7. Remove Unused Images by Filter

```bash
docker image prune --filter "until=24h"
```

---

## 🧠 8. Show Disk Usage (Detailed)

```bash 
docker system df -v
```

---

## 🧩 9. Inspect Specific Field (Formatted Output)

```bash 
docker inspect --format='{{.State.Status}}' my-nginx
```

---

## 📡 10. Show Container Port Internals

```bash 
docker inspect --format='{{.NetworkSettings.Ports}}' my-nginx
```

---

## 🧪 11. Monitor Only Specific Events

```bash 
docker events --filter 'type=container'
```

---

## 🔄 12. Update Restart Policy of Running Container

```bash 
docker update --restart always my-nginx
```

---

## 📁 13. Copy Only Folder Contents (Advanced)

```bash 
docker cp my-nginx:/usr/share/nginx/html/. .
```

---

## 🧱 14. Save Image with Specific Platform

```bash
docker save --platform=linux/amd64 -o image.tar nginx
```

## 🐳 Docker Basics Course – Part 8 (Production Utilities)

---

## 🧰 1. Show Docker Disk Usage by Specific Type

Only show volume usage:

```bash
docker system df --volumes
````

---

## 🗃️ 2. List All Volumes

```bash
docker volume ls
```

---

## 🌐 3. List All Networks

```bash
docker network ls
```

---

## 📁 4. Create a Bind Mount Using --mount

More modern and clearer than `-v`:

```bash
docker run --mount type=bind,source=$(pwd),target=/app ubuntu
```

---

## 💾 5. Create a Named Volume Using --mount

```bash
docker run --mount type=volume,source=mydata,target=/data nginx
```

---

## 🌉 6. Create Custom Bridge Network

```bash
docker network create --driver bridge my-bridge
```

---

## 📡 7. Create Internal Network (No Internet Access)

Containers can talk to each other but not outside internet:

```bash
docker network create --internal secure-net
```

---

## 🔐 8. Create Network with Custom Subnet

```bash
docker network create \
  --subnet=172.20.0.0/16 \
  custom-net
```

---

## 🧪 9. Run Image with Interactive Temporary Shell

```bash
docker run -it --rm alpine sh
```

---

## 📛 10. Remove Multiple Images at Once

```bash
docker rmi image1 image2 image3
```

---

## 🧹 11. Remove Multiple Volumes at Once

```bash
docker volume rm volume1 volume2 volume3
```

---

## 🌐 12. Remove Multiple Networks at Once

```bash
docker network rm network1 network2 network3
```

---

## 📦 13. List Only Image Names and Tags

```bash
docker images --format "{{.Repository}}:{{.Tag}}"
```

---

## 📋 14. List Only Container Names

```bash
docker ps --format "{{.Names}}"
```

---

## 🧠 15. Show Container Exit Code

```bash
docker inspect --format='{{.State.ExitCode}}' my-nginx
```
---

## 🐳 Docker Basics Course – Part 9 (Automation, Backup & Advanced Utilities – No Repeats)

---

## ⏱️ 1. Show Real-Time Resource Usage Once (No Stream)

```bash
docker stats --no-stream
````

---

## 📜 2. Show Only Last Container Logs (By Time)

```bash
docker logs --since 10m my-nginx
```

---

## ⌛ 3. Show Logs Until Specific Time

```bash
docker logs --until 2026-01-01T10:00:00 my-nginx
```

---

## 🔍 4. Inspect Only Container IP Address

```bash
docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' my-nginx
```

---

## 📦 5. Export Image as Compressed Archive

```bash
docker save nginx | gzip > nginx.tar.gz
```

---

## 📥 6. Load Compressed Image

```bash
gunzip -c nginx.tar.gz | docker load
```

---

## 🧹 7. Remove Unused Everything Older Than Time

```bash
docker system prune --filter "until=48h"
```

---

## 📁 8. Show Volume Mount Points

```bash
docker volume inspect --format='{{.Mountpoint}}' mydata
```

---

## 🌐 9. Show Network Subnet Info

```bash
docker network inspect --format='{{range .IPAM.Config}}{{.Subnet}}{{end}}' bridge
```

---

## 🧠 10. Show Container Memory Limit

```bash
docker inspect --format='{{.HostConfig.Memory}}' my-nginx
```

---

## 🔄 11. Copy Entire Container Filesystem (Advanced Backup)

```bash
docker export my-nginx | tar -t
```

---

## 📊 12. Show Running Containers Count

```bash
docker ps -q | wc -l
```

---

## 🧪 13. Show Stopped Containers Only

```bash
docker ps --filter "status=exited"
```

---

## 📛 14. Show Containers by Name Pattern

```bash
docker ps --filter "name=nginx"
```

---
## 🐳 Docker Basics Course – Part 10 (Automation, Scripting & Final Utilities – No Repeats)

---

## 🔁 1. Restart Multiple Containers Using Pattern

```bash
docker ps -q --filter "name=nginx" | xargs docker restart
````

---

## ⏹️ 2. Stop Containers Older Than Time

```bash
docker ps -q --filter "status=running" | xargs -I {} docker inspect --format '{{.Created}} {{.Name}}' {} 
```

(Use this output in scripts to stop old containers)

---

## 🧹 3. Remove Containers by Status

```bash
docker ps -q --filter "status=created" | xargs docker rm
```

---

## 📦 4. Remove Images by Pattern

```bash
docker images | grep nginx | awk '{print $3}' | xargs docker rmi
```

---

## 📁 5. Backup Volume Data to File

```bash
docker run --rm \
  -v mydata:/volume \
  -v $(pwd):/backup \
  alpine tar czf /backup/backup.tar.gz -C /volume .
```

---

## 📥 6. Restore Volume Data from Backup

```bash
docker run --rm \
  -v mydata:/volume \
  -v $(pwd):/backup \
  alpine tar xzf /backup/backup.tar.gz -C /volume
```

---

## 📊 7. Show Top CPU Consuming Containers

```bash
docker stats --no-stream | sort -k3 -r
```

---

## 🧠 8. Show Containers with Restart Policy

```bash
docker inspect --format='{{.Name}} {{.HostConfig.RestartPolicy.Name}}' $(docker ps -aq)
```

---

## 🔍 9. Find Containers Without Limits

```bash
docker inspect --format='{{.Name}} {{.HostConfig.Memory}}' $(docker ps -aq) | grep "0"
```

---

## 📛 10. Rename Images in Bulk (Tag + Remove Old)

```bash
docker images | grep nginx | awk '{print $1":"$2}' | xargs -I {} docker tag {} myrepo/{}
```

---

## 🧪 11. Dry Run Style Check (List Before Delete)

```bash
docker images --filter "dangling=true"
```

---

## ⚙️ 12. Show Full Container Config in JSON File

```bash
docker inspect my-nginx > config.json
```

---

## 📡 13. Monitor Events for Specific Container

```bash
docker events --filter "container=my-nginx"
```

---

## 🧩 14. Combine Filters for Precision

```b
docker ps --filter "status=running" --filter "ancestor=nginx"
```

--- 
[JOIN FOR LIVE CLASSES AND SUPPORT](https://t.me/efxtv) 

[![Buy Me A Coffee](https://img.buymeacoffee.com/button-api/?text=Buy%20me%20a%20coffee&emoji=&slug=efxtv&button_colour=FFDD00&font_colour=000000&font_family=Cookie&outline_colour=000000&coffee_colour=ffffff)](https://buymeacoffee.com/efxtv)
