# COMPLETE DOCKER COURSE - PART 3 (EXPERT TO ARCHITECT LEVEL)

## TABLE OF CONTENTS
1. Docker Enterprise & DTR
2. Advanced Swarm Operations
3. Container Networking Deep Dive
4. Custom Docker Plugins Development
5. Docker Registry Operations
6. Advanced BuildKit Features
7. Container Security Hardening
8. Observability & Telemetry
9. Disaster Recovery & Business Continuity
10. Multi-Cloud & Hybrid Deployments
11. GitOps with Docker
12. Service Mesh Integration
13. Serverless Containers
14. Edge Computing & IoT
15. Advanced Troubleshooting & Debugging

---

## 1. DOCKER ENTERPRISE & DTR

### 1001. Install Docker Trusted Registry (DTR)
```
$ docker run -it --rm docker/dtr install --ucp-node <node-hostname> --ucp-insecure-tls
```

### 1002. Configure DTR with external certificates
```
$ docker run -it --rm docker/dtr reconfigure --dtr-external-url dtr.example.com --ucp-insecure-tls
```

### 1003. Enable DTR image scanning
```
$ curl -u admin:password https://dtr.example.com/api/v0/imagescan/scansummary/repositories/myrepo/myimage
```

### 1004. Configure DTR garbage collection
```
$ docker run -it --rm docker/dtr reconfigure --enable-gc
```

### 1005. Set DTR storage backend to S3
```
$ docker run -it --rm docker/dtr reconfigure --storage-backend s3 --s3-bucket dtr-images --s3-region us-east-1
```

### 1006. Configure DTR with Azure Blob Storage
```
$ docker run -it --rm docker/dtr reconfigure --storage-backend azure --azure-account-name myaccount --azure-account-key mykey
```

### 1007. Enable DTR content trust
```
$ docker run -it --rm docker/dtr reconfigure --enable-content-trust
```

### 1008. Configure DTR with LDAP authentication
```
$ docker run -it --rm docker/dtr reconfigure --enable-ldap --ldap-server ldap.example.com --ldap-reader-dn cn=admin
```

### 1009. Set up DTR high availability
```
$ docker run -it --rm docker/dtr join --ucp-node node2 --existing-replica-id <replica-id>
```

### 1010. Backup DTR metadata
```
$ docker run -it --rm docker/dtr backup --ucp-insecure-tls > dtr-backup.tar
```

### 1011. Restore DTR from backup
```
$ docker run -it --rm docker/dtr restore --ucp-insecure-tls < dtr-backup.tar
```

### 1012. Configure DTR webhooks
```
$ curl -X POST https://dtr.example.com/api/v0/repositories/myrepo/webhooks \
  -H "Content-Type: application/json" \
  -d '{"url":"https://webhook.site/unique-id","type":"TAG_PUSH"}'
```

### 1013. Enable DTR vulnerability scanning
```
$ curl -X POST https://dtr.example.com/api/v0/repositories/myrepo/myimage/scan
```

### 1014. Get DTR scan results
```
$ curl https://dtr.example.com/api/v0/imagescan/repositories/myrepo/myimage/tag/latest
```

### 1015. Configure DTR promotion policies
```
$ curl -X POST https://dtr.example.com/api/v0/repositories/myrepo/promotionPolicies \
  -d '{"enabled":true,"rules":[{"field":"vulnerabilities","operator":"<","value":"5"}]}'
```

### 1016. Set up DTR mirroring
```
$ curl -X POST https://dtr.example.com/api/v0/repositories/myrepo/mirrors \
  -d '{"remoteURL":"https://mirror.example.com","enabled":true}'
```

### 1017. Configure DTR rate limiting
```
$ docker run -it --rm docker/dtr reconfigure --rate-limit 100
```

### 1018. Enable DTR audit logging
```
$ docker run -it --rm docker/dtr reconfigure --enable-audit-log
```

### 1019. Configure DTR with custom CA
```
$ docker run -it --rm docker/dtr reconfigure --ucp-ca "$(cat ca.pem)"
```

### 1020. Monitor DTR health
```
$ curl https://dtr.example.com/health
```

### 1021. Get DTR system info
```
$ curl https://dtr.example.com/api/v0/meta/settings
```

### 1022. Configure DTR log driver
```
$ docker run -it --rm docker/dtr reconfigure --log-driver json-file --log-opts max-size=10m
```

### 1023. Set DTR replica count
```
$ docker run -it --rm docker/dtr reconfigure --replica-count 3
```

### 1024. Enable DTR online garbage collection
```
$ curl -X POST https://dtr.example.com/api/v0/admin/settings/gc
```

### 1025. Configure DTR job queue
```
$ docker run -it --rm docker/dtr reconfigure --job-queue-size 100
```

---

## 2. ADVANCED SWARM OPERATIONS

### 1026. Initialize Swarm with custom address pool
```
$ docker swarm init --default-addr-pool 10.20.0.0/16 --default-addr-pool-mask-length 24
```

### 1027. Configure Swarm certificate rotation
```
$ docker swarm update --cert-expiry 720h
```

### 1028. Set Swarm dispatcher heartbeat
```
$ docker swarm update --dispatcher-heartbeat 10s
```

### 1029. Configure Swarm task history retention
```
$ docker swarm update --task-history-limit 10
```

### 1030. Enable Swarm autolock
```
$ docker swarm update --autolock=true
```

### 1031. Get Swarm unlock key after autolock
```
$ docker swarm unlock-key
```

### 1032. Rotate Swarm unlock key
```
$ docker swarm unlock-key --rotate
```

### 1033. Configure Swarm snapshot interval
```
$ docker swarm update --snapshot-interval 10000
```

### 1034. Set Swarm max snapshots
```
$ docker swarm update --max-snapshots 5
```

### 1035. Force new cluster from single manager
```
$ docker swarm init --force-new-cluster
```

### 1036. Configure node availability drain
```
$ docker node update --availability drain node1
```

### 1037. Set node as active
```
$ docker node update --availability active node1
```

### 1038. Set node as pause
```
$ docker node update --availability pause node1
```

### 1039. Add node label for placement
```
$ docker node update --label-add disk=ssd node1
```

### 1040. Remove node label
```
$ docker node update --label-rm disk node1
```

### 1041. Set node role to manager
```
$ docker node promote node1
```

### 1042. Set node role to worker
```
$ docker node demote node1
```

### 1043. Filter nodes by label
```
$ docker node ls --filter "node.label=disk=ssd"
```

### 1044. Inspect node with format
```
$ docker node inspect --format '{{.Status.State}}' node1
```

### 1045. Create global service
```
$ docker service create --mode global --name monitoring prometheus
```

### 1046. Create replicated service with specific count
```
$ docker service create --replicas 5 --name web nginx
```

### 1047. Configure service with placement constraints
```
$ docker service create --constraint 'node.labels.disk==ssd' --name db postgres
```

### 1048. Set service placement preference
```
$ docker service create --placement-pref 'spread=node.labels.zone' --name web nginx
```

### 1049. Configure service with multiple constraints
```
$ docker service create --constraint 'node.role==worker' --constraint 'node.labels.type==web' nginx
```

### 1050. Set service update parallelism
```
$ docker service update --update-parallelism 2 web
```

### 1051. Configure service update delay
```
$ docker service update --update-delay 30s web
```

### 1052. Set service update failure action
```
$ docker service update --update-failure-action rollback web
```

### 1053. Configure service rollback
```
$ docker service update --rollback web
```

### 1054. Set rollback parallelism
```
$ docker service update --rollback-parallelism 1 web
```

### 1055. Configure rollback delay
```
$ docker service update --rollback-delay 10s web
```

### 1056. Set service restart condition
```
$ docker service update --restart-condition on-failure web
```

### 1057. Configure restart delay
```
$ docker service update --restart-delay 5s web
```

### 1058. Set restart max attempts
```
$ docker service update --restart-max-attempts 3 web
```

### 1059. Configure restart window
```
$ docker service update --restart-window 120s web
```

### 1060. Set service stop grace period
```
$ docker service update --stop-grace-period 30s web
```

### 1061. Configure service resource limits
```
$ docker service update --limit-cpu 0.5 --limit-memory 512M web
```

### 1062. Set service resource reservations
```
$ docker service update --reserve-cpu 0.25 --reserve-memory 256M web
```

### 1063. Add service network
```
$ docker service update --network-add mynetwork web
```

### 1064. Remove service network
```
$ docker service update --network-rm oldnetwork web
```

### 1065. Publish service port in host mode
```
$ docker service create --publish mode=host,target=80,published=8080 nginx
```

### 1066. Configure service with endpoint mode vip
```
$ docker service create --endpoint-mode vip --name web nginx
```

### 1067. Set service endpoint mode to dnsrr
```
$ docker service create --endpoint-mode dnsrr --name web nginx
```

### 1068. Add service secret
```
$ docker service update --secret-add db_password web
```

### 1069. Remove service secret
```
$ docker service update --secret-rm old_password web
```

### 1070. Add service config
```
$ docker service update --config-add source=app_config,target=/etc/config.json web
```

### 1071. Remove service config
```
$ docker service update --config-rm app_config web
```

### 1072. Set service DNS options
```
$ docker service create --dns 8.8.8.8 --dns-search example.com nginx
```

### 1073. Configure service hostname
```
$ docker service create --hostname webserver nginx
```

### 1074. Set service with specific container labels
```
$ docker service create --container-label env=production nginx
```

### 1075. Force service update (refresh)
```
$ docker service update --force web
```

### 1076. Scale service to zero
```
$ docker service scale web=0
```

### 1077. Get service logs with timestamps
```
$ docker service logs -t web
```

### 1078. Filter service logs by time
```
$ docker service logs --since 2024-01-01T00:00:00 web
```

### 1079. Get raw service logs (no formatting)
```
$ docker service logs --raw web
```

