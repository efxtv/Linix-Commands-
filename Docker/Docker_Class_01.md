# COMPLETE DOCKER COURSE - BEGINNER TO ADVANCED

## TABLE OF CONTENTS
1. Installation & Setup
2. Docker Basics
3. Working with Images
4. Working with Containers
5. Docker Volumes
6. Docker Networks
7. Dockerfile Creation
8. Docker Compose
9. Docker Registry & Hub
10. Advanced Docker Commands
11. Docker Swarm
12. Monitoring & Logging
13. Security & Best Practices
14. Troubleshooting

---

## 1. INSTALLATION & SETUP

### 1. Check if Docker is installed
```
$ docker --version
```

### 2. Display Docker system information
```
$ docker info
```

### 3. Display detailed Docker system information
```
$ docker system info
```

### 4. Install Docker on Ubuntu/Debian
```
$ sudo apt-get update
$ sudo apt-get install docker.io -y
```

### 5. Install Docker on CentOS/RHEL
```
$ sudo yum install docker -y
```

### 6. Start Docker service
```
$ sudo systemctl start docker
```

### 7. Enable Docker to start on boot
```
$ sudo systemctl enable docker
```

### 8. Check Docker service status
```
$ sudo systemctl status docker
```

### 9. Add current user to docker group (avoid sudo)
```
$ sudo usermod -aG docker $USER
```

### 10. Apply group changes (logout and login or run)
```
$ newgrp docker
```

### 11. Verify Docker installation with test container
```
$ docker run hello-world
```

### 12. Display Docker version details
```
$ docker version
```

### 13. Check Docker Compose version
```
$ docker-compose --version
```

---

## 2. DOCKER BASICS

### 14. Display all Docker commands
```
$ docker --help
```

### 15. Get help for specific command
```
$ docker run --help
```

### 16. Display running containers
```
$ docker ps
```

### 17. Display all containers (running and stopped)
```
$ docker ps -a
```

### 18. Display all containers with size
```
$ docker ps -as
```

### 19. Display latest created container
```
$ docker ps -l
```

### 20. Display container IDs only
```
$ docker ps -q
```

### 21. Display all container IDs (including stopped)
```
$ docker ps -aq
```

### 22. Display containers with custom format
```
$ docker ps --format "table {{.ID}}\t{{.Image}}\t{{.Status}}"
```

### 23. Filter containers by status
```
$ docker ps -a --filter "status=exited"
```

### 24. Filter containers by name
```
$ docker ps -a --filter "name=mycontainer"
```

---

## 3. WORKING WITH IMAGES

### 25. List all Docker images
```
$ docker images
```

### 26. List all images with digests
```
$ docker images --digests
```

### 27. List all images including intermediate
```
$ docker images -a
```

### 28. List image IDs only
```
$ docker images -q
```

### 29. Search for image on Docker Hub
```
$ docker search ubuntu
```

### 30. Search for official images only
```
$ docker search --filter "is-official=true" ubuntu
```

### 31. Limit search results
```
$ docker search --limit 5 nginx
```

### 32. Pull an image from Docker Hub
```
$ docker pull ubuntu
```

### 33. Pull specific version/tag of image
```
$ docker pull ubuntu:20.04
```

### 34. Pull image from specific registry
```
$ docker pull gcr.io/google-containers/busybox
```

### 35. Pull all tags of an image
```
$ docker pull -a ubuntu
```

### 36. Display image history/layers
```
$ docker history ubuntu
```

### 37. Display detailed image information
```
$ docker inspect ubuntu
```

### 38. Display image digest
```
$ docker inspect --format='{{.RepoDigests}}' ubuntu
```

### 39. Display image size
```
$ docker inspect --format='{{.Size}}' ubuntu
```

### 40. Tag an image
```
$ docker tag ubuntu:latest myubuntu:v1
```

### 41. Tag image for private registry
```
$ docker tag ubuntu:latest myregistry.com:5000/ubuntu:latest
```

### 42. Remove an image
```
$ docker rmi ubuntu
```

### 43. Force remove an image
```
$ docker rmi -f ubuntu
```

### 44. Remove multiple images
```
$ docker rmi ubuntu nginx alpine
```

### 45. Remove all unused images
```
$ docker image prune
```

### 46. Remove all images
```
$ docker rmi $(docker images -q)
```

### 47. Remove dangling images (untagged)
```
$ docker image prune -a
```

### 48. Save image to tar archive
```
$ docker save ubuntu -o ubuntu.tar
```

### 49. Save multiple images to archive
```
$ docker save ubuntu nginx -o images.tar
```

### 50. Load image from tar archive
```
$ docker load -i ubuntu.tar
```

### 51. Export container filesystem to tar
```
$ docker export mycontainer -o mycontainer.tar
```

### 52. Import filesystem from tar to create image
```
$ docker import mycontainer.tar myimage:latest
```

---

## 4. WORKING WITH CONTAINERS

### 53. Run a container from image
```
$ docker run ubuntu
```

### 54. Run container with interactive terminal
```
$ docker run -it ubuntu /bin/bash
```

### 55. Run container in detached mode (background)
```
$ docker run -d nginx
```

### 56. Run container with custom name
```
$ docker run --name mycontainer nginx
```

### 57. Run container and remove after exit
```
$ docker run --rm ubuntu echo "Hello Docker"
```

### 58. Run container with port mapping
```
$ docker run -d -p 8080:80 nginx
```

### 59. Run container with random port mapping
```
$ docker run -d -P nginx
```

### 60. Run container with multiple port mappings
```
$ docker run -d -p 8080:80 -p 8443:443 nginx
```

### 61. Run container with environment variable
```
$ docker run -e MY_VAR=value ubuntu env
```

### 62. Run container with multiple environment variables
```
$ docker run -e VAR1=value1 -e VAR2=value2 ubuntu
```

### 63. Run container with environment file
```
$ docker run --env-file ./env.list ubuntu
```