### 1080. Inspect service with pretty print
```
$ docker service inspect --pretty web
```

### 1081. Get service virtual IP
```
$ docker service inspect --format='{{.Endpoint.VirtualIPs}}' web
```

### 1082. List service tasks with filters
```
$ docker service ps --filter "desired-state=running" web
```

### 1083. Show only running tasks
```
$ docker service ps --filter "desired-state=running" --format "table {{.ID}}\t{{.Node}}" web
```

### 1084. Create service with TTY
```
$ docker service create --tty --name interactive alpine sh
```

### 1085. Configure service with init process
```
$ docker service create --init --name web nginx
```

### 1086. Set service isolation mode (Windows)
```
$ docker service create --isolation process --name web mcr.microsoft.com/windows/nanoserver
```

### 1087. Configure service with read-only root filesystem
```
$ docker service create --read-only --name web nginx
```

### 1088. Set service with generic resources
```
$ docker service create --generic-resource "NVIDIA-GPU=2" --name ml-app tensorflow
```

### 1089. Configure service max replicas per node
```
$ docker service create --replicas-max-per-node 2 --name web nginx
```

### 1090. Set service rollback order
```
$ docker service create --rollback-order start-first --name web nginx
```

---

## 3. CONTAINER NETWORKING DEEP DIVE

### 1091. Create network with custom bridge options
```
$ docker network create -o "com.docker.network.bridge.name=br-custom" \
  -o "com.docker.network.bridge.enable_icc=true" \
  -o "com.docker.network.bridge.enable_ip_masquerade=true" custom-bridge
```

### 1092. Configure network with hairpin mode
```
$ docker network create -o "com.docker.network.bridge.enable_hairpin_mode=true" hairpin-net
```

### 1093. Set network bridge MTU
```
$ docker network create -o "com.docker.network.driver.mtu=9000" jumbo-net
```

### 1094. Create network with IPv4 and IPv6
```
$ docker network create --ipv6 --subnet=2001:db8:1::/64 --gateway=2001:db8:1::1 \
  --subnet=172.28.0.0/16 --gateway=172.28.0.1 dual-stack
```

### 1095. Configure network with custom DNS
```
$ docker network create --dns 1.1.1.1 --dns 8.8.8.8 custom-dns-net
```

### 1096. Set network DNS search domains
```
$ docker network create --dns-search example.com --dns-search test.local search-net
```

### 1097. Create network with aux addresses
```
$ docker network create --aux-address "gateway=192.168.1.1" \
  --aux-address "dns=192.168.1.2" --subnet 192.168.1.0/24 aux-net
```

### 1098. Configure overlay network with encryption
```
$ docker network create --driver overlay --opt encrypted=true secure-overlay
```

### 1099. Set overlay network VXLAN ID
```
$ docker network create --driver overlay --opt com.docker.network.driver.overlay.vxlanid_list=4096 custom-vxlan
```

### 1100. Create macvlan network on specific interface
```
$ docker network create -d macvlan --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 -o parent=eth0 macvlan-net
```

### 1101. Configure macvlan with 802.1Q trunk
```
$ docker network create -d macvlan --subnet=192.168.1.0/24 \
  -o parent=eth0.100 macvlan-vlan100
```

### 1102. Create ipvlan L3 mode network
```
$ docker network create -d ipvlan --subnet=192.168.1.0/24 \
  -o parent=eth0 -o ipvlan_mode=l3 ipvlan-l3
```

### 1103. Configure network isolation with internal flag
```
$ docker network create --internal --subnet=10.0.1.0/24 isolated-net
```

### 1104. Set network attachable for standalone containers
```
$ docker network create --driver overlay --attachable swarm-attachable
```

### 1105. Create network with ingress mode
```
$ docker network create --driver overlay --ingress --subnet=10.11.0.0/16 custom-ingress
```

### 1106. Configure network scope
```
$ docker network create --scope swarm --driver overlay swarm-scoped
```

### 1107. Inspect network containers
```
$ docker network inspect --format='{{range .Containers}}{{.Name}} {{end}}' bridge
```

### 1108. Get network subnet
```
$ docker network inspect --format='{{range .IPAM.Config}}{{.Subnet}}{{end}}' mynetwork
```

### 1109. List network driver options
```
$ docker network inspect --format='{{json .Options}}' mynetwork | jq
```

### 1110. Connect container with custom IP and MAC
```
$ docker network connect --ip 172.18.0.10 --mac-address 02:42:ac:11:00:02 mynetwork mycontainer
```

### 1111. Connect container with alias
```
$ docker network connect --alias db --alias database mynetwork mycontainer
```

### 1112. Connect container with link-local IP
```
$ docker network connect --link-local-ip 169.254.1.1 mynetwork mycontainer
```

### 1113. Disconnect container with force
```
$ docker network disconnect -f mynetwork mycontainer
```

### 1114. Create custom IPAM driver network
```
$ docker network create --ipam-driver custom-ipam --subnet=10.0.0.0/8 custom-ipam-net
```

### 1115. Configure network with IPAM options
```
$ docker network create --subnet=192.168.1.0/24 \
  --ipam-opt com.docker.network.ipam.serial=true ipam-serial
```

### 1116. Set network IP range for containers
```
$ docker network create --subnet=10.10.0.0/16 --ip-range=10.10.1.0/24 range-net
```

### 1117. Inspect network with verbose output
```
$ docker network inspect -v mynetwork
```

### 1118. Filter networks by driver
```
$ docker network ls --filter driver=overlay
```

### 1119. Filter networks by label
```
$ docker network ls --filter label=env=production
```

### 1120. List networks with custom format
```
$ docker network ls --format "table {{.ID}}\t{{.Name}}\t{{.Driver}}\t{{.Scope}}"
```

### 1121. Prune networks with filter
```
$ docker network prune --filter "until=24h"
```

### 1122. Create network with labels
```
$ docker network create --label environment=production --label team=backend prod-net
```

### 1123. Test network connectivity between containers
```
$ docker run --rm --network container:container1 nicolaka/netshoot ping container2
```

### 1124. Diagnose network with netshoot
```
$ docker run -it --rm --network host nicolaka/netshoot
```

### 1125. Capture network traffic with tcpdump
```
$ docker run --rm --net=host --cap-add=NET_ADMIN nicolaka/netshoot tcpdump -i eth0
```

---

## 4. CUSTOM DOCKER PLUGINS DEVELOPMENT

### 1126. Create plugin directory structure
```
$ mkdir -p my-plugin/rootfs
$ mkdir -p my-plugin/config.json
```

### 1127. Create plugin config.json
```json
{
  "description": "My custom plugin",
  "documentation": "https://docs.example.com",
  "interface": {
    "types": ["docker.volumedriver/1.0"],
    "socket": "my-plugin.sock"
  },
  "entrypoint": ["/usr/bin/my-plugin"],
  "network": {
    "type": "host"
  },
  "mounts": [
    {
      "source": "/var/lib/my-plugin",
      "destination": "/data",
      "type": "bind"
    }
  ],
  "env": [
    {
      "name": "DEBUG",
      "value": "0"
    }
  ]
}
```

### 1128. Build plugin rootfs with Docker
```
$ docker build -t rootfs .
$ docker create --name tmp rootfs
$ docker export tmp | tar -x -C my-plugin/rootfs
$ docker rm tmp
```

### 1129. Create plugin from rootfs
```
$ docker plugin create my-plugin ./my-plugin
```

### 1130. Enable plugin
```
$ docker plugin enable my-plugin
```

### 1131. Test volume plugin
```
$ docker volume create -d my-plugin test-volume
```

### 1132. Develop network plugin interface
```go
type NetworkDriver interface {
    CreateNetwork(*CreateNetworkRequest) error
    DeleteNetwork(*DeleteNetworkRequest) error
    CreateEndpoint(*CreateEndpointRequest) (*CreateEndpointResponse, error)
    DeleteEndpoint(*DeleteEndpointRequest) error
    Join(*JoinRequest) (*JoinResponse, error)
    Leave(*LeaveRequest) error
}
```

### 1133. Implement volume plugin interface
```go
type VolumeDriver interface {
    Create(*CreateRequest) error
    Remove(*RemoveRequest) error
    Mount(*MountRequest) (*MountResponse, error)
    Unmount(*UnmountRequest) error
    List() (*ListResponse, error)
    Get(*GetRequest) (*GetResponse, error)
}
```

### 1134. Create authorization plugin
```go
type AuthZPlugin interface {
    AuthZReq(*Request) *Response
    AuthZRes(*Request) *Response
}
```

### 1135. Implement IPAM plugin
```go
type IpamDriver interface {
    GetDefaultAddressSpaces() (*AddressSpacesResponse, error)
    RequestPool(*RequestPoolRequest) (*RequestPoolResponse, error)
    ReleasePool(*ReleasePoolRequest) error
    RequestAddress(*RequestAddressRequest) (*RequestAddressResponse, error)
    ReleaseAddress(*ReleaseAddressRequest) error
}
```

### 1136. Create logging plugin
```go
type LogDriver interface {
    StartLogging(file string, info logger.Info) error
    StopLogging(file string) error
    ReadLogs(info logger.Info, config logger.ReadConfig) (io.ReadCloser, error)
}
```

### 1137. Debug plugin with logs
```
$ docker plugin inspect my-plugin --format='{{.Settings.Env}}'
```

### 1138. Set plugin environment variables
```
$ docker plugin set my-plugin DEBUG=1
```

### 1139. Configure plugin mounts
```
$ docker plugin set my-plugin MOUNT_SOURCE=/custom/path
```

### 1140. Upgrade plugin
```
$ docker plugin upgrade my-plugin my-plugin:v2
```

### 1141. Export plugin
```
$ docker plugin create my-plugin-export my-plugin
```

### 1142. Push plugin to registry
```
$ docker plugin push myregistry.com/my-plugin:latest
```