### 64. Run container with volume mount
```
$ docker run -v /host/path:/container/path ubuntu
```

### 65. Run container with read-only volume
```
$ docker run -v /host/path:/container/path:ro ubuntu
```

### 66. Run container with named volume
```
$ docker run -v myvolume:/data ubuntu
```

### 67. Run container with working directory
```
$ docker run -w /app ubuntu pwd
```

### 68. Run container with hostname
```
$ docker run --hostname myhost ubuntu hostname
```

### 69. Run container with user
```
$ docker run --user 1000:1000 ubuntu
```

### 70. Run container with memory limit
```
$ docker run -m 512m ubuntu
```

### 71. Run container with CPU limit
```
$ docker run --cpus=1.5 ubuntu
```

### 72. Run container with restart policy
```
$ docker run --restart=always nginx
```

### 73. Run container with 'unless-stopped' restart policy
```
$ docker run --restart=unless-stopped nginx
```

### 74. Run container with 'on-failure' restart policy
```
$ docker run --restart=on-failure:5 nginx
```

### 75. Run container with network mode
```
$ docker run --network=host nginx
```

### 76. Run container attached to specific network
```
$ docker run --network=mynetwork nginx
```

### 77. Run container with DNS server
```
$ docker run --dns=8.8.8.8 ubuntu
```

### 78. Run container with additional host entry
```
$ docker run --add-host=myhost:192.168.1.100 ubuntu
```

### 79. Run container with custom MAC address
```
$ docker run --mac-address=92:d0:c6:0a:29:33 ubuntu
```

### 80. Run container in privileged mode
```
$ docker run --privileged ubuntu
```

### 81. Start a stopped container
```
$ docker start mycontainer
```

### 82. Start multiple containers
```
$ docker start container1 container2
```

### 83. Start container and attach to it
```
$ docker start -a mycontainer
```

### 84. Stop a running container
```
$ docker stop mycontainer
```

### 85. Stop container with timeout
```
$ docker stop -t 30 mycontainer
```

### 86. Stop all running containers
```
$ docker stop $(docker ps -q)
```

### 87. Restart a container
```
$ docker restart mycontainer
```

### 88. Kill a container (forcefully)
```
$ docker kill mycontainer
```

### 89. Kill container with specific signal
```
$ docker kill -s SIGTERM mycontainer
```

### 90. Pause a running container
```
$ docker pause mycontainer
```

### 91. Unpause a paused container
```
$ docker unpause mycontainer
```

### 92. Attach to a running container
```
$ docker attach mycontainer
```

### 93. Execute command in running container
```
$ docker exec mycontainer ls /app
```

### 94. Execute interactive command in container
```
$ docker exec -it mycontainer /bin/bash
```

### 95. Execute command as specific user
```
$ docker exec -u root mycontainer whoami
```

### 96. Execute command with environment variable
```
$ docker exec -e VAR=value mycontainer env
```

### 97. Execute command in specific directory
```
$ docker exec -w /app mycontainer pwd
```

### 98. Display container logs
```
$ docker logs mycontainer
```

### 99. Follow container logs in real-time
```
$ docker logs -f mycontainer
```

### 100. Display last N lines of logs
```
$ docker logs --tail 100 mycontainer
```

### 101. Display logs with timestamps
```
$ docker logs -t mycontainer
```

### 102. Display logs since specific time
```
$ docker logs --since 2024-01-01T00:00:00 mycontainer
```

### 103. Display logs until specific time
```
$ docker logs --until 2024-01-01T23:59:59 mycontainer
```

### 104. Inspect container details
```
$ docker inspect mycontainer
```

### 105. Get specific information using format
```
$ docker inspect --format='{{.NetworkSettings.IPAddress}}' mycontainer
```

### 106. Get container state
```
$ docker inspect --format='{{.State.Status}}' mycontainer
```

### 107. Display container port mappings
```
$ docker port mycontainer
```

### 108. Display specific port mapping
```
$ docker port mycontainer 80
```

### 109. Display container processes
```
$ docker top mycontainer
```

### 110. Display container with custom ps format
```
$ docker top mycontainer aux
```

### 111. Display container resource usage statistics
```
$ docker stats mycontainer
```

### 112. Display stats for all containers
```
$ docker stats
```

### 113. Display stats without streaming
```
$ docker stats --no-stream
```

### 114. Display container filesystem changes
```
$ docker diff mycontainer
```

### 115. Copy file from container to host
```
$ docker cp mycontainer:/app/file.txt /host/path/
```

### 116. Copy file from host to container
```
$ docker cp /host/file.txt mycontainer:/app/
```

### 117. Copy directory from container
```
$ docker cp mycontainer:/app/folder /host/path/
```

### 118. Rename a container
```
$ docker rename oldname newname
```

### 119. Update container configuration
```
$ docker update --memory 512m mycontainer
```

### 120. Update container restart policy
```
$ docker update --restart=always mycontainer
```

### 121. Update CPU shares for container
```
$ docker update --cpu-shares 512 mycontainer
```

### 122. Wait for container to stop
```
$ docker wait mycontainer
```

### 123. Remove a stopped container
```
$ docker rm mycontainer
```

### 124. Force remove a running container
```
$ docker rm -f mycontainer
```

### 125. Remove multiple containers
```
$ docker rm container1 container2 container3
```

### 126. Remove all stopped containers
```
$ docker container prune
```

### 127. Remove container and its volumes
```
$ docker rm -v mycontainer
```

### 128. Remove all containers (stopped and running)
```
$ docker rm -f $(docker ps -aq)
```

### 129. Commit container changes to new image
```
$ docker commit mycontainer mynewimage:v1
```

### 130. Commit with author and message
```
$ docker commit -a "Author Name" -m "Commit message" mycontainer myimage:v1
```

### 131. Commit with CMD change
```
$ docker commit --change='CMD ["nginx", "-g", "daemon off;"]' mycontainer myimage
```

---

## 5. DOCKER VOLUMES

### 132. List all volumes
```
$ docker volume ls
```

### 133. Create a named volume
```
$ docker volume create myvolume
```

### 134. Create volume with driver options
```
$ docker volume create --driver local --opt type=none --opt device=/host/path --opt o=bind myvolume
```

### 135. Inspect volume details
```
$ docker volume inspect myvolume
```

### 136. Remove a volume
```
$ docker volume rm myvolume
```

### 137. Remove all unused volumes
```
$ docker volume prune
```

### 138. Remove volumes with filter
```
$ docker volume prune --filter "label!=keep"
```

### 139. Create volume with label
```
$ docker volume create --label environment=production myvolume
```

### 140. List volumes with filter
```
$ docker volume ls --filter "dangling=true"
```

### 141. Mount volume to container
```
$ docker run -v myvolume:/data ubuntu
```

### 142. Mount bind volume from host
```
$ docker run -v /host/path:/container/path ubuntu
```

### 143. Mount volume with read-only access
```
$ docker run -v myvolume:/data:ro ubuntu
```

### 144. Use --mount syntax for volumes
```
$ docker run --mount source=myvolume,target=/data ubuntu
```

### 145. Mount tmpfs volume (in-memory)
```
$ docker run --mount type=tmpfs,destination=/app ubuntu
```

### 146. Mount volume with specific driver
```
$ docker run --mount type=volume,source=myvolume,target=/data,volume-driver=local ubuntu
```

### 147. Backup volume to tar file
```
$ docker run --rm -v myvolume:/data -v $(pwd):/backup ubuntu tar czf /backup/backup.tar.gz /data
```

### 148. Restore volume from backup
```
$ docker run --rm -v myvolume:/data -v $(pwd):/backup ubuntu tar xzf /backup/backup.tar.gz -C /
```

### 149. Copy data between volumes
```
$ docker run --rm -v volume1:/from -v volume2:/to ubuntu cp -r /from/* /to/
```

---

## 6. DOCKER NETWORKS

### 150. List all networks
```
$ docker network ls
```

### 151. Create a bridge network
```
$ docker network create mynetwork
```

### 152. Create network with specific driver
```
$ docker network create --driver bridge mynetwork
```

### 153. Create network with subnet
```
$ docker network create --subnet=172.18.0.0/16 mynetwork
```

### 154. Create network with gateway
```
$ docker network create --gateway=172.18.0.1 --subnet=172.18.0.0/16 mynetwork
```

### 155. Create network with IP range
```
$ docker network create --subnet=172.18.0.0/16 --ip-range=172.18.5.0/24 mynetwork
```

### 156. Create overlay network (for Swarm)
```
$ docker network create --driver overlay myoverlay
```

### 157. Create internal network (no external access)
```
$ docker network create --internal mynetwork
```

### 158. Create network with custom MTU
```
$ docker network create --opt com.docker.network.driver.mtu=1450 mynetwork
```

### 159. Inspect network details
```
$ docker network inspect mynetwork
```

### 160. Connect container to network
```
$ docker network connect mynetwork mycontainer
```

### 161. Connect container with specific IP
```
$ docker network connect --ip 172.18.0.10 mynetwork mycontainer
```

### 162. Connect container with alias
```
$ docker network connect --alias webapp mynetwork mycontainer
```

### 163. Disconnect container from network
```
$ docker network disconnect mynetwork mycontainer
```

### 164. Force disconnect container
```
$ docker network disconnect -f mynetwork mycontainer
```

### 165. Remove a network
```
$ docker network rm mynetwork
```

### 166. Remove all unused networks
```
$ docker network prune
```

### 167. Remove networks with filter
```
$ docker network prune --filter "until=24h"
```

### 168. Run container on specific network
```
$ docker run --network=mynetwork nginx
```

### 169. Run container with network alias
```
$ docker run --network=mynetwork --network-alias=webapp nginx
```

### 170. Disable networking for container
```
$ docker run --network=none ubuntu
```

### 171. Use host network mode
```
$ docker run --network=host nginx
```

### 172. Link containers (legacy method)
```
$ docker run --link container1:alias ubuntu
```

---

## 7. DOCKERFILE CREATION

### 173. Create basic Dockerfile
```
$ cat > Dockerfile << EOF
FROM ubuntu:20.04
RUN apt-get update && apt-get install -y nginx
CMD ["nginx", "-g", "daemon off;"]
EOF
```

### 174. Dockerfile with LABEL
```
FROM ubuntu:20.04
LABEL maintainer="your@email.com"
LABEL version="1.0"
LABEL description="My application"
```

### 175. Dockerfile with ENV variables
```
FROM ubuntu:20.04
ENV APP_HOME=/app
ENV APP_VERSION=1.0
```

### 176. Dockerfile with ARG (build-time variables)
```
FROM ubuntu:20.04
ARG DEBIAN_FRONTEND=noninteractive
ARG BUILD_DATE
```

### 177. Dockerfile with WORKDIR
```
FROM ubuntu:20.04
WORKDIR /app
RUN pwd
```

### 178. Dockerfile with COPY
```
FROM ubuntu:20.04
COPY ./app /app
COPY file.txt /destination/
```

### 179. Dockerfile with ADD (can extract archives)
```
FROM ubuntu:20.04
ADD archive.tar.gz /app/
ADD https://example.com/file.txt /app/
```

### 180. Dockerfile with RUN commands
```
FROM ubuntu:20.04
RUN apt-get update
RUN apt-get install -y python3
```

### 181. Dockerfile with combined RUN (best practice)
```
FROM ubuntu:20.04
RUN apt-get update && \
    apt-get install -y python3 && \
    rm -rf /var/lib/apt/lists/*
```

### 182. Dockerfile with EXPOSE
```
FROM nginx:latest
EXPOSE 80
EXPOSE 443
```

### 183. Dockerfile with USER
```
FROM ubuntu:20.04
RUN useradd -m myuser
USER myuser
```

### 184. Dockerfile with VOLUME
```
FROM ubuntu:20.04
VOLUME ["/data", "/logs"]
```