### 1143. Create plugin with capabilities
```json
{
  "linux": {
    "capabilities": ["CAP_SYS_ADMIN", "CAP_NET_ADMIN"]
  }
}
```

### 1144. Configure plugin devices
```json
{
  "linux": {
    "devices": [
      {
        "path": "/dev/fuse"
      }
    ]
  }
}
```

### 1145. Set plugin propagated mount
```json
{
  "mounts": [
    {
      "source": "/var/lib/docker/plugins",
      "destination": "/mnt/state",
      "type": "bind",
      "options": ["rbind", "rshared"]
    }
  ]
}
```

---

## 5. DOCKER REGISTRY OPERATIONS

### 1146. Run registry with Redis cache
```
$ docker run -d -p 5000:5000 --name registry \
  -e REGISTRY_STORAGE_CACHE_BLOBDESCRIPTOR=redis \
  -e REGISTRY_REDIS_ADDR=redis:6379 \
  registry:2
```

### 1147. Configure registry with filesystem storage
```yaml
storage:
  filesystem:
    rootdirectory: /var/lib/registry
```

### 1148. Set up registry with S3 backend
```yaml
storage:
  s3:
    accesskey: AKIAIOSFODNN7EXAMPLE
    secretkey: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
    region: us-east-1
    bucket: my-registry-bucket
    encrypt: true
    secure: true
```

### 1149. Configure registry with Azure storage
```yaml
storage:
  azure:
    accountname: myaccount
    accountkey: mykey
    container: registry-container
```

### 1150. Set up registry with GCS backend
```yaml
storage:
  gcs:
    bucket: my-gcs-bucket
    keyfile: /path/to/keyfile.json
```

### 1151. Enable registry delete operation
```yaml
storage:
  delete:
    enabled: true
```

### 1152. Configure registry with maintenance mode
```yaml
storage:
  maintenance:
    readonly:
      enabled: true
```

### 1153. Set registry upload purge settings
```yaml
storage:
  maintenance:
    uploadpurging:
      enabled: true
      age: 168h
      interval: 24h
      dryrun: false
```

### 1154. Configure registry HTTP settings
```yaml
http:
  addr: :5000
  secret: mysecretkey
  relativeurls: false
  draintimeout: 60s
```

### 1155. Enable registry TLS
```yaml
http:
  tls:
    certificate: /path/to/cert.pem
    key: /path/to/key.pem
```

### 1156. Configure registry authentication
```yaml
auth:
  htpasswd:
    realm: basic-realm
    path: /auth/htpasswd
```

### 1157. Set up registry with token auth
```yaml
auth:
  token:
    realm: https://auth.example.com/token
    service: registry.example.com
    issuer: auth-service
    rootcertbundle: /path/to/root.crt
```

### 1158. Configure registry middleware
```yaml
middleware:
  storage:
    - name: cloudfront
      options:
        baseurl: https://d111111abcdef8.cloudfront.net
        privatekey: /path/to/private.pem
        keypairid: APKAEXAMPLE
```

### 1159. Enable registry notifications
```yaml
notifications:
  endpoints:
    - name: webhook
      url: https://webhook.example.com
      headers:
        Authorization: [Bearer token]
      timeout: 1s
      threshold: 5
      backoff: 1s
```

### 1160. Configure registry health check
```yaml
health:
  storagedriver:
    enabled: true
    interval: 10s
    threshold: 3
```

### 1161. Set up registry proxy
```yaml
proxy:
  remoteurl: https://registry-1.docker.io
  username: myuser
  password: mypassword
```

### 1162. Configure registry validation
```yaml
validation:
  manifests:
    urls:
      allow:
        - ^https?://([^/]+\.)?example\.com/
      deny:
        - ^https?://www\.example\.com/
```

### 1163. Enable registry debug mode
```yaml
log:
  level: debug
  formatter: text
  fields:
    service: registry
    environment: production
```

### 1164. Configure registry log hooks
```yaml
log:
  hooks:
    - type: mail
      disabled: false
      levels:
        - panic
      options:
        smtp:
          addr: smtp.example.com:25
          username: mailuser
          password: mailpass
          from: registry@example.com
          to:
            - admin@example.com
```

### 1165. List catalog from registry
```
$ curl -X GET http://localhost:5000/v2/_catalog
```

### 1166. List tags for repository
```
$ curl -X GET http://localhost:5000/v2/myrepo/tags/list
```

### 1167. Get image manifest
```
$ curl -X GET http://localhost:5000/v2/myrepo/manifests/latest
```

### 1168. Delete image manifest
```
$ curl -X DELETE http://localhost:5000/v2/myrepo/manifests/sha256:abc123...
```

### 1169. Get blob from registry
```
$ curl -X GET http://localhost:5000/v2/myrepo/blobs/sha256:abc123...
```

### 1170. Check blob existence
```
$ curl -I http://localhost:5000/v2/myrepo/blobs/sha256:abc123...
```

### 1171. Run garbage collection
```
$ docker exec registry bin/registry garbage-collect /etc/docker/registry/config.yml
```

### 1172. Run GC with dry-run
```
$ docker exec registry bin/registry garbage-collect --dry-run /etc/docker/registry/config.yml
```

### 1173. Generate htpasswd for registry
```
$ docker run --entrypoint htpasswd httpd:2 -Bbn username password > htpasswd
```

### 1174. Configure registry rate limiting
```yaml
middleware:
  registry:
    - name: ratelimit
      options:
        interval: 1m
        requests: 100
```

### 1175. Set up registry mirroring
```
$ docker run -d -p 5000:5000 --name mirror \
  -e REGISTRY_PROXY_REMOTEURL=https://registry-1.docker.io \
  registry:2
```

---

## 6. ADVANCED BUILDKIT FEATURES

### 1176. Enable BuildKit inline cache export
```
$ docker buildx build --cache-to type=inline --tag myimage .
```

### 1177. Use BuildKit registry cache
```
$ docker buildx build \
  --cache-from type=registry,ref=myregistry/cache \
  --cache-to type=registry,ref=myregistry/cache,mode=max \
  --tag myimage .
```

### 1178. Configure BuildKit local cache
```
$ docker buildx build \
  --cache-from type=local,src=/tmp/cache \
  --cache-to type=local,dest=/tmp/cache \
  --tag myimage .
```

### 1179. Use BuildKit gha cache (GitHub Actions)
```
$ docker buildx build \
  --cache-from type=gha \
  --cache-to type=gha,mode=max \
  --tag myimage .
```

### 1180. Build with BuildKit S3 cache
```
$ docker buildx build \
  --cache-from type=s3,region=us-east-1,bucket=mybucket,name=mycache \
  --cache-to type=s3,region=us-east-1,bucket=mybucket,name=mycache,mode=max \
  --tag myimage .
```

### 1181. Use BuildKit secrets from environment
```
$ docker buildx build --secret id=token,env=GITHUB_TOKEN --tag myimage .
```

### 1182. Mount BuildKit secret to custom path
```
RUN --mount=type=secret,id=token,dst=/run/secrets/token \
  cat /run/secrets/token
```

### 1183. Use BuildKit SSH with custom key
```
$ docker buildx build --ssh default=$HOME/.ssh/custom_key --tag myimage .
```

### 1184. Configure SSH mount options
```
RUN --mount=type=ssh,id=default,mode=0600 \
  ssh-keyscan github.com >> ~/.ssh/known_hosts && \
  git clone git@github.com:user/repo.git
```

### 1185. Use BuildKit bind mount
```
RUN --mount=type=bind,source=.,target=/src \
  make -C /src build
```

### 1186. Configure bind mount with readonly
```
RUN --mount=type=bind,source=.,target=/src,readonly \
  cp /src/config.json /app/
```

### 1187. Use BuildKit cache mount
```
RUN --mount=type=cache,target=/root/.cache/pip \
  pip install -r requirements.txt
```

### 1188. Configure cache mount with sharing
```
RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
  apt-get update && apt-get install -y package
```

### 1189. Use cache mount with ID
```
RUN --mount=type=cache,id=myapp-cache,target=/cache \
  npm install
```

### 1190. Configure tmpfs mount
```
RUN --mount=type=tmpfs,target=/tmp \
  make test
```

### 1191. Use BuildKit heredoc syntax
```
RUN <<EOF
apt-get update
apt-get install -y curl
curl -sSL https://get.docker.com | sh
EOF
```

### 1192. Multi-line commands with heredoc
```
COPY <<EOF /app/config.json
{
  "server": "production",
  "debug": false
}
EOF
```

### 1193. Use BuildKit network mode none
```
RUN --network=none echo "No network access"
```

### 1194. Configure network mode host
```
RUN --network=host curl http://localhost:8080
```

### 1195. Use BuildKit security mode insecure
```
RUN --security=insecure curl -k https://self-signed.example.com
```

### 1196. Configure security mode sandbox
```
RUN --security=sandbox make build
```

### 1197. Use BuildKit image source
```
FROM scratch AS builder
COPY --from=golang:1.20 /usr/local/go /usr/local/go
```

### 1198. Export build output to local
```
$ docker buildx build --output type=local,dest=./output .
```

### 1199. Export build to tar
```
$ docker buildx build --output type=tar,dest=output.tar .
```

### 1200. Export OCI image
```
$ docker buildx build --output type=oci,dest=output.tar .
```

### 1201. Export Docker image
```
$ docker buildx build --output type=docker,dest=output.tar .
```

### 1202. Build with custom attestations
```
$ docker buildx build \
  --provenance=mode=max \
  --sbom=true \
  --tag myimage .
```

### 1203. Configure build attestation output
```
$ docker buildx build \
  --attest type=provenance,mode=max \
  --attest type=sbom \
  --tag myimage .
```

### 1204. Use BuildKit build args with defaults
```
ARG VERSION=latest
FROM ubuntu:${VERSION}
```

### 1205. Configure global build args
```
$ docker buildx build --build-arg BUILDKIT_MULTI_PLATFORM=1 --tag myimage .
```

### 1206. Use BuildKit image tools
```
$ docker buildx imagetools inspect myregistry/myimage:latest
```

### 1207. Create manifest with imagetools
```
$ docker buildx imagetools create -t myimage:latest \
  myimage:amd64 myimage:arm64
```

### 1208. Inspect build cache
```
$ docker buildx du --verbose
```

### 1209. Prune build cache by age
```
$ docker buildx prune --filter until=48h
```

### 1210. Configure BuildKit worker
```
$ docker buildx create \
  --name mybuilder \
  --driver docker-container \
  --driver-opt network=host \
  --use
```

### 1211. Set BuildKit worker platform
```
$ docker buildx create \
  --platform linux/amd64,linux/arm64 \
  --name multiarch \
  --use
```

### 1212. Configure BuildKit with config file
```toml
# buildkitd.toml
debug = true
[worker.oci]
  max-parallelism = 4
[worker.containerd]
  namespace = "buildkit"
```

### 1213. Use custom BuildKit config
```
$ docker buildx create \
  --config ./buildkitd.toml \
  --name custom-builder \
  --use
```

### 1214. Build with progress output
```
$ docker buildx build --progress=auto --tag myimage .
```

### 1215. Use plain progress output
```
$ docker buildx build --progress=plain --tag myimage .
```

---

## 7. CONTAINER SECURITY HARDENING

### 1216. Run container with user namespace
```
$ docker run --userns=host --user 1000:1000 nginx
```

### 1217. Configure cgroup limits
```
$ docker run --cgroup-parent=/custom/cgroup --memory=512m nginx
```

### 1218. Set container PID limit
```
$ docker run --pids-limit 200 nginx
```

### 1219. Configure device cgroup rules
```
$ docker run --device-cgroup-rule='c 1:3 rmw' nginx
```

### 1220. Set kernel memory limit
```
$ docker run --kernel-memory=50m nginx
```

### 1221. Configure OOM score adjustment
```
$ docker run --oom-score-adj=-500 nginx
```

### 1222. Disable OOM killer
```
$ docker run --oom-kill-disable --memory=512m nginx
```

### 1223. Set container CPU quota
```
$ docker run --cpu-quota=50000 --cpu-period=100000 nginx
```

### 1224. Configure CPU realtime period
```
$ docker run --cpu-rt-period=1000000 nginx
```

### 1225. Set CPU realtime runtime
```
$ docker run --cpu-rt-runtime=950000 nginx
```

### 1226. Configure cpuset CPUs
```
$ docker run --cpuset-cpus=0,1 nginx
```

### 1227. Set cpuset memory nodes
```
$ docker run --cpuset-mems=0,1 nginx
```

### 1228. Configure block I/O weight
```
$ docker run --blkio-weight=500 nginx
```

### 1229. Set device read BPS limit
```
$ docker run --device-read-bps /dev/sda:10mb nginx
```

### 1230. Configure device write BPS limit
```
$ docker run --device-write-bps /dev/sda:5mb nginx
```

### 1231. Set device read IOPS limit
```
$ docker run --device-read-iops /dev/sda:1000 nginx
```

### 1232. Configure device write IOPS limit
```
$ docker run --device-write-iops /dev/sda:500 nginx
```

### 1233. Set block I/O weight device
```
$ docker run --blkio-weight-device "/dev/sda:200" nginx
```

### 1234. Use landlock security module
```
$ docker run --security-opt="landlock=enabled" nginx
```

### 1235. Configure masked paths
```
$ docker run --security-opt="mask=/proc/kcore" nginx
```

### 1236. Set readonly paths
```
$ docker run --security-opt="ro=/sys" nginx
```

### 1237. Use custom seccomp profile with annotations
```json
{
  "defaultAction": "SCMP_ACT_ERRNO",
  "architectures": ["SCMP_ARCH_X86_64"],
  "syscalls": [
    {
      "names": ["accept", "accept4", "access"],
      "action": "SCMP_ACT_ALLOW"
    }
  ]
}
```

### 1238. Load seccomp profile
```
$ docker run --security-opt seccomp=./custom-seccomp.json nginx
```

### 1239. Audit seccomp actions
```json
{
  "defaultAction": "SCMP_ACT_LOG",
  "syscalls": [
    {
      "names": ["chmod", "chown"],
      "action": "SCMP_ACT_ERRNO"
    }
  ]
}
```

### 1240. Use AppArmor with custom profile
```
$ sudo apparmor_parser -r -W /etc/apparmor.d/docker-nginx
$ docker run --security-opt apparmor=docker-nginx nginx
```

### 1241. Create AppArmor profile for container
```
#include <tunables/global>

profile docker-nginx flags=(attach_disconnected,mediate_deleted) {
  #include <abstractions/base>
  
  network inet tcp,
  network inet udp,
  
  /usr/sbin/nginx ix,
  /var/log/nginx/** rw,
  /etc/nginx/** r,
}
```

### 1242. Use SELinux with custom type
```
$ docker run --security-opt label=type:svirt_apache_t nginx
```

### 1243. Set SELinux level
```
$ docker run --security-opt label=level:s0:c100,c200 nginx
```

### 1244. Configure SELinux user
```
$ docker run --security-opt label=user:system_u nginx
```

### 1245. Disable SELinux labeling
```
$ docker run --security-opt label=disable nginx
```

### 1246. Run rootless Docker daemon
```
$ dockerd-rootless-setuptool.sh install
$ systemctl --user enable docker
$ systemctl --user start docker
```

### 1247. Configure rootless Docker storage
```
$ export DOCKER_HOST=unix://$XDG_RUNTIME_DIR/docker.sock
```

### 1248. Use rootless mode with slirp4netns
```
$ dockerd-rootless.sh --experimental --storage-driver=overlay2
```

### 1249. Configure rootless port driver
```
$ dockerd-rootless.sh --experimental --port-driver=slirp4netns
```

### 1250. Run privileged container safely
```
$ docker run --privileged --security-opt apparmor=unconfined nginx
```

### 1251. Add specific Linux capability
```
$ docker run --cap-add=NET_ADMIN --cap-add=SYS_TIME nginx
```

### 1252. Drop all capabilities and add specific
```
$ docker run --cap-drop=ALL --cap-add=CHOWN --cap-add=DAC_OVERRIDE nginx
```

### 1253. List all Linux capabilities
```
$ docker run --rm alpine sh -c 'cat /proc/1/status | grep Cap'
```

### 1254. Check effective capabilities
```
$ docker exec mycontainer capsh --print
```

### 1255. Use Falco for runtime security
```
$ docker run -d --name falco --privileged \
  -v /var/run/docker.sock:/host/var/run/docker.sock \
  -v /dev:/host/dev \
  -v /proc:/host/proc:ro \
  falcosecurity/falco:latest
```

### 1256. Configure Falco rules
```yaml
- rule: Unauthorized Process
  desc: Detect unauthorized process execution
  condition: spawned_process and container and not proc.name in (allowed_processes)
  output: "Unauthorized process (user=%user.name command=%proc.cmdline)"
  priority: WARNING
```

### 1257. Enable Falco alerts
```
$ docker run -d --name falco \
  -e FALCO_BPF_PROBE="" \
  falcosecurity/falco:latest
```

### 1258. Scan running containers with Anchore
```
$ docker run -d --name anchore-engine anchore/engine:latest
$ anchore-cli system wait
$ anchore-cli image add myimage:latest
$ anchore-cli evaluate check myimage:latest
```

### 1259. Use Clair for vulnerability scanning
```
$ docker run -d --name clair-db postgres:latest
$ docker run -d --name clair --link clair-db:postgres quay.io/coreos/clair:latest
```

### 1260. Scan with Grype
```
$ docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
  anchore/grype:latest myimage:latest
```

---

## 8. OBSERVABILITY & TELEMETRY

### 1261. Export metrics to Prometheus
```
$ docker run -d -p 9090:9090 \
  -v ./prometheus.yml:/etc/prometheus/prometheus.yml \
  prom/prometheus
```

### 1262. Configure Prometheus scrape config
```yaml
scrape_configs:
  - job_name: 'docker'
    static_configs:
      - targets: ['localhost:9323']
```

### 1263. Enable Docker daemon metrics
```json
{
  "metrics-addr": "0.0.0.0:9323",
  "experimental": true
}
```

### 1264. Use cAdvisor for container metrics
```
$ docker run -d --name=cadvisor -p 8080:8080 \
  -v /:/rootfs:ro \
  -v /var/run:/var/run:ro \
  -v /sys:/sys:ro \
  -v /var/lib/docker/:/var/lib/docker:ro \
  gcr.io/cadvisor/cadvisor:latest
```

### 1265. Configure Grafana datasource
```
$ docker run -d --name=grafana -p 3000:3000 \
  -e GF_SECURITY_ADMIN_PASSWORD=admin \
  grafana/grafana
```

### 1266. Set up Loki for log aggregation
```
$ docker run -d --name=loki -p 3100:3100 grafana/loki:latest
```

### 1267. Configure Promtail for log shipping
```
$ docker run -d --name=promtail \
  -v /var/log:/var/log \
  -v ./promtail-config.yml:/etc/promtail/config.yml \
  grafana/promtail:latest
```

### 1268. Use Jaeger for distributed tracing
```
$ docker run -d --name jaeger \
  -p 5775:5775/udp \
  -p 6831:6831/udp \
  -p 6832:6832/udp \
  -p 5778:5778 \
  -p 16686:16686 \
  -p 14268:14268 \
  jaegertracing/all-in-one:latest
```

### 1269. Configure OpenTelemetry collector
```
$ docker run -d --name otel-collector \
  -p 4317:4317 \
  -v ./otel-config.yaml:/etc/otel/config.yaml \
  otel/opentelemetry-collector:latest
```