### 185. Dockerfile with CMD (default command)
```
FROM ubuntu:20.04
CMD ["echo", "Hello World"]
```

### 186. Dockerfile with ENTRYPOINT
```
FROM ubuntu:20.04
ENTRYPOINT ["python3"]
CMD ["app.py"]
```

### 187. Dockerfile with HEALTHCHECK
```
FROM nginx:latest
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost/ || exit 1
```

### 188. Dockerfile with STOPSIGNAL
```
FROM nginx:latest
STOPSIGNAL SIGTERM
```

### 189. Dockerfile with SHELL
```
FROM ubuntu:20.04
SHELL ["/bin/bash", "-c"]
```

### 190. Dockerfile with ONBUILD triggers
```
FROM node:14
ONBUILD COPY package.json /app/
ONBUILD RUN npm install
```

### 191. Multi-stage Dockerfile
```
FROM node:14 AS builder
WORKDIR /app
COPY . .
RUN npm install && npm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
```

### 192. Build image from Dockerfile
```
$ docker build -t myimage:v1 .
```

### 193. Build with custom Dockerfile name
```
$ docker build -f Dockerfile.custom -t myimage:v1 .
```

### 194. Build with build arguments
```
$ docker build --build-arg VERSION=1.0 -t myimage:v1 .
```

### 195. Build without cache
```
$ docker build --no-cache -t myimage:v1 .
```

### 196. Build with tag
```
$ docker build -t username/myimage:latest .
```

### 197. Build with multiple tags
```
$ docker build -t myimage:v1 -t myimage:latest .
```

### 198. Build and show output
```
$ docker build --progress=plain -t myimage:v1 .
```

### 199. Build with target stage (multi-stage)
```
$ docker build --target builder -t myimage:builder .
```

### 200. Build with custom context
```
$ docker build -t myimage:v1 -f /path/to/Dockerfile /path/to/context
```

### 201. Build from URL
```
$ docker build -t myimage:v1 https://github.com/user/repo.git
```

### 202. Build from stdin
```
$ docker build -t myimage:v1 - < Dockerfile
```

### 203. Build with labels
```
$ docker build --label version=1.0 --label env=prod -t myimage:v1 .
```

### 204. Build with memory limit
```
$ docker build --memory=1g -t myimage:v1 .
```

### 205. Build with CPU limit
```
$ docker build --cpu-shares=1024 -t myimage:v1 .
```

### 206. Build with pull always
```
$ docker build --pull -t myimage:v1 .
```

### 207. Build with squash (experimental)
```
$ docker build --squash -t myimage:v1 .
```

---

## 8. DOCKER COMPOSE

### 208. Check Docker Compose version
```
$ docker-compose version
```

### 209. Create basic docker-compose.yml
```
$ cat > docker-compose.yml << EOF
version: '3.8'
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
EOF
```

### 210. Docker Compose with build
```
version: '3.8'
services:
  app:
    build: .
    ports:
      - "5000:5000"
```

### 211. Docker Compose with build context
```
version: '3.8'
services:
  app:
    build:
      context: ./app
      dockerfile: Dockerfile.custom
```

### 212. Docker Compose with environment variables
```
version: '3.8'
services:
  app:
    image: myapp
    environment:
      - DB_HOST=database
      - DB_PORT=5432
```

### 213. Docker Compose with env_file
```
version: '3.8'
services:
  app:
    image: myapp
    env_file:
      - .env
      - .env.production
```

### 214. Docker Compose with volumes
```
version: '3.8'
services:
  app:
    image: myapp
    volumes:
      - ./data:/app/data
      - myvolume:/app/storage

volumes:
  myvolume:
```

### 215. Docker Compose with networks
```
version: '3.8'
services:
  app:
    image: myapp
    networks:
      - frontend
      - backend

networks:
  frontend:
  backend:
```

### 216. Docker Compose with depends_on
```
version: '3.8'
services:
  web:
    image: nginx
    depends_on:
      - app
  app:
    image: myapp
```

### 217. Docker Compose with restart policy
```
version: '3.8'
services:
  app:
    image: myapp
    restart: always
```

### 218. Docker Compose with healthcheck
```
version: '3.8'
services:
  app:
    image: myapp
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost"]
      interval: 30s
      timeout: 10s
      retries: 3
```

### 219. Start services with Docker Compose
```
$ docker-compose up
```

### 220. Start services in detached mode
```
$ docker-compose up -d
```

### 221. Start specific service
```
$ docker-compose up web
```

### 222. Build images before starting
```
$ docker-compose up --build
```

### 223. Force recreate containers
```
$ docker-compose up --force-recreate
```

### 224. Don't start dependent services
```
$ docker-compose up --no-deps web
```

### 225. Scale service to multiple instances
```
$ docker-compose up --scale web=3
```

### 226. Stop services
```
$ docker-compose stop
```

### 227. Stop specific service
```
$ docker-compose stop web
```

### 228. Stop with timeout
```
$ docker-compose stop -t 30
```

### 229. Start stopped services
```
$ docker-compose start
```

### 230. Restart services
```
$ docker-compose restart
```

### 231. Pause services
```
$ docker-compose pause
```

### 232. Unpause services
```
$ docker-compose unpause
```

### 233. View service logs
```
$ docker-compose logs
```

### 234. Follow service logs
```
$ docker-compose logs -f
```

### 235. View logs for specific service
```
$ docker-compose logs web
```

### 236. View last N lines of logs
```
$ docker-compose logs --tail=100
```

### 237. List running services
```
$ docker-compose ps
```

### 238. List all services (including stopped)
```
$ docker-compose ps -a
```

### 239. Execute command in service
```
$ docker-compose exec web bash
```

### 240. Execute command without TTY
```
$ docker-compose exec -T web ls
```

### 241. Run one-off command
```
$ docker-compose run web python script.py
```

### 242. Run command without starting dependencies
```
$ docker-compose run --no-deps web command
```