### 1270. Use Zipkin for tracing
```
$ docker run -d --name zipkin -p 9411:9411 openzipkin/zipkin
```

### 1271. Configure Fluentd for centralized logging
```
$ docker run -d --name fluentd -p 24224:24224 \
  -v ./fluent.conf:/fluentd/etc/fluent.conf \
  fluent/fluentd:latest
```

### 1272. Set up ELK stack - Elasticsearch
```
$ docker run -d --name elasticsearch \
  -p 9200:9200 -p 9300:9300 \
  -e "discovery.type=single-node" \
  elasticsearch:7.17.0
```

### 1273. Run Logstash
```
$ docker run -d --name logstash \
  -p 5000:5000 \
  -v ./logstash.conf:/usr/share/logstash/pipeline/logstash.conf \
  logstash:7.17.0
```

### 1274. Deploy Kibana
```
$ docker run -d --name kibana \
  -p 5601:5601 \
  -e "ELASTICSEARCH_HOSTS=http://elasticsearch:9200" \
  kibana:7.17.0
```

### 1275. Use Filebeat for log forwarding
```
$ docker run -d --name filebeat \
  -v ./filebeat.yml:/usr/share/filebeat/filebeat.yml \
  -v /var/lib/docker/containers:/var/lib/docker/containers:ro \
  elastic/filebeat:7.17.0
```

### 1276. Configure Metricbeat for metrics
```
$ docker run -d --name metricbeat \
  -v ./metricbeat.yml:/usr/share/metricbeat/metricbeat.yml \
  -v /var/run/docker.sock:/var/run/docker.sock \
  elastic/metricbeat:7.17.0
```

### 1277. Use APM server
```
$ docker run -d --name apm-server \
  -p 8200:8200 \
  elastic/apm-server:7.17.0
```

### 1278. Configure application with APM
```
$ docker run -d \
  -e ELASTIC_APM_SERVER_URL=http://apm-server:8200 \
  -e ELASTIC_APM_SERVICE_NAME=myapp \
  myapp:latest
```

### 1279. Use Node Exporter for host metrics
```
$ docker run -d --name node-exporter \
  -p 9100:9100 \
  -v /proc:/host/proc:ro \
  -v /sys:/host/sys:ro \
  -v /:/rootfs:ro \
  prom/node-exporter:latest
```

### 1280. Configure Alertmanager
```
$ docker run -d --name alertmanager \
  -p 9093:9093 \
  -v ./alertmanager.yml:/etc/alertmanager/alertmanager.yml \
  prom/alertmanager:latest
```

### 1281. Set up Thanos for Prometheus HA
```
$ docker run -d --name thanos-sidecar \
  -v ./prometheus-data:/prometheus \
  thanosio/thanos:latest sidecar \
  --prometheus.url=http://prometheus:9090
```

### 1282. Use Victoria Metrics
```
$ docker run -d --name victoria-metrics \
  -p 8428:8428 \
  -v victoria-metrics-data:/victoria-metrics-data \
  victoriametrics/victoria-metrics:latest
```

### 1283. Configure Grafana Mimir
```
$ docker run -d --name mimir \
  -p 9009:9009 \
  -v ./mimir.yaml:/etc/mimir/config.yaml \
  grafana/mimir:latest
```

### 1284. Use Tempo for distributed tracing
```
$ docker run -d --name tempo \
  -p 3200:3200 -p 4317:4317 \
  grafana/tempo:latest
```

### 1285. Configure custom metrics endpoint
```
$ docker run -d \
  -e METRICS_PORT=9090 \
  -e METRICS_PATH=/metrics \
  myapp:latest
```

---

## 9. DISASTER RECOVERY & BUSINESS CONTINUITY

### 1286. Create full system backup
```
$ docker run --rm -v /var/lib/docker:/docker -v $(pwd):/backup \
  ubuntu tar czf /backup/docker-full-backup-$(date +%Y%m%d).tar.gz /docker
```

### 1287. Backup Docker volumes
```
$ docker run --rm -v myvolume:/data -v $(pwd):/backup \
  ubuntu tar czf /backup/volume-backup-$(date +%Y%m%d).tar.gz /data
```

### 1288. Incremental volume backup with rsync
```
$ docker run --rm -v myvolume:/data -v /backup:/backup \
  instrumentisto/rsync rsync -av --link-dest=/backup/previous /data/ /backup/current/
```

### 1289. Backup container configuration
```
$ docker inspect mycontainer > container-config-backup.json
```

### 1290. Export all images
```
$ docker images --format '{{.Repository}}:{{.Tag}}' | \
  xargs -I {} docker save {} -o {}.tar
```

### 1291. Backup Docker Swarm state
```
$ docker run --rm -v /var/lib/docker/swarm:/swarm -v $(pwd):/backup \
  ubuntu tar czf /backup/swarm-backup-$(date +%Y%m%d).tar.gz /swarm
```

### 1292. Create automated backup script
```bash
#!/bin/bash
BACKUP_DIR="/backups/$(date +%Y%m%d)"
mkdir -p $BACKUP_DIR
docker ps -q | xargs -I {} docker export {} > $BACKUP_DIR/{}.tar
docker volume ls -q | xargs -I {} \
  docker run --rm -v {}:/data -v $BACKUP_DIR:/backup \
  ubuntu tar czf /backup/{}.tar.gz /data
```

### 1293. Restore volume from backup
```
$ docker run --rm -v myvolume:/data -v $(pwd):/backup \
  ubuntu tar xzf /backup/volume-backup.tar.gz -C /data --strip 1
```

### 1294. Restore container from config
```
$ cat container-config-backup.json | docker run -i --rm jq
```

### 1295. Implement snapshot strategy
```
$ docker run --rm -v myvolume:/data -v /snapshots:/snapshots \
  ubuntu cp -al /data /snapshots/snapshot-$(date +%Y%m%d-%H%M%S)
```

### 1296. Configure automated snapshots with cron
```
0 */6 * * * /usr/local/bin/docker-snapshot.sh
```

### 1297. Backup to remote storage
```
$ docker run --rm -v myvolume:/data \
  -e AWS_ACCESS_KEY_ID=$AWS_KEY \
  -e AWS_SECRET_ACCESS_KEY=$AWS_SECRET \
  mesosphere/aws-cli s3 sync /data s3://my-backup-bucket/
```

### 1298. Restore from S3
```
$ docker run --rm -v myvolume:/data \
  -e AWS_ACCESS_KEY_ID=$AWS_KEY \
  -e AWS_SECRET_ACCESS_KEY=$AWS_SECRET \
  mesosphere/aws-cli s3 sync s3://my-backup-bucket/ /data
```

### 1299. Backup to Azure Blob Storage
```
$ docker run --rm -v myvolume:/data \
  microsoft/azure-cli az storage blob upload-batch \
  --destination backup-container --source /data
```

### 1300. Create point-in-time recovery
```
$ docker run --rm -v myvolume:/data -v /backups:/backups \
  ubuntu sh -c 'tar czf /backups/pitr-$(date +%s).tar.gz /data'
```

### 1301. Test backup integrity
```
$ docker run --rm -v $(pwd):/backup ubuntu tar tzf /backup/backup.tar.gz > /dev/null
```

### 1302. Implement 3-2-1 backup strategy
```bash
# 3 copies, 2 different media, 1 offsite
docker volume backup to local disk
rsync to NAS
upload to cloud storage
```

### 1303. Configure volume replication
```
$ docker volume create --driver rexray/s3fs:latest \
  --opt replication=sync replicated-volume
```

### 1304. Set up cross-region replication
```
$ docker run --rm \
  -e SOURCE_REGION=us-east-1 \
  -e DEST_REGION=eu-west-1 \
  replication-tool replicate
```

### 1305. Implement failover mechanism
```
$ docker service update --image myapp:backup-instance \
  --constraint 'node.labels.region==backup' myapp
```

### 1306. Create disaster recovery runbook
```markdown
# DR Runbook
1. Restore volumes from backup
2. Recreate networks
3. Deploy services
4. Verify application health
5. Update DNS records
```

### 1307. Test DR procedure
```
$ ./test-dr.sh --dry-run
```

### 1308. Monitor backup health
```
$ docker run --rm -v /backups:/backups \
  backup-monitor check --age 24h --size 100M
```

### 1309. Set up backup retention policy
```bash
find /backups -name "*.tar.gz" -mtime +30 -delete
```

### 1310. Encrypt backups
```
$ docker run --rm -v myvolume:/data -v $(pwd):/backup \
  ubuntu sh -c 'tar czf - /data | openssl enc -aes-256-cbc -out /backup/encrypted-backup.tar.gz.enc'
```

### 1311. Decrypt and restore backup
```
$ openssl enc -d -aes-256-cbc -in encrypted-backup.tar.gz.enc | \
  docker run -i --rm -v myvolume:/data ubuntu tar xzf - -C /data
```

### 1312. Implement versioned backups
```
$ docker run --rm -v myvolume:/data -v /backups:/backups \
  ubuntu tar czf /backups/backup-v$(cat /data/version).tar.gz /data
```

### 1313. Create differential backup
```
$ docker run --rm -v myvolume:/data -v /backups:/backups \
  ubuntu rsync -av --compare-dest=/backups/full /data/ /backups/diff/
```

### 1314. Backup Docker secrets
```
$ docker secret ls --format '{{.Name}}' | \
  xargs -I {} docker secret inspect {} > secrets-backup.json
```

### 1315. Restore Docker secrets
```
$ cat secrets-backup.json | jq -r '.[] | .Spec.Name' | \
  xargs -I {} docker secret create {} -
```

---

## 10. MULTI-CLOUD & HYBRID DEPLOYMENTS

### 1316. Configure Docker for AWS
```
$ docker context create ecs myapp --from-env
$ docker context use ecs myapp
```

### 1317. Deploy to AWS ECS
```
$ docker compose -f docker-compose.yml up
```

### 1318. Configure Docker for Azure ACI
```
$ docker context create aci myapp \
  --subscription-id $AZURE_SUBSCRIPTION \
  --resource-group mygroup \
  --location eastus
```

### 1319. Deploy to Azure Container Instances
```
$ docker context use aci myapp
$ docker run -d -p 80:80 nginx
```

### 1320. Set up Google Cloud Run
```
$ gcloud run deploy myapp \
  --image gcr.io/myproject/myimage \
  --platform managed \
  --region us-central1
```

### 1321. Deploy to GKE with Docker
```
$ docker build -t gcr.io/myproject/myapp .
$ docker push gcr.io/myproject/myapp
$ kubectl apply -f deployment.yaml
```

### 1322. Configure multi-cloud registry sync
```
$ docker run -d --name registry-sync \
  -e SOURCE_REGISTRY=docker.io \
  -e DEST_REGISTRY=gcr.io \
  registry-sync:latest
```

### 1323. Use Skopeo for cross-registry copy
```
$ skopeo copy \
  docker://docker.io/myimage:latest \
  docker://gcr.io/myproject/myimage:latest
```

### 1324. Sync images across regions
```
$ for region in us-east-1 eu-west-1 ap-south-1; do
  docker tag myimage:latest $region.dkr.ecr.amazonaws.com/myimage:latest
  docker push $region.dkr.ecr.amazonaws.com/myimage:latest
done
```

### 1325. Configure hybrid cloud networking
```
$ docker network create \
  --driver overlay \
  --opt encrypted=true \
  --subnet 10.0.0.0/16 \
  --attachable \
  hybrid-network
```

### 1326. Set up VPN for hybrid deployment
```
$ docker run -d --name openvpn \
  --cap-add=NET_ADMIN \
  -v openvpn-data:/etc/openvpn \
  kylemanna/openvpn
```

### 1327. Configure cloud storage for volumes
```
$ docker volume create \
  --driver rexray/s3fs:latest \
  --opt accessKey=$AWS_KEY \
  --opt secretKey=$AWS_SECRET \
  cloud-volume
```

### 1328. Use Azure Files for volumes
```
$ docker volume create \
  --driver local \
  --opt type=cifs \
  --opt device=//myaccount.file.core.windows.net/myshare \
  --opt o=username=myaccount,password=$AZURE_KEY \
  azure-volume
```

### 1329. Configure GCS for persistent storage
```
$ docker volume create \
  --driver gcsfuse \
  --opt bucket=my-gcs-bucket \
  gcs-volume
```

### 1330. Deploy to multiple clouds simultaneously
```
$ parallel ::: \
  'docker --context aws compose up' \
  'docker --context azure compose up' \
  'docker --context gcp compose up'
```

### 1331. Implement cloud failover
```bash
if ! curl -f http://primary-cloud/health; then
  docker context use backup-cloud
  docker service update --force myapp
fi
```

### 1332. Configure cloud-agnostic deployment
```yaml
version: '3.8'
services:
  app:
    image: myapp:latest
    deploy:
      placement:
        constraints:
          - cloud.provider==aws || cloud.provider==azure
```

### 1333. Use Terraform for multi-cloud
```hcl
resource "docker_container" "app" {
  count = var.cloud_provider == "aws" ? 1 : 0
  image = docker_image.app.latest
  name  = "app-aws"
}
```

### 1334. Implement cloud cost optimization
```
$ docker run -d --name cost-optimizer \
  -e CLOUDS=aws,azure,gcp \
  cost-optimizer:latest
```

### 1335. Monitor multi-cloud deployments
```
$ docker run -d --name multi-cloud-monitor \
  -e AWS_REGION=us-east-1 \
  -e AZURE_REGION=eastus \
  -e GCP_REGION=us-central1 \
  cloud-monitor:latest
```

---

## 11. GITOPS WITH DOCKER

### 1336. Set up ArgoCD with Docker
```
$ docker run -d --name argocd \
  -p 8080:8080 -p 8083:8083 \
  argoproj/argocd:latest
```

### 1337. Configure FluxCD for GitOps
```
$ docker run --rm -it \
  -v ~/.kube:/root/.kube \
  fluxcd/flux:latest install
```

### 1338. Use Git as source of truth
```yaml
# docker-compose.yml in Git
version: '3.8'
services:
  app:
    image: myapp:${GIT_COMMIT_SHA}
    deploy:
      replicas: 3
```

### 1339. Implement automated deployment from Git
```bash
git clone https://github.com/user/repo.git
cd repo
docker compose up -d
```

### 1340. Configure webhook for Git push
```
$ docker run -d --name webhook \
  -p 9000:9000 \
  -v ./hooks.json:/etc/webhook/hooks.json \
  almir/webhook
```

### 1341. Create deployment hook
```json
[
  {
    "id": "deploy-app",
    "execute-command": "/scripts/deploy.sh",
    "command-working-directory": "/app",
    "trigger-rule": {
      "match": {
        "type": "payload-hash-sha1",
        "secret": "mysecret",
        "parameter": {
          "source": "header",
          "name": "X-Hub-Signature"
        }
      }
    }
  }
]
```

### 1342. Use Renovate for dependency updates
```
$ docker run -d --name renovate \
  -e RENOVATE_TOKEN=$GITHUB_TOKEN \
  -v ./config.json:/usr/src/app/config.json \
  renovate/renovate:latest
```

### 1343. Implement canary deployments with Git
```bash
# Deploy to canary based on branch
if [[ $GIT_BRANCH == "canary" ]]; then
  docker service update --image myapp:canary --replicas 1 myapp-canary
fi
```

### 1344. Configure progressive delivery
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Rollout
spec:
  strategy:
    canary:
      steps:
      - setWeight: 20
      - pause: {duration: 1h}
      - setWeight: 50
      - pause: {duration: 1h}
```

### 1345. Use Kustomize with Docker
```
$ docker run --rm -v $(pwd):/app \
  k8s.gcr.io/kustomize/kustomize:latest \
  build /app | docker compose -f - up
```

### 1346. Implement GitOps workflow
```bash
# 1. Commit changes
git commit -m "Update image version"
# 2. Push to repository
git push origin main
# 3. CI/CD pipeline triggered
# 4. Automated deployment
```

### 1347. Version control Docker configs
```
$ docker config create app_config_v2 ./config.json
$ docker service update --config-rm app_config_v1 \
  --config-add app_config_v2 myapp
```

### 1348. Track infrastructure changes
```
$ docker system events --format '{{json .}}' | \
  jq -r '. | @json' >> infrastructure-changelog.jsonl
```

### 1349. Implement approval gates
```yaml
# github-actions.yml
jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: production
      url: https://example.com
    steps:
      - run: docker compose up -d
```

### 1350. Rollback using Git
```
$ git revert HEAD
$ git push origin main
# Automated rollback triggered
```

---

## 12. SERVICE MESH INTEGRATION

### 1351. Install Istio sidecar
```
$ docker run -d --name app \
  --label istio-injection=enabled \
  myapp:latest
```

### 1352. Configure Linkerd with Docker
```
$ docker run -d --name linkerd \
  -p 4191:4191 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  linkerd/linkerd:latest
```

### 1353. Deploy Consul Connect
```
$ docker run -d --name consul \
  -p 8500:8500 \
  consul:latest agent -dev
```

### 1354. Register service with Consul
```
$ curl -X PUT -d @service.json \
  http://localhost:8500/v1/agent/service/register
```

### 1355. Configure service mesh proxy
```
$ docker run -d --name envoy \
  -p 10000:10000 \
  -v ./envoy.yaml:/etc/envoy/envoy.yaml \
  envoyproxy/envoy:latest
```

### 1356. Implement mTLS with service mesh
```yaml
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: default
spec:
  mtls:
    mode: STRICT
```

### 1357. Configure traffic splitting
```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
spec:
  http:
  - match:
    - headers:
        user-agent:
          regex: ".*Chrome.*"
    route:
    - destination:
        host: app-v2
      weight: 100
```

### 1358. Set up circuit breaking
```yaml
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
spec:
  trafficPolicy:
    connectionPool:
      tcp:
        maxConnections: 100
      http:
        http1MaxPendingRequests: 50
        maxRequestsPerConnection: 2
    outlierDetection:
      consecutiveErrors: 5
      interval: 30s
      baseEjectionTime: 30s
```

### 1359. Configure retry policies
```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
spec:
  http:
  - retries:
      attempts: 3
      perTryTimeout: 2s
      retryOn: 5xx,retriable-4xx
```

### 1360. Implement timeout policies
```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
spec:
  http:
  - timeout: 10s
```

### 1361. Configure rate limiting
```yaml
apiVersion: networking.istio.io/v1alpha3
kind: EnvoyFilter
spec:
  configPatches:
  - applyTo: HTTP_FILTER
    match:
      context: SIDECAR_INBOUND
    patch:
      operation: INSERT_BEFORE
      value:
        name: envoy.filters.http.local_ratelimit
        typed_config:
          "@type": type.googleapis.com/envoy.extensions.filters.http.local_ratelimit.v3.LocalRateLimit
          stat_prefix: http_local_rate_limiter
          token_bucket:
            max_tokens: 100
            tokens_per_fill: 100
            fill_interval: 1s
```

### 1362. Set up service mesh observability
```
$ docker run -d --name jaeger-mesh \
  -p 16686:16686 \
  jaegertracing/all-in-one:latest
```

### 1363. Configure distributed tracing
```yaml
apiVersion: install.istio.io/v1alpha1
kind: IstioOperator
spec:
  meshConfig:
    enableTracing: true
    defaultConfig:
      tracing:
        sampling: 100
```

### 1364. Implement service-to-service auth
```yaml
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: service-auth
spec:
  action: ALLOW
  rules:
  - from:
    - source:
        principals: ["cluster.local/ns/default/sa/frontend"]
    to:
    - operation:
        methods: ["GET"]