### 243. Run command and remove container after
```
$ docker-compose run --rm web command
```

### 244. Build or rebuild services
```
$ docker-compose build
```

### 245. Build specific service
```
$ docker-compose build web
```

### 246. Build without cache
```
$ docker-compose build --no-cache
```

### 247. Build with parallel builds
```
$ docker-compose build --parallel
```

### 248. Pull service images
```
$ docker-compose pull
```

### 249. Pull specific service image
```
$ docker-compose pull web
```

### 250. Push service images
```
$ docker-compose push
```

### 251. Validate compose file
```
$ docker-compose config
```

### 252. Validate and view resolved config
```
$ docker-compose config --resolve-image-digests
```

### 253. List images used by services
```
$ docker-compose images
```

### 254. Display service ports
```
$ docker-compose port web 80
```

### 255. View service processes
```
$ docker-compose top
```

### 256. Stop and remove containers, networks
```
$ docker-compose down
```

### 257. Remove containers and volumes
```
$ docker-compose down -v
```

### 258. Remove containers and images
```
$ docker-compose down --rmi all
```

### 259. Remove only images built by compose
```
$ docker-compose down --rmi local
```

### 260. Specify custom compose file
```
$ docker-compose -f custom-compose.yml up
```

### 261. Use multiple compose files
```
$ docker-compose -f docker-compose.yml -f docker-compose.prod.yml up
```

### 262. Set project name
```
$ docker-compose -p myproject up
```

### 263. Create services without starting
```
$ docker-compose create
```

### 264. Kill services
```
$ docker-compose kill
```

### 265. Remove stopped service containers
```
$ docker-compose rm
```

### 266. Force remove without confirmation
```
$ docker-compose rm -f
```

---

## 9. DOCKER REGISTRY & HUB

### 267. Login to Docker Hub
```
$ docker login
```

### 268. Login with username and password
```
$ docker login -u username -p password
```

### 269. Login to private registry
```
$ docker login myregistry.com:5000
```

### 270. Logout from Docker Hub
```
$ docker logout
```

### 271. Logout from private registry
```
$ docker logout myregistry.com:5000
```

### 272. Push image to Docker Hub
```
$ docker push username/myimage:latest
```

### 273. Push all tags of an image
```
$ docker push -a username/myimage
```

### 274. Push to private registry
```
$ docker push myregistry.com:5000/myimage:latest
```

### 275. Pull from private registry
```
$ docker pull myregistry.com:5000/myimage:latest
```

### 276. Run local Docker registry
```
$ docker run -d -p 5000:5000 --name registry registry:2
```

### 277. Run registry with volume
```
$ docker run -d -p 5000:5000 -v /mnt/registry:/var/lib/registry registry:2
```

### 278. Tag image for local registry
```
$ docker tag myimage localhost:5000/myimage
```

### 279. Push to local registry
```
$ docker push localhost:5000/myimage
```

### 280. List images in registry (using API)
```
$ curl -X GET http://localhost:5000/v2/_catalog
```

### 281. List image tags in registry
```
$ curl -X GET http://localhost:5000/v2/myimage/tags/list
```

### 282. Delete image from registry
```
$ docker exec registry bin/registry garbage-collect /etc/docker/registry/config.yml
```

---

## 10. ADVANCED DOCKER COMMANDS

### 283. Display Docker disk usage
```
$ docker system df
```

### 284. Display detailed disk usage
```
$ docker system df -v
```

### 285. Clean up unused data
```
$ docker system prune
```

### 286. Clean up everything including volumes
```
$ docker system prune -a --volumes
```

### 287. Clean up with force (no confirmation)
```
$ docker system prune -f
```

### 288. Filter cleanup by time
```
$ docker system prune --filter "until=24h"
```

### 289. Display Docker events in real-time
```
$ docker events
```

### 290. Filter events by type
```
$ docker events --filter type=container
```

### 291. Filter events by container
```
$ docker events --filter container=mycontainer
```

### 292. Filter events by event type
```
$ docker events --filter event=start
```

### 293. Display events since specific time
```
$ docker events --since '2024-01-01T00:00:00'
```

### 294. Create container checkpoint (experimental)
```
$ docker checkpoint create mycontainer checkpoint1
```

### 295. List container checkpoints
```
$ docker checkpoint ls mycontainer
```

### 296. Restore from checkpoint
```
$ docker start --checkpoint checkpoint1 mycontainer
```

### 297. Remove checkpoint
```
$ docker checkpoint rm mycontainer checkpoint1
```

### 298. Create container configuration
```
$ docker container create --name mycontainer nginx
```

### 299. Display container configuration differences
```
$ docker container diff mycontainer
```

### 300. Export container to tarball
```
$ docker container export mycontainer -o container.tar
```

### 301. Inspect container metadata
```
$ docker container inspect mycontainer
```

### 302. Kill container with signal
```
$ docker container kill --signal=SIGHUP mycontainer
```

### 303. View container logs with details
```
$ docker container logs --details mycontainer
```

### 304. Display running processes in container
```
$ docker container top mycontainer
```

### 305. Block until container stops
```
$ docker container wait mycontainer
```

### 306. Attach local stdin/stdout to container
```
$ docker container attach --sig-proxy=false mycontainer
```

### 307. Create image from container
```
$ docker container commit mycontainer newimage:tag
```

### 308. Copy files between container and host
```
$ docker container cp mycontainer:/file.txt ./
```

### 309. Get real-time events from container
```
$ docker container events
```

### 310. Pause all processes in container
```
$ docker container pause mycontainer
```

### 311. Display port mappings
```
$ docker container port mycontainer
```

### 312. Rename container
```
$ docker container rename old_name new_name
```

### 313. Restart container with timeout
```
$ docker container restart -t 10 mycontainer
```

### 314. Start container
```
$ docker container start mycontainer
```

### 315. Display resource usage statistics
```
$ docker container stats mycontainer
```

### 316. Stop container gracefully
```
$ docker container stop mycontainer
```