```

### 1365. Configure egress traffic
```yaml
apiVersion: networking.istio.io/v1alpha3
kind: ServiceEntry
metadata:
  name: external-service
spec:
  hosts:
  - api.external.com
  ports:
  - number: 443
    name: https
    protocol: HTTPS
  location: MESH_EXTERNAL
  resolution: DNS
```

---

## 13. SERVERLESS CONTAINERS

### 1366. Deploy to AWS Fargate
```
$ docker context create ecs myapp
$ docker context use ecs myapp
$ docker compose up
```

### 1367. Configure Fargate task definition
```json
{
  "family": "myapp",
  "networkMode": "awsvpc",
  "requiresCompatibilities": ["FARGATE"],
  "cpu": "256",
  "memory": "512",
  "containerDefinitions": [
    {
      "name": "app",
      "image": "myapp:latest",
      "portMappings": [
        {
          "containerPort": 80,
          "protocol": "tcp"
        }
      ]
    }
  ]
}
```

### 1368. Deploy to Google Cloud Run
```
$ gcloud run deploy myapp \
  --image gcr.io/myproject/myapp \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated
```

### 1369. Configure Cloud Run autoscaling
```
$ gcloud run services update myapp \
  --min-instances=1 \
  --max-instances=100 \
  --concurrency=80
```

### 1370. Deploy to Azure Container Apps
```
$ az containerapp create \
  --name myapp \
  --resource-group mygroup \
  --image myregistry.azurecr.io/myapp:latest \
  --target-port 80 \
  --ingress external \
  --min-replicas 1 \
  --max-replicas 10
```

### 1371. Use Knative for serverless
```
$ kubectl apply -f - <<EOF
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  name: myapp
spec:
  template:
    spec:
      containers:
      - image: myapp:latest
        ports:
        - containerPort: 8080
EOF
```

### 1372. Configure scale-to-zero
```yaml
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  name: myapp
spec:
  template:
    metadata:
      annotations:
        autoscaling.knative.dev/minScale: "0"
        autoscaling.knative.dev/maxScale: "100"
```

### 1373. Set concurrency limits
```yaml
spec:
  template:
    spec:
      containerConcurrency: 10
```

### 1374. Configure cold start optimization
```
$ docker build --build-arg ENABLE_WARMUP=true -t myapp:optimized .
```

### 1375. Implement request-based scaling
```yaml
metadata:
  annotations:
    autoscaling.knative.dev/target: "100"
    autoscaling.knative.dev/metric: "rps"
```

### 1376. Use OpenFaaS for functions
```
$ docker run -d --name openfaas \
  -p 8080:8080 \
  openfaas/gateway:latest
```

### 1377. Deploy function with OpenFaaS
```
$ faas-cli deploy --image myfunction:latest --name myfunction
```

### 1378. Configure function autoscaling
```yaml
functions:
  myfunction:
    labels:
      com.openfaas.scale.min: "1"
      com.openfaas.scale.max: "10"
      com.openfaas.scale.factor: "20"
```

### 1379. Implement custom metrics scaling
```yaml
annotations:
  autoscaling.knative.dev/class: "hpa.autoscaling.knative.dev"
  autoscaling.knative.dev/metric: "cpu"
  autoscaling.knative.dev/target: "75"
```

### 1380. Use AWS Lambda with containers
```
$ aws lambda create-function \
  --function-name myfunction \
  --package-type Image \
  --code ImageUri=123456789012.dkr.ecr.us-east-1.amazonaws.com/myfunction:latest \
  --role arn:aws:iam::123456789012:role/lambda-role
```

---

## 14. EDGE COMPUTING & IOT

### 1381. Deploy to edge with K3s
```
$ docker run -d --privileged --name k3s \
  -p 6443:6443 \
  rancher/k3s:latest server
```

### 1382. Configure lightweight container runtime
```
$ docker run --runtime=crun myapp:latest
```

### 1383. Use containerd for edge
```
$ ctr images pull docker.io/library/nginx:alpine
$ ctr run docker.io/library/nginx:alpine nginx
```

### 1384. Implement offline container operations
```
$ docker save myapp:latest -o myapp.tar
$ docker load -i myapp.tar
```

### 1385. Configure ARM containers for IoT
```
$ docker buildx build --platform linux/arm/v7 -t myapp:armv7 .
```

### 1386. Deploy to Raspberry Pi
```
$ docker run -d --restart=unless-stopped \
  --device /dev/gpiomem \
  myiot:latest
```

### 1387. Use Balena for IoT fleet management
```
$ balena push myapp
```

### 1388. Configure edge caching
```
$ docker run -d --name edge-cache \
  -v cache:/cache \
  nginx:alpine
```

### 1389. Implement local registry for edge
```
$ docker run -d --name edge-registry \
  -p 5000:5000 \
  -v /mnt/registry:/var/lib/registry \
  --restart=always \
  registry:2
```

### 1390. Configure bandwidth-limited deployments
```
$ docker run --network-mode=slirp4netns:outbound_addr=127.0.0.1,enable_ipv6=true \
  myapp:latest
```

### 1391. Use resource constraints for edge
```
$ docker run --memory=128m --cpus=0.5 --pids-limit=50 myapp:latest
```

### 1392. Implement edge analytics
```
$ docker run -d --name edge-analytics \
  -v /data:/data \
  edge-analytics:latest
```

### 1393. Configure edge-to-cloud sync
```
$ docker run -d --name sync-agent \
  -e CLOUD_ENDPOINT=https://cloud.example.com \
  -v /local/data:/data \
  sync-agent:latest
```

### 1394. Use MQTT for IoT messaging
```
$ docker run -d --name mosquitto \
  -p 1883:1883 -p 9001:9001 \
  eclipse-mosquitto:latest
```

### 1395. Implement edge ML inference
```
$ docker run -d --name edge-ml \
  --device /dev/video0 \
  -v models:/models \
  tensorflow/serving:latest
```

---

## 15. ADVANCED TROUBLESHOOTING & DEBUGGING

### 1396. Enable Docker daemon debug mode
```
$ sudo dockerd --debug
```

### 1397. Capture Docker API traffic
```
$ socat -v UNIX-LISTEN:/tmp/docker.sock,fork UNIX-CONNECT:/var/run/docker.sock
```

### 1398. Debug BuildKit issues
```
$ docker buildx build --progress=plain --no-cache -t debug .
```

### 1399. Inspect build layers interactively
```
$ docker build --target debug -t debug . && docker run -it debug sh
```

### 1400. Use nsenter to debug containers
```
$ docker inspect -f '{{.State.Pid}}' mycontainer
$ sudo nsenter -t <PID> -n -m -u -i -p
```

### 1401. Debug networking with netshoot
```
$ docker run -it --rm --net container:mycontainer nicolaka/netshoot
```

### 1402. Capture container network traffic
```
$ docker run --rm --net=host --cap-add=NET_RAW \
  nicolaka/netshoot tcpdump -i any -w /tmp/capture.pcap
```

### 1403. Analyze syscalls with strace
```
$ docker run --rm --cap-add=SYS_PTRACE --pid=container:mycontainer \
  alpine strace -p 1
```

### 1404. Debug with gdb
```
$ docker run -it --rm --cap-add=SYS_PTRACE --pid=container:mycontainer \
  gcc gdb -p 1
```

### 1405. Profile CPU usage
```
$ docker run --rm --pid=container:mycontainer \
  brendangregg/perf-tools:latest perf record -F 99 -a -g -- sleep 30
```

### 1406. Analyze memory usage
```
$ docker exec mycontainer cat /proc/meminfo
```

### 1407. Debug OOM kills
```
$ dmesg | grep -i 'killed process'
$ docker inspect -f '{{.State.OOMKilled}}' mycontainer
```

### 1408. Check container exit code
```
$ docker inspect -f '{{.State.ExitCode}}' mycontainer
```

### 1409. View detailed error logs
```
$ journalctl -u docker.service -n 100 --no-pager
```

### 1410. Debug storage driver issues
```
$ docker info --format '{{json .DriverStatus}}' | jq
```

### 1411. Check overlay2 metadata
```
$ sudo cat /var/lib/docker/image/overlay2/repositories.json | jq
```

### 1412. Debug volume mount issues
```
$ docker exec mycontainer mount | grep -i volume
```

### 1413. Inspect inode usage
```
$ docker exec mycontainer df -i
```

### 1414. Check file descriptor limits
```
$ docker exec mycontainer sh -c 'ulimit -n'
```

### 1415. Debug DNS resolution
```
$ docker exec mycontainer nslookup example.com
$ docker exec mycontainer cat /etc/resolv.conf
```

### 1416. Test network connectivity
```
$ docker exec mycontainer ping -c 3 8.8.8.8
$ docker exec mycontainer traceroute google.com
```

### 1417. Check routing table
```
$ docker exec mycontainer ip route
```

### 1418. Debug iptables rules
```
$ sudo iptables -t nat -L -n -v | grep DOCKER
```

### 1419. Analyze bridge network
```
$ docker network inspect bridge | jq
```

### 1420. Check port bindings
```
$ sudo netstat -tulpn | grep docker
```

### 1421. Debug image pull issues
```
$ docker pull -a myimage --debug
```

### 1422. Check registry connectivity
```
$ curl -v https://registry-1.docker.io/v2/
```

### 1423. Verify TLS certificates
```
$ openssl s_client -connect registry.example.com:443
```

### 1424. Debug build context issues
```
$ docker build --no-cache --progress=plain . 2>&1 | tee build.log
```

### 1425. Inspect build cache
```
$ docker builder prune --verbose
```

### 1426. Debug Dockerfile syntax
```
$ docker run --rm -i hadolint/hadolint < Dockerfile
```

### 1427. Validate Compose file
```
$ docker compose config --quiet
```

### 1428. Check Swarm cluster health
```
$ docker node ls
$ docker node inspect --format '{{.Status.State}}' $(docker node ls -q)
```

### 1429. Debug service deployment
```
$ docker service ps --no-trunc myservice
```

### 1430. Inspect service logs
```
$ docker service logs --raw --tail 100 myservice
```

### 1431. Check service endpoint
```
$ docker service inspect --format '{{json .Endpoint}}' myservice | jq
```

### 1432. Debug load balancing
```
$ for i in {1..10}; do curl -s http://service-vip | grep hostname; done
```

### 1433. Verify secrets access
```
$ docker exec mycontainer ls -la /run/secrets/
```

### 1434. Check config mounts
```
$ docker exec mycontainer cat /config/app.json
```

### 1435. Debug health check failures
```
$ docker inspect --format='{{json .State.Health}}' mycontainer | jq
```

### 1436. Test health check manually
```
$ docker exec mycontainer sh -c 'curl -f http://localhost/health || exit 1'
```

### 1437. Analyze container startup time
```
$ docker events --filter 'event=start' --filter 'container=mycontainer' \
  --format '{{.Time}}'