### 317. Unpause container processes
```
$ docker container unpause mycontainer
```

### 318. Update container resource limits
```
$ docker container update --memory 512m --cpus 2 mycontainer
```

### 319. Create image from Dockerfile
```
$ docker image build -t myimage .
```

### 320. Display image creation history
```
$ docker image history myimage
```

### 321. Import tarball as image
```
$ docker image import file.tar myimage:tag
```

### 322. Inspect image details
```
$ docker image inspect myimage
```

### 323. Load images from tar archive
```
$ docker image load -i images.tar
```

### 324. List images with filters
```
$ docker image ls --filter "dangling=true"
```

### 325. Remove unused images
```
$ docker image prune -a
```

### 326. Pull image from registry
```
$ docker image pull nginx:latest
```

### 327. Push image to registry
```
$ docker image push myimage:tag
```

### 328. Remove image by ID
```
$ docker image rm abc123
```

### 329. Save images to tar
```
$ docker image save -o images.tar image1 image2
```

### 330. Tag image with new name
```
$ docker image tag source:tag target:tag
```

---

## 11. DOCKER SWARM

### 331. Initialize Docker Swarm
```
$ docker swarm init
```

### 332. Initialize Swarm with advertise address
```
$ docker swarm init --advertise-addr 192.168.1.100
```

### 333. Display Swarm join token for worker
```
$ docker swarm join-token worker
```

### 334. Display join token for manager
```
$ docker swarm join-token manager
```

### 335. Rotate join token
```
$ docker swarm join-token --rotate worker
```

### 336. Join Swarm as worker
```
$ docker swarm join --token TOKEN 192.168.1.100:2377
```

### 337. Join Swarm as manager
```
$ docker swarm join --token TOKEN 192.168.1.100:2377
```

### 338. Leave Swarm
```
$ docker swarm leave
```

### 339. Force leave Swarm (for manager)
```
$ docker swarm leave --force
```

### 340. Update Swarm configuration
```
$ docker swarm update --task-history-limit 5
```

### 341. Unlock Swarm
```
$ docker swarm unlock
```

### 342. Get unlock key
```
$ docker swarm unlock-key
```

### 343. Rotate unlock key
```
$ docker swarm unlock-key --rotate
```

### 344. List Swarm nodes
```
$ docker node ls
```

### 345. Inspect node details
```
$ docker node inspect nodename
```

### 346. Promote node to manager
```
$ docker node promote nodename
```

### 347. Demote manager to worker
```
$ docker node demote nodename
```

### 348. Remove node from Swarm
```
$ docker node rm nodename
```

### 349. Update node availability
```
$ docker node update --availability drain nodename
```

### 350. Set node to active
```
$ docker node update --availability active nodename
```

### 351. Add label to node
```
$ docker node update --label-add type=web nodename
```

### 352. Remove label from node
```
$ docker node update --label-rm type nodename
```

### 353. Create a service
```
$ docker service create --name myservice nginx
```

### 354. Create service with replicas
```
$ docker service create --name myservice --replicas 3 nginx
```

### 355. Create service with port publishing
```
$ docker service create --name myservice -p 8080:80 nginx
```

### 356. Create service with environment variables
```
$ docker service create --name myservice -e KEY=value nginx
```

### 357. Create service with mount
```
$ docker service create --name myservice --mount type=volume,source=myvol,target=/data nginx
```

### 358. Create service with constraints
```
$ docker service create --name myservice --constraint node.role==worker nginx
```

### 359. Create service with placement preferences
```
$ docker service create --name myservice --placement-pref spread=node.labels.zone nginx
```

### 360. Create service with update config
```
$ docker service create --name myservice --update-delay 10s --update-parallelism 2 nginx
```

### 361. Create service with rollback config
```
$ docker service create --name myservice --rollback-delay 5s nginx
```

### 362. Create service with restart policy
```
$ docker service create --name myservice --restart-condition on-failure nginx
```

### 363. List services
```
$ docker service ls
```

### 364. List services with filter
```
$ docker service ls --filter name=myservice
```

### 365. Inspect service
```
$ docker service inspect myservice
```

### 366. Display service logs
```
$ docker service logs myservice
```

### 367. Follow service logs
```
$ docker service logs -f myservice
```

### 368. List service tasks
```
$ docker service ps myservice
```

### 369. Scale service
```
$ docker service scale myservice=5
```

### 370. Update service image
```
$ docker service update --image nginx:alpine myservice
```

### 371. Update service replicas
```
$ docker service update --replicas 3 myservice
```

### 372. Add published port to service
```
$ docker service update --publish-add 8080:80 myservice
```

### 373. Remove published port from service
```
$ docker service update --publish-rm 8080:80 myservice
```

### 374. Add environment variable to service
```
$ docker service update --env-add KEY=value myservice
```

### 375. Remove environment variable
```
$ docker service update --env-rm KEY myservice
```

### 376. Update service constraints
```
$ docker service update --constraint-add node.role==worker myservice
```

### 377. Force service update
```
$ docker service update --force myservice
```

### 378. Rollback service update
```
$ docker service rollback myservice
```

### 379. Remove service
```
$ docker service rm myservice
```

### 380. Create service network
```
$ docker network create --driver overlay myoverlay
```

### 381. Create encrypted overlay network
```
$ docker network create --driver overlay --opt encrypted myoverlay
```

### 382. Create service with network
```
$ docker service create --name myservice --network myoverlay nginx
```

### 383. Create stack from compose file
```
$ docker stack deploy -c docker-compose.yml mystack
```

### 384. List stacks
```
$ docker stack ls
```

### 385. List stack services
```
$ docker stack services mystack
```

### 386. List stack tasks
```
$ docker stack ps mystack
```

### 387. Remove stack
```
$ docker stack rm mystack
```

### 388. Create secret
```
$ echo "secret_data" | docker secret create mysecret -
```

### 389. Create secret from file
```
$ docker secret create mysecret ./secret.txt
```

### 390. List secrets
```
$ docker secret ls
```

### 391. Inspect secret
```
$ docker secret inspect mysecret
```

### 392. Remove secret
```
$ docker secret rm mysecret
```

### 393. Create service with secret
```
$ docker service create --name myservice --secret mysecret nginx
```

### 394. Create config
```
$ docker config create myconfig ./config.txt
```

### 395. List configs
```
$ docker config ls
```

### 396. Inspect config
```
$ docker config inspect myconfig
```

### 397. Remove config
```
$ docker config rm myconfig
```

### 398. Create service with config
```
$ docker service create --name myservice --config myconfig nginx
```

---

## 12. MONITORING & LOGGING

### 399. Monitor container CPU and memory usage
```
$ docker stats --no-stream
```

### 400. Monitor specific containers
```
$ docker stats container1 container2
```

### 401. Format stats output
```
$ docker stats --format "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}"
```

### 402. View container resource limits
```
$ docker inspect --format='{{.HostConfig.Memory}}' mycontainer
```

### 403. Stream container logs
```
$ docker logs -f mycontainer
```

### 404. View logs since specific time
```
$ docker logs --since 2024-01-01T00:00:00 mycontainer
```

### 405. View logs until specific time
```
$ docker logs --until 2024-01-01T23:59:59 mycontainer
```

### 406. View logs with timestamps
```
$ docker logs -t mycontainer
```

### 407. View last 100 log lines
```
$ docker logs --tail 100 mycontainer
```

### 408. Configure JSON file logging driver
```
$ docker run --log-driver json-file --log-opt max-size=10m --log-opt max-file=3 nginx
```

### 409. Configure syslog logging driver
```
$ docker run --log-driver syslog --log-opt syslog-address=tcp://192.168.1.100:514 nginx
```

### 410. Configure journald logging driver
```
$ docker run --log-driver journald nginx
```

### 411. Configure gelf logging driver
```
$ docker run --log-driver gelf --log-opt gelf-address=udp://192.168.1.100:12201 nginx
```

### 412. Configure fluentd logging driver
```
$ docker run --log-driver fluentd --log-opt fluentd-address=192.168.1.100:24224 nginx
```

### 413. Disable logging
```
$ docker run --log-driver none nginx
```

### 414. View daemon logs (Ubuntu/Debian)
```
$ journalctl -u docker.service
```

### 415. View daemon logs (CentOS/RHEL)
```
$ sudo cat /var/log/messages | grep docker
```

### 416. Monitor Docker events
```
$ docker events
```

### 417. Filter events by type
```
$ docker events --filter type=container
```

### 418. Monitor specific container events
```
$ docker events --filter container=mycontainer
```

### 419. View system-wide information
```
$ docker system info
```

### 420. Check Docker daemon health
```
$ docker system events --filter type=daemon
```

---

## 13. SECURITY & BEST PRACTICES

### 421. Run container as non-root user
```
$ docker run --user 1000:1000 nginx
```

### 422. Run container with read-only filesystem
```
$ docker run --read-only nginx
```

### 423. Run container with no new privileges
```
$ docker run --security-opt=no-new-privileges nginx
```

### 424. Drop all capabilities and add specific ones
```
$ docker run --cap-drop=ALL --cap-add=NET_BIND_SERVICE nginx
```

### 425. Run container with AppArmor profile
```
$ docker run --security-opt apparmor=docker-default nginx
```

### 426. Run container with SELinux label
```
$ docker run --security-opt label=level:s0:c100,c200 nginx
```

### 427. Run container with seccomp profile
```
$ docker run --security-opt seccomp=/path/to/profile.json nginx
```

### 428. Limit container PIDs
```
$ docker run --pids-limit 100 nginx
```

### 429. Set container ulimits
```
$ docker run --ulimit nofile=1024:2048 nginx
```

### 430. Scan image for vulnerabilities (using Trivy)
```
$ docker run aquasec/trivy image nginx:latest
```

### 431. Enable Docker Content Trust
```
$ export DOCKER_CONTENT_TRUST=1
```

### 432. Sign image
```
$ docker trust sign myimage:tag
```

### 433. View image signatures
```
$ docker trust inspect --pretty myimage:tag
```

### 434. Create user namespace
```
$ docker run --userns-remap=default nginx
```

### 435. Limit container memory
```
$ docker run -m 512m nginx
```

### 436. Set memory reservation
```
$ docker run --memory-reservation 256m nginx
```

### 437. Limit container swap
```
$ docker run --memory-swap 1g nginx
```

### 438. Set CPU shares
```
$ docker run --cpu-shares 512 nginx
```

### 439. Limit CPU cores
```
$ docker run --cpus 1.5 nginx
```

### 440. Set CPU quota and period
```
$ docker run --cpu-quota 50000 --cpu-period 100000 nginx
```

### 441. Set block IO weight
```
$ docker run --blkio-weight 500 nginx
```

### 442. Limit device read rate
```
$ docker run --device-read-bps /dev/sda:1mb nginx
```

### 443. Limit device write rate
```
$ docker run --device-write-bps /dev/sda:1mb nginx
```

### 444. Add Linux capabilities
```
$ docker run --cap-add SYS_ADMIN nginx
```

### 445. Drop Linux capabilities
```
$ docker run --cap-drop CHOWN nginx
```

---

## 14. TROUBLESHOOTING

### 446. Check Docker daemon status
```
$ sudo systemctl status docker
```

### 447. Restart Docker daemon
```
$ sudo systemctl restart docker
```

### 448. Enable debug mode
```
$ dockerd --debug
```

### 449. View Docker daemon configuration
```
$ docker info --format '{{json .}}'
```

### 450. Check container exit code
```
$ docker inspect --format='{{.State.ExitCode}}' mycontainer
```

### 451. View container error logs
```
$ docker logs --tail 50 mycontainer
```

### 452. Inspect container filesystem changes
```
$ docker diff mycontainer
```