```

### 1438. Debug slow builds
```
$ time docker build --no-cache -t myimage .
```

### 1439. Profile Docker commands
```
$ time docker run --rm alpine echo test
```

### 1440. Check Docker daemon responsiveness
```
$ time docker ps
```

### 1441. Debug hung containers
```
$ docker exec mycontainer ps aux
$ docker top mycontainer aux
```

### 1442. Force remove stuck container
```
$ docker rm -f mycontainer
$ sudo rm -rf /var/lib/docker/containers/<container-id>
```

### 1443. Recover from corrupted Docker state
```
$ sudo systemctl stop docker
$ sudo rm /var/lib/docker/network/files/local-kv.db
$ sudo systemctl start docker
```

### 1444. Debug registry push issues
```
$ docker push myimage:latest --debug
```

### 1445. Check image manifest
```
$ docker manifest inspect myimage:latest
```

### 1446. Verify image layers
```
$ docker history --no-trunc myimage:latest
```

### 1447. Debug permission issues
```
$ docker exec mycontainer ls -la /
$ docker exec mycontainer id
```

### 1448. Check AppArmor status
```
$ docker exec mycontainer cat /proc/self/attr/current
```

### 1449. Verify SELinux context
```
$ docker exec mycontainer ls -Z /
```

### 1450. Debug seccomp violations
```
$ sudo ausearch -m SECCOMP -ts recent
```

### 1451. Analyze capability usage
```
$ docker exec mycontainer capsh --print
```

### 1452. Check cgroup limits
```
$ docker exec mycontainer cat /sys/fs/cgroup/memory/memory.limit_in_bytes
```

### 1453. Debug resource constraints
```
$ docker stats --no-stream --format "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}\t{{.MemPerc}}\t{{.NetIO}}\t{{.BlockIO}}\t{{.PIDs}}"
```

### 1454. Monitor real-time events
```
$ docker events --filter 'type=container' --format '{{json .}}'
```

### 1455. Create diagnostic bundle
```
$ docker system diagnose > docker-diagnostics.log
```

### 1456. Export container state
```
$ docker inspect mycontainer > container-state.json
```

### 1457. Dump Docker daemon config
```
$ docker info --format '{{json .}}' > daemon-info.json
```

### 1458. Collect all logs
```
$ docker logs $(docker ps -aq) > all-logs.txt 2>&1
```

### 1459. Generate support bundle
```bash
#!/bin/bash
mkdir -p docker-support
docker info > docker-support/info.txt
docker version > docker-support/version.txt
docker ps -a > docker-support/containers.txt
docker images > docker-support/images.txt
docker network ls > docker-support/networks.txt
docker volume ls > docker-support/volumes.txt
journalctl -u docker > docker-support/daemon-logs.txt
tar czf docker-support-$(date +%Y%m%d).tar.gz docker-support/
```

### 1460. Debug with verbose output
```
$ docker --log-level debug run -it alpine sh
```

### 1461. Enable experimental features
```json
{
  "experimental": true,
  "debug": true
}
```

### 1462. Test with minimal image
```
$ docker run --rm -it busybox sh
```

### 1463. Compare working vs broken config
```
$ diff <(docker inspect working-container) <(docker inspect broken-container)
```

### 1464. Validate JSON output
```
$ docker inspect mycontainer | jq empty
```

### 1465. Test registry authentication
```
$ docker login --username test --password test myregistry.com
```

### 1466. Check Docker socket permissions
```
$ ls -la /var/run/docker.sock
$ sudo chmod 666 /var/run/docker.sock
```

### 1467. Debug with isolated environment
```
$ docker run -it --rm --network=none --read-only alpine sh
```

### 1468. Test with different runtime
```
$ docker run --runtime=runc myapp
$ docker run --runtime=kata-runtime myapp
```

### 1469. Benchmark disk I/O
```
$ docker run --rm -v testvol:/data alpine dd if=/dev/zero of=/data/test bs=1M count=100
```

### 1470. Test network throughput
```
$ docker run --rm networkstatic/iperf3 -s &
$ docker run --rm networkstatic/iperf3 -c server-ip
```

### 1471. Profile container startup
```
$ time docker run --rm alpine echo "test"
```

### 1472. Check for zombies
```
$ docker exec mycontainer ps aux | grep defunct
```

### 1473. Debug init system
```
$ docker exec mycontainer ps -p 1
```

### 1474. Verify environment variables
```
$ docker exec mycontainer env | sort
```

### 1475. Check process limits
```
$ docker exec mycontainer cat /proc/1/limits
```

### 1476. Analyze file system usage
```
$ docker exec mycontainer du -sh /*
```

### 1477. Find large files
```
$ docker exec mycontainer find / -type f -size +100M
```

### 1478. Check open files
```
$ docker exec mycontainer lsof | wc -l
```

### 1479. Monitor system calls
```
$ docker run --rm --cap-add=SYS_PTRACE \
  --pid=container:mycontainer \
  alpine strace -c -p 1
```

### 1480. Debug with ephemeral container
```
$ docker run -it --rm --pid=container:mycontainer \
  --net=container:mycontainer \
  --cap-add sys_admin \
  alpine sh
```

### 1481. Create debug Dockerfile
```dockerfile
FROM myapp:latest
RUN apk add --no-cache curl netcat-openbsd tcpdump strace
CMD ["/bin/sh"]
```

### 1482. Test with minimal privileges
```
$ docker run --rm --user=nobody --cap-drop=ALL alpine id
```

### 1483. Verify image integrity
```
$ docker trust inspect --pretty myimage:latest
```

### 1484. Check plugin status
```
$ docker plugin ls
$ docker plugin inspect myplugin
```

### 1485. Debug builder instance
```
$ docker buildx inspect --bootstrap
```

### 1486. Clear BuildKit state
```
$ docker buildx prune -af
$ rm -rf ~/.docker/buildx
```

### 1487. Test context switching
```
$ docker context ls
$ docker context use default
$ docker ps
```

### 1488. Verify daemon config
```
$ sudo cat /etc/docker/daemon.json
$ docker info | grep -A 20 "Server Version"
```

### 1489. Check for version compatibility
```
$ docker version --format '{{.Client.Version}} vs {{.Server.Version}}'
```

### 1490. Monitor Docker events continuously
```
$ docker events --since '2024-01-01' --format '{{json .}}' | jq
```

### 1491. Trace API calls
```
$ export DOCKER_API_VERSION=1.41
$ docker --log-level=debug pull nginx
```

### 1492. Debug with custom DNS
```
$ docker run --dns=8.8.8.8 --dns=1.1.1.1 alpine nslookup google.com
```

### 1493. Test with host networking
```
$ docker run --rm --network=host alpine netstat -tuln
```

### 1494. Verify kernel compatibility
```
$ docker run --rm alpine uname -r
```

### 1495. Check storage driver compatibility
```
$ docker info | grep "Storage Driver"
$ docker info | grep "Backing Filesystem"
```

### 1496. Test image on different architectures
```
$ docker manifest inspect --verbose myimage:latest
```

### 1497. Debug QEMU emulation
```
$ docker run --rm --privileged multiarch/qemu-user-static --reset -p yes
```

### 1498. Verify BuildKit features
```
$ docker buildx ls
$ docker buildx inspect --bootstrap default
```

### 1499. Run comprehensive health check
```bash
#!/bin/bash
echo "=== Docker Version ==="
docker version
echo "=== Docker Info ==="
docker info
echo "=== Running Containers ==="
docker ps
echo "=== Container Stats ==="
docker stats --no-stream
echo "=== Disk Usage ==="
docker system df
echo "=== Network List ==="
docker network ls
echo "=== Volume List ==="
docker volume ls
```

### 1500. Final comprehensive diagnostic
```
$ docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
  -v /:/host:ro \
  --privileged \
  nicolaka/netshoot \
  /bin/bash -c "docker info && docker ps && ip addr && netstat -tulpn"
```

---

## FINAL SUMMARY - PART 3

This Part 3 expert-level course covered:
- **500 additional commands** (1001-1500)
- Docker Enterprise & DTR operations
- Advanced Swarm orchestration
- Deep dive into container networking
- Custom plugin development
- Advanced registry management
- BuildKit advanced features
- Container security hardening
- Observability and telemetry
- Disaster recovery strategies
- Multi-cloud and hybrid deployments
- GitOps workflows
- Service mesh integration
- Serverless containers
- Edge computing and IoT
- Advanced troubleshooting and debugging

--- 
[JOIN FOR LIVE CLASSES AND SUPPORT](https://t.me/efxtv) 

[![Buy Me A Coffee](https://img.buymeacoffee.com/button-api/?text=Buy%20me%20a%20coffee&emoji=&slug=efxtv&button_colour=FFDD00&font_colour=000000&font_family=Cookie&outline_colour=000000&coffee_colour=ffffff)](https://buymeacoffee.com/efxtv)