### 453. Export container filesystem
```
$ docker export mycontainer -o debug.tar
```

### 454. Check container network connectivity
```
$ docker exec mycontainer ping -c 3 google.com
```

### 455. View container processes
```
$ docker top mycontainer aux
```

### 456. Check container resource usage
```
$ docker stats --no-stream mycontainer
```

### 457. Inspect container network settings
```
$ docker inspect --format='{{.NetworkSettings.IPAddress}}' mycontainer
```

### 458. Test container port accessibility
```
$ docker exec mycontainer netstat -tulpn
```

### 459. View container environment variables
```
$ docker exec mycontainer env
```

### 460. Check container DNS settings
```
$ docker exec mycontainer cat /etc/resolv.conf
```

### 461. View container hosts file
```
$ docker exec mycontainer cat /etc/hosts
```

### 462. Check disk space usage
```
$ docker system df -v
```

### 463. Clean up stopped containers
```
$ docker container prune -f
```

### 464. Remove dangling images
```
$ docker image prune -f
```

### 465. Remove unused volumes
```
$ docker volume prune -f
```

### 466. Remove unused networks
```
$ docker network prune -f
```

### 467. Comprehensive cleanup
```
$ docker system prune -a --volumes -f
```

### 468. Inspect Docker daemon logs
```
$ sudo journalctl -fu docker.service
```

### 469. Check image layers
```
$ docker history --no-trunc myimage
```

### 470. Verify image integrity
```
$ docker pull --disable-content-trust=false myimage
```

### 471. Debug container startup
```
$ docker run -it --entrypoint /bin/sh myimage
```

### 472. Override container command
```
$ docker run -it myimage /bin/bash
```

### 473. Check container mounts
```
$ docker inspect --format='{{.Mounts}}' mycontainer
```

### 474. View container labels
```
$ docker inspect --format='{{.Config.Labels}}' mycontainer
```

### 475. Check container restart count
```
$ docker inspect --format='{{.RestartCount}}' mycontainer
```

### 476. Validate Dockerfile syntax
```
$ docker build --no-cache -t test . 2>&1 | grep -i error
```

### 477. Test network connectivity between containers
```
$ docker exec container1 ping container2
```

### 478. Trace Docker API calls
```
$ docker --log-level=debug run nginx
```

### 479. Export all container logs
```
$ docker ps -aq | xargs -I {} docker logs {} > all_logs.txt
```

### 480. Check Docker storage driver
```
$ docker info | grep "Storage Driver"
```

### 481. Verify Docker daemon is running
```
$ ps aux | grep dockerd
```

### 482. Check Docker socket
```
$ ls -la /var/run/docker.sock
```

### 483. Test Docker socket connectivity
```
$ curl --unix-socket /var/run/docker.sock http://localhost/info
```

### 484. Force remove all containers
```
$ docker rm -f $(docker ps -aq)
```

### 485. Inspect container entrypoint
```
$ docker inspect --format='{{.Config.Entrypoint}}' mycontainer
```

### 486. Check container command
```
$ docker inspect --format='{{.Config.Cmd}}' mycontainer
```

### 487. View container creation time
```
$ docker inspect --format='{{.Created}}' mycontainer
```

### 488. Check container state
```
$ docker inspect --format='{{json .State}}' mycontainer | jq
```

### 489. List all container networks
```
$ docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' mycontainer
```

### 490. Debug network issues
```
$ docker network inspect bridge
```

### 491. Check container DNS resolution
```
$ docker exec mycontainer nslookup google.com
```

### 492. Test container internet connectivity
```
$ docker exec mycontainer curl -I https://google.com
```

### 493. Check container timezone
```
$ docker exec mycontainer date
```

### 494. View container user
```
$ docker exec mycontainer whoami
```

### 495. Check container working directory
```
$ docker exec mycontainer pwd
```

### 496. List container filesystem
```
$ docker exec mycontainer ls -la /
```

### 497. Check container permissions
```
$ docker exec mycontainer ls -l /var/log
```

### 498. Verify volume mounts
```
$ docker exec mycontainer df -h
```

### 499. Test container write permissions
```
$ docker exec mycontainer touch /tmp/test.txt
```

### 500. Final system check
```
$ docker version && docker info && docker ps -a
```

---

## BONUS COMMANDS

### 501. Create lightweight Alpine container
```
$ docker run -it alpine sh
```

### 502. Run container with init system
```
$ docker run --init nginx
```

### 503. Set container OOM score
```
$ docker run --oom-score-adj 500 nginx
```

### 504. Attach to container stdin only
```
$ docker attach --no-stdin mycontainer
```

### 505. Run container with custom DNS
```
$ docker run --dns 1.1.1.1 --dns 8.8.8.8 nginx
```

### 506. Set container MAC address
```
$ docker run --mac-address 02:42:ac:11:00:02 nginx
```

### 507. Run container in specific cgroup
```
$ docker run --cgroup-parent=/custom/path nginx
```

### 508. Set container IPC namespace
```
$ docker run --ipc=host nginx
```

### 509. Share PID namespace with host
```
$ docker run --pid=host nginx
```

### 510. Mount host device in container
```
$ docker run --device=/dev/sda:/dev/xvda nginx
```

---

## TOPICS WE COVERED

This comprehensive Docker course covers:
- 510+ commands from beginner to advanced
- Installation and setup
- Container lifecycle management
- Image management
- Volume and network operations
- Dockerfile best practices
- Docker Compose orchestration
- Docker Swarm clustering
- Security hardening
- Monitoring and logging
- Troubleshooting techniques

--- 
[JOIN FOR LIVE CLASSES AND SUPPORT](https://t.me/efxtv) 

[![Buy Me A Coffee](https://img.buymeacoffee.com/button-api/?text=Buy%20me%20a%20coffee&emoji=&slug=efxtv&button_colour=FFDD00&font_colour=000000&font_family=Cookie&outline_colour=000000&coffee_colour=ffffff)](https://buymeacoffee.com/efxtv)

