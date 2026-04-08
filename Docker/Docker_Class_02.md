# COMPLETE DOCKER COURSE - PART 2 (ADVANCED TO EXPERT)

## TABLE OF CONTENTS
1. Docker BuildKit & Advanced Builds
2. Multi-Architecture Images
3. Docker Contexts
4. Docker Plugins
5. Container Runtime & OCI
6. Docker Benchmarking & Performance
7. Docker API & SDK
8. Advanced Networking
9. Storage Drivers & Graph Drivers
10. Docker in CI/CD
11. Docker Desktop Features
12. Container Orchestration Patterns
13. Advanced Security & Scanning
14. Docker Extensions & Tools
15. Production Best Practices

---

## 1. DOCKER BUILDKIT & ADVANCED BUILDS

### 511. Enable BuildKit for single build
```
$ DOCKER_BUILDKIT=1 docker build -t myimage .
```

### 512. Enable BuildKit globally
```
$ export DOCKER_BUILDKIT=1
```

### 513. Build with BuildKit and progress output
```
$ docker build --progress=plain -t myimage .
```

### 514. Build with BuildKit secrets
```
$ docker build --secret id=mysecret,src=/path/to/secret -t myimage .
```

### 515. Access secret in Dockerfile
```
RUN --mount=type=secret,id=mysecret cat /run/secrets/mysecret
```

### 516. Build with SSH agent forwarding
```
$ docker build --ssh default -t myimage .
```

### 517. Use SSH in Dockerfile
```
RUN --mount=type=ssh git clone git@github.com:user/repo.git
```

### 518. Build with cache from registry
```
$ docker build --cache-from myregistry/myimage:latest -t myimage .
```

### 519. Build with inline cache
```
$ docker build --build-arg BUILDKIT_INLINE_CACHE=1 -t myimage .
```

### 520. Export build cache
```
$ docker build --cache-to type=local,dest=/tmp/cache -t myimage .
```

### 521. Import build cache
```
$ docker build --cache-from type=local,src=/tmp/cache -t myimage .
```

### 522. Build with registry cache
```
$ docker build --cache-to type=registry,ref=myregistry/cache -t myimage .
```

### 523. Build with max cache
```
$ docker build --cache-to type=registry,ref=myregistry/cache,mode=max -t myimage .
```

### 524. Build with output to local filesystem
```
$ docker build --output type=local,dest=./output -t myimage .
```

### 525. Build and export to tar
```
$ docker build --output type=tar,dest=output.tar -t myimage .
```

### 526. Build with custom network mode
```
$ docker build --network=host -t myimage .
```

### 527. Build with specific platform
```
$ docker build --platform linux/amd64 -t myimage .
```

### 528. Build with multiple platforms
```
$ docker buildx build --platform linux/amd64,linux/arm64 -t myimage .
```

### 529. Create new buildx builder
```
$ docker buildx create --name mybuilder
```

### 530. Use specific builder
```
$ docker buildx use mybuilder
```

### 531. List buildx builders
```
$ docker buildx ls
```

### 532. Inspect buildx builder
```
$ docker buildx inspect mybuilder
```

### 533. Bootstrap builder
```
$ docker buildx inspect --bootstrap
```

### 534. Remove buildx builder
```
$ docker buildx rm mybuilder
```

### 535. Build and push with buildx
```
$ docker buildx build --platform linux/amd64,linux/arm64 --push -t myimage .
```

### 536. Build and load with buildx
```
$ docker buildx build --load -t myimage .
```

### 537. Create builder with driver
```
$ docker buildx create --driver docker-container --name mybuilder
```

### 538. Create builder with config
```
$ docker buildx create --config /path/to/buildkitd.toml --name mybuilder
```

### 539. Build with custom Dockerfile syntax
```
# syntax=docker/dockerfile:1.4
FROM ubuntu:22.04
```

### 540. Use heredoc in Dockerfile
```
RUN <<EOF
apt-get update
apt-get install -y curl
EOF
```

### 541. Copy with link flag
```
COPY --link . /app
```

### 542. Use bind mount in RUN
```
RUN --mount=type=bind,source=.,target=/src make build
```

### 543. Use cache mount in RUN
```
RUN --mount=type=cache,target=/root/.cache pip install -r requirements.txt
```

### 544. Use tmpfs mount in RUN
```
RUN --mount=type=tmpfs,target=/tmp make test
```

### 545. Build with metadata output
```
$ docker buildx build --metadata-file metadata.json -t myimage .
```

### 546. Prune build cache
```
$ docker buildx prune
```

### 547. Prune build cache with force
```
$ docker buildx prune -f
```

### 548. Prune all build cache
```
$ docker buildx prune -a
```

### 549. Show disk usage of build cache
```
$ docker buildx du
```

### 550. Stop builder instance
```
$ docker buildx stop mybuilder
```

---

## 2. MULTI-ARCHITECTURE IMAGES

### 551. Inspect image manifest
```
$ docker manifest inspect nginx
```

### 552. Create manifest list
```
$ docker manifest create myimage:latest myimage:amd64 myimage:arm64
```

### 553. Annotate manifest
```
$ docker manifest annotate myimage:latest myimage:amd64 --os linux --arch amd64
```

### 554. Push manifest list
```
$ docker manifest push myimage:latest
```

### 555. Inspect manifest list
```
$ docker manifest inspect myimage:latest
```

### 556. Remove local manifest list
```
$ docker manifest rm myimage:latest
```

### 557. Build for ARM64
```
$ docker buildx build --platform linux/arm64 -t myimage:arm64 .
```

### 558. Build for AMD64
```
$ docker buildx build --platform linux/amd64 -t myimage:amd64 .
```

### 559. Build for ARM v7
```
$ docker buildx build --platform linux/arm/v7 -t myimage:armv7 .
```

### 560. Build for multiple architectures simultaneously
```
$ docker buildx build --platform linux/amd64,linux/arm64,linux/arm/v7 -t myimage:latest --push .
```

### 561. Install QEMU for emulation
```
$ docker run --privileged --rm tonistiigi/binfmt --install all
```

### 562. Verify QEMU installation
```
$ docker buildx ls
```

### 563. Create cross-platform builder
```
$ docker buildx create --name multiarch --driver docker-container --use
```

### 564. Build with attestations
```
$ docker buildx build --platform linux/amd64 --provenance=true --sbom=true -t myimage .
```

### 565. Build with specific variant
```
$ docker buildx build --platform linux/arm/v8 -t myimage:armv8 .
```

### 566. Test ARM image on AMD64
```
$ docker run --platform linux/arm64 myimage:arm64
```

### 567. Pull specific platform image
```
$ docker pull --platform linux/arm64 nginx
```

### 568. Inspect image platform
```
$ docker inspect --format='{{.Os}}/{{.Architecture}}' myimage
```

### 569. Create builder with multiple nodes
```
$ docker buildx create --name multinodebuilder ssh://user@host1
$ docker buildx create --name multinodebuilder --append ssh://user@host2
```

### 570. Build using remote builder
```
$ docker buildx build --builder multinodebuilder -t myimage .
```

---

## 3. DOCKER CONTEXTS

### 571. List Docker contexts
```
$ docker context ls
```

### 572. Create new context
```
$ docker context create mycontext --docker "host=tcp://192.168.1.100:2376"
```

### 573. Create context with TLS
```
$ docker context create mycontext --docker "host=tcp://192.168.1.100:2376,ca=/path/to/ca.pem,cert=/path/to/cert.pem,key=/path/to/key.pem"
```

### 574. Use specific context
```
$ docker context use mycontext
```

### 575. Inspect context
```
$ docker context inspect mycontext
```

### 576. Update context
```
$ docker context update mycontext --docker "host=tcp://192.168.1.101:2376"
```

### 577. Remove context
```
$ docker context rm mycontext
```

### 578. Export context
```
$ docker context export mycontext
```

### 579. Export context to file
```
$ docker context export mycontext --file mycontext.tar
```

### 580. Import context
```
$ docker context import mycontext mycontext.tar
```

### 581. Show current context
```
$ docker context show
```

### 582. Run command with specific context
```
$ docker --context mycontext ps
```

### 583. Create context for remote Docker
```
$ docker context create remote --docker "host=ssh://user@remote-host"
```

### 584. Create context for Kubernetes
```
$ docker context create k8s --kubernetes config-file=/path/to/kubeconfig
```

### 585. Switch to default context
```
$ docker context use default
```

---

## 4. DOCKER PLUGINS

### 586. List installed plugins
```
$ docker plugin ls
```

### 587. Install plugin
```
$ docker plugin install vieux/sshfs
```

### 588. Install plugin with alias
```
$ docker plugin install vieux/sshfs --alias sshfs
```

### 589. Install plugin with grant permissions
```
$ docker plugin install vieux/sshfs --grant-all-permissions
```

### 590. Enable plugin
```
$ docker plugin enable vieux/sshfs
```

### 591. Disable plugin
```
$ docker plugin disable vieux/sshfs
```

### 592. Disable plugin with force
```
$ docker plugin disable -f vieux/sshfs
```

### 593. Inspect plugin
```
$ docker plugin inspect vieux/sshfs
```

### 594. Remove plugin
```
$ docker plugin rm vieux/sshfs
```

### 595. Remove plugin with force
```
$ docker plugin rm -f vieux/sshfs
```

### 596. Upgrade plugin
```
$ docker plugin upgrade vieux/sshfs
```

### 597. Set plugin configuration
```
$ docker plugin set vieux/sshfs sshkey.source=/root/.ssh/
```

### 598. Create plugin from rootfs
```
$ docker plugin create myplugin /path/to/plugin/rootfs
```

### 599. Push plugin to registry
```
$ docker plugin push myplugin:latest
```

### 600. Install network plugin
```
$ docker plugin install store/weaveworks/net-plugin:latest
```

### 601. Install volume plugin
```
$ docker plugin install rexray/s3fs
```

### 602. Use volume plugin
```
$ docker volume create -d rexray/s3fs myvol
```

### 603. Install logging plugin
```
$ docker plugin install grafana/loki-docker-driver:latest --alias loki
```

### 604. Configure container to use plugin
```
$ docker run --log-driver=loki --log-opt loki-url=http://localhost:3100/loki/api/v1/push nginx
```

### 605. List plugin capabilities
```
$ docker plugin inspect --format='{{.Config.Interface.Types}}' myplugin
```

---

## 5. CONTAINER RUNTIME & OCI

### 606. Check container runtime
```
$ docker info | grep -i runtime
```

### 607. Run container with specific runtime
```
$ docker run --runtime=runc nginx
```

### 608. Run with nvidia runtime
```
$ docker run --runtime=nvidia --gpus all nvidia/cuda:11.0-base nvidia-smi
```

### 609. List available runtimes
```
$ docker info --format '{{.Runtimes}}'
```

### 610. Configure default runtime in daemon.json
```
{
  "default-runtime": "runc",
  "runtimes": {
    "nvidia": {
      "path": "nvidia-container-runtime",
      "runtimeArgs": []
    }
  }
}
```

### 611. Run with Kata Containers runtime
```
$ docker run --runtime=kata-runtime nginx
```

### 612. Run with gVisor runsc
```
$ docker run --runtime=runsc nginx
```

### 613. Export container as OCI bundle
```
$ runc spec
```

### 614. Create OCI runtime bundle
```
$ docker export mycontainer | tar -C /bundle -xf -
```

### 615. Run OCI bundle with runc
```
$ runc run mycontainer
```

### 616. List containers with runc
```
$ runc list
```

### 617. Get container state with runc
```
$ runc state mycontainer
```

### 618. Delete container with runc
```
$ runc delete mycontainer
```

### 619. Kill container with runc
```
$ runc kill mycontainer SIGTERM
```

### 620. Pause container with runc
```
$ runc pause mycontainer
```

### 621. Resume container with runc
```
$ runc resume mycontainer
```

### 622. Execute command in runc container
```
$ runc exec mycontainer ps aux
```

### 623. Get container events with runc
```
$ runc events mycontainer
```

### 624. Create checkpoint with runc (CRIU)
```
$ runc checkpoint --image-path /checkpoints mycontainer
```

### 625. Restore from checkpoint
```
$ runc restore --image-path /checkpoints mycontainer
```

---

## 6. DOCKER BENCHMARKING & PERFORMANCE

### 626. Measure container startup time
```
$ time docker run --rm alpine echo "Hello"
```

### 627. Benchmark image pull speed
```
$ time docker pull nginx
```

### 628. Stress test CPU in container
```
$ docker run --rm -it progrium/stress --cpu 2 --timeout 30s
```

### 629. Stress test memory in container
```
$ docker run --rm -it progrium/stress --vm 1 --vm-bytes 256M --timeout 30s
```

### 630. Stress test I/O in container
```
$ docker run --rm -it progrium/stress --io 4 --timeout 30s
```

### 631. Monitor container I/O operations
```
$ docker stats --format "table {{.Container}}\t{{.BlockIO}}"
```

### 632. Measure network performance with iperf3
```
$ docker run -d --name=iperf-server networkstatic/iperf3 -s
$ docker run --rm networkstatic/iperf3 -c iperf-server
```

### 633. Test disk I/O with fio
```
$ docker run --rm -v /tmp:/tmp axltxl/docker-fio --name=test --rw=randread --size=1G
```

### 634. Profile container CPU usage
```
$ docker stats --no-stream --format "table {{.Container}}\t{{.CPUPerc}}"
```

### 635. Analyze container memory usage
```
$ docker stats --no-stream --format "table {{.Container}}\t{{.MemUsage}}\t{{.MemPerc}}"
```

### 636. Check container network statistics
```
$ docker stats --no-stream --format "table {{.Container}}\t{{.NetIO}}"
```

### 637. Monitor PID usage
```
$ docker stats --no-stream --format "table {{.Container}}\t{{.PIDs}}"
```

### 638. Measure image size efficiency
```
$ docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}"
```

### 639. Analyze layer sizes
```
$ docker history --no-trunc --format "table {{.Size}}\t{{.CreatedBy}}" myimage
```

### 640. Test container restart performance
```
$ time docker restart mycontainer
```

### 641. Benchmark build time
```
$ time docker build -t myimage .
```

### 642. Test BuildKit performance vs classic builder
```
$ time DOCKER_BUILDKIT=0 docker build -t classic .
$ time DOCKER_BUILDKIT=1 docker build -t buildkit .
```

### 643. Profile network latency between containers
```
$ docker exec container1 ping -c 10 container2
```

### 644. Test volume mount performance
```
$ docker run --rm -v testvol:/data alpine dd if=/dev/zero of=/data/test bs=1M count=100
```

### 645. Compare overlay2 vs other storage drivers
```
$ docker info | grep "Storage Driver"
```

### 646. Measure container creation time
```
$ time docker create nginx
```

### 647. Test parallel container startup
```
$ time docker-compose up -d --scale web=10
```

### 648. Benchmark registry push speed
```
$ time docker push myregistry/myimage:latest
```

### 649. Test pull speed with concurrent pulls
```
$ time (docker pull image1 & docker pull image2 & docker pull image3 & wait)
```

### 650. Monitor system resource usage during operations
```
$ docker system events & docker stats
```

---

## 7. DOCKER API & SDK

### 651. Query Docker API version
```
$ curl --unix-socket /var/run/docker.sock http://localhost/version
```

### 652. List containers via API
```
$ curl --unix-socket /var/run/docker.sock http://localhost/containers/json
```

### 653. Inspect container via API
```
$ curl --unix-socket /var/run/docker.sock http://localhost/containers/mycontainer/json
```

### 654. Start container via API
```
$ curl -X POST --unix-socket /var/run/docker.sock http://localhost/containers/mycontainer/start
```

### 655. Stop container via API
```
$ curl -X POST --unix-socket /var/run/docker.sock http://localhost/containers/mycontainer/stop
```

### 656. Create container via API
```
$ curl -X POST -H "Content-Type: application/json" --unix-socket /var/run/docker.sock -d '{"Image":"nginx"}' http://localhost/containers/create
```

### 657. Remove container via API
```
$ curl -X DELETE --unix-socket /var/run/docker.sock http://localhost/containers/mycontainer
```

### 658. List images via API
```
$ curl --unix-socket /var/run/docker.sock http://localhost/images/json
```

### 659. Pull image via API
```
$ curl -X POST --unix-socket /var/run/docker.sock http://localhost/images/create?fromImage=nginx
```

### 660. Get container logs via API
```
$ curl --unix-socket /var/run/docker.sock "http://localhost/containers/mycontainer/logs?stdout=1"
```

### 661. Get container stats via API
```
$ curl --unix-socket /var/run/docker.sock http://localhost/containers/mycontainer/stats?stream=0
```

### 662. Execute command via API
```
$ curl -X POST -H "Content-Type: application/json" --unix-socket /var/run/docker.sock -d '{"Cmd":["ls"]}' http://localhost/containers/mycontainer/exec
```

### 663. Get system info via API
```
$ curl --unix-socket /var/run/docker.sock http://localhost/info
```

### 664. Monitor events via API
```
$ curl --unix-socket /var/run/docker.sock http://localhost/events
```

### 665. Ping Docker daemon via API
```
$ curl --unix-socket /var/run/docker.sock http://localhost/_ping
```

### 666. List networks via API
```
$ curl --unix-socket /var/run/docker.sock http://localhost/networks
```

### 667. Create network via API
```
$ curl -X POST -H "Content-Type: application/json" --unix-socket /var/run/docker.sock -d '{"Name":"mynetwork"}' http://localhost/networks/create
```

### 668. List volumes via API
```
$ curl --unix-socket /var/run/docker.sock http://localhost/volumes
```

### 669. Create volume via API
```
$ curl -X POST -H "Content-Type: application/json" --unix-socket /var/run/docker.sock -d '{"Name":"myvolume"}' http://localhost/volumes/create
```

### 670. Use Python Docker SDK
```python
import docker
client = docker.from_env()
print(client.containers.list())
```

### 671. Run container with Python SDK
```python
import docker
client = docker.from_env()
container = client.containers.run("nginx", detach=True)
```

### 672. Get container logs with Python SDK
```python
import docker
client = docker.from_env()
container = client.containers.get('mycontainer')
print(container.logs())
```

### 673. Build image with Python SDK
```python
import docker
client = docker.from_env()
image = client.images.build(path=".", tag="myimage:latest")
```

### 674. Pull image with Python SDK
```python
import docker
client = docker.from_env()
image = client.images.pull("nginx:latest")
```

### 675. Use Go Docker SDK
```go
package main
import (
    "context"
    "github.com/docker/docker/client"
)
func main() {
    cli, _ := client.NewClientWithOpts(client.FromEnv)
    containers, _ := cli.ContainerList(context.Background(), types.ContainerListOptions{})
}
```

---

## 8. ADVANCED NETWORKING

### 676. Create macvlan network
```
$ docker network create -d macvlan --subnet=192.168.1.0/24 --gateway=192.168.1.1 -o parent=eth0 macvlan-net
```

### 677. Create ipvlan network (L2 mode)
```
$ docker network create -d ipvlan --subnet=192.168.1.0/24 -o parent=eth0 -o ipvlan_mode=l2 ipvlan-net
```

### 678. Create ipvlan network (L3 mode)
```
$ docker network create -d ipvlan --subnet=192.168.1.0/24 -o parent=eth0 -o ipvlan_mode=l3 ipvlan-net
```

### 679. Assign static IP to container
```
$ docker run -d --network mynetwork --ip 172.18.0.10 nginx
```

### 680. Assign IPv6 address
```
$ docker run -d --network mynetwork --ip6 2001:db8::10 nginx
```

### 681. Create IPv6 enabled network
```
$ docker network create --ipv6 --subnet=2001:db8::/64 ipv6-net
```

### 682. Disable inter-container communication
```
$ docker network create --internal isolated-net
```

### 683. Create network with custom DNS
```
$ docker network create --dns=8.8.8.8 --dns=1.1.1.1 custom-dns-net
```

### 684. Publish container to all interfaces
```
$ docker run -d -p 0.0.0.0:8080:80 nginx
```

### 685. Publish to specific interface
```
$ docker run -d -p 192.168.1.100:8080:80 nginx
```

### 686. Publish UDP port
```
$ docker run -d -p 8080:80/udp nginx
```

### 687. Publish range of ports
```
$ docker run -d -p 8080-8090:8080-8090 nginx
```

### 688. Connect container to multiple networks
```
$ docker network connect network1 mycontainer
$ docker network connect network2 mycontainer
```

### 689. Set bandwidth limit (requires tc)
```
$ docker run -d --cap-add=NET_ADMIN nginx
$ docker exec mycontainer tc qdisc add dev eth0 root tbf rate 1mbit burst 32kbit latency 400ms
```

### 690. Create VXLAN overlay network
```
$ docker network create --driver overlay --opt encrypted --subnet=10.0.9.0/24 vxlan-overlay
```

### 691. Configure network MTU
```
$ docker network create --opt com.docker.network.driver.mtu=9000 jumbo-net
```

### 692. Use host DNS resolver
```
$ docker run --dns-search=example.com nginx
```

### 693. Add extra host entries
```
$ docker run --add-host db:192.168.1.50 --add-host cache:192.168.1.51 nginx
```

### 694. Disable networking completely
```
$ docker run --network none alpine
```

### 695. Share network namespace
```
$ docker run -d --name container1 nginx
$ docker run --network container:container1 alpine
```

### 696. Create network with aux addresses
```
$ docker network create --aux-address "host1=192.168.1.5" --subnet 192.168.1.0/24 aux-net
```

### 697. Configure network ingress
```
$ docker network create --ingress --driver overlay ingress-routing
```

### 698. Set network labels
```
$ docker network create --label environment=production --label team=backend prod-net
```

### 699. Filter networks by label
```
$ docker network ls --filter label=environment=production
```

### 700. Inspect network in JSON format
```
$ docker network inspect --format='{{json .}}' mynetwork | jq
```

### 701. Get network gateway
```
$ docker network inspect --format='{{range .IPAM.Config}}{{.Gateway}}{{end}}' mynetwork
```

### 702. List containers in network
```
$ docker network inspect --format='{{range $k,$v := .Containers}}{{$k}} {{end}}' mynetwork
```

### 703. Create bridge with custom bridge name
```
$ docker network create -o com.docker.network.bridge.name=mybr0 custom-bridge
```

### 704. Enable ICC on bridge network
```
$ docker network create -o com.docker.network.bridge.enable_icc=true icc-net
```

### 705. Disable IP masquerading
```
$ docker network create -o com.docker.network.bridge.enable_ip_masquerade=false no-masq-net
```

---

## 9. STORAGE DRIVERS & GRAPH DRIVERS

### 706. Check current storage driver
```
$ docker info | grep "Storage Driver"
```

### 707. View storage driver details
```
$ docker info --format '{{json .Driver}}' | jq
```

### 708. Configure overlay2 driver
```
{
  "storage-driver": "overlay2",
  "storage-opts": [
    "overlay2.override_kernel_check=true"
  ]
}
```

### 709. Configure devicemapper driver
```
{
  "storage-driver": "devicemapper",
  "storage-opts": [
    "dm.thinpooldev=/dev/mapper/docker-thinpool",
    "dm.use_deferred_removal=true",
    "dm.use_deferred_deletion=true"
  ]
}
```

### 710. Configure btrfs driver
```
{
  "storage-driver": "btrfs"
}
```

### 711. Configure zfs driver
```
{
  "storage-driver": "zfs"
}
```

### 712. View graph driver data
```
$ docker inspect --format='{{json .GraphDriver}}' mycontainer | jq
```

### 713. Check overlay2 layers
```
$ docker inspect --format='{{.GraphDriver.Data.LowerDir}}' mycontainer
```

### 714. View merged directory
```
$ docker inspect --format='{{.GraphDriver.Data.MergedDir}}' mycontainer
```

### 715. Check upper directory
```
$ docker inspect --format='{{.GraphDriver.Data.UpperDir}}' mycontainer
```

### 716. View work directory
```
$ docker inspect --format='{{.GraphDriver.Data.WorkDir}}' mycontainer
```

### 717. List Docker data root contents
```
$ sudo ls -la /var/lib/docker/
```

### 718. Check overlay2 directory
```
$ sudo ls -la /var/lib/docker/overlay2/
```

### 719. View image layers directory
```
$ sudo ls -la /var/lib/docker/image/overlay2/
```

### 720. Check volumes directory
```
$ sudo ls -la /var/lib/docker/volumes/
```

### 721. Configure data root location
```
{
  "data-root": "/mnt/docker-data"
}
```

### 722. Move Docker data directory
```
$ sudo systemctl stop docker
$ sudo mv /var/lib/docker /mnt/docker-data
$ sudo ln -s /mnt/docker-data /var/lib/docker
$ sudo systemctl start docker
```

### 723. Enable user namespace remapping
```
{
  "userns-remap": "default"
}
```

### 724. Configure specific user for remapping
```
{
  "userns-remap": "dockremap:dockremap"
}
```

### 725. Check storage driver options
```
$ docker info --format '{{json .DriverStatus}}' | jq
```

### 726. Set XFS project quotas
```
{
  "storage-opts": [
    "overlay2.size=10G"
  ]
}
```

### 727. Configure direct-lvm mode
```
{
  "storage-driver": "devicemapper",
  "storage-opts": [
    "dm.directlvm_device=/dev/xvdf",
    "dm.thinp_percent=95",
    "dm.thinp_metapercent=1",
    "dm.thinp_autoextend_threshold=80",
    "dm.thinp_autoextend_percent=20",
    "dm.directlvm_device_force=false"
  ]
}
```

### 728. View container filesystem changes
```
$ docker diff mycontainer
```

### 729. Export container filesystem
```
$ docker export mycontainer > container-fs.tar
```

### 730. Import filesystem as image
```
$ docker import container-fs.tar myimage:imported
```

---

## 10. DOCKER IN CI/CD

### 731. GitLab CI: Build Docker image
```yaml
build:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t myimage:$CI_COMMIT_SHA .
    - docker push myimage:$CI_COMMIT_SHA
```

### 732. GitLab CI: Use Docker layer caching
```yaml
build:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  variables:
    DOCKER_DRIVER: overlay2
  before_script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
  script:
    - docker pull $CI_REGISTRY_IMAGE:latest || true
    - docker build --cache-from $CI_REGISTRY_IMAGE:latest -t $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA .
    - docker push $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
```

### 733. Jenkins: Build in pipeline
```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t myimage:${BUILD_NUMBER} .'
            }
        }
    }
}
```

### 734. Jenkins: Use Docker agent
```groovy
pipeline {
    agent {
        docker {
            image 'node:14'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
    }
}
```

### 735. GitHub Actions: Build and push
```yaml
name: Docker Build
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Build image
        run: docker build -t myimage:${{ github.sha }} .
      - name: Push image
        run: docker push myimage:${{ github.sha }}
```

### 736. GitHub Actions: Multi-platform build
```yaml
- name: Set up QEMU
  uses: docker/setup-qemu-action@v2
- name: Set up Docker Buildx
  uses: docker/setup-buildx-action@v2
- name: Build and push
  uses: docker/build-push-action@v4
  with:
    platforms: linux/amd64,linux/arm64
    push: true
    tags: myimage:latest
```

### 737. CircleCI: Build Docker image
```yaml
version: 2.1
jobs:
  build:
    docker:
      - image: docker:latest
    steps:
      - checkout
      - setup_remote_docker
      - run: docker build -t myimage:${CIRCLE_SHA1} .
```

### 738. Azure DevOps: Build task
```yaml
steps:
- task: Docker@2
  inputs:
    command: build
    repository: myimage
    tags: $(Build.BuildId)
```

### 739. Travis CI: Build configuration
```yaml
services:
  - docker
script:
  - docker build -t myimage:$TRAVIS_BUILD_NUMBER .
  - docker push myimage:$TRAVIS_BUILD_NUMBER
```

### 740. Drone CI: Build pipeline
```yaml
kind: pipeline
name: default
steps:
  - name: build
    image: plugins/docker
    settings:
      repo: myimage
      tags: ${DRONE_COMMIT_SHA}
```

### 741. Build with kaniko in Kubernetes
```yaml
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:latest
    args:
      - "--dockerfile=Dockerfile"
      - "--context=git://github.com/user/repo"
      - "--destination=myregistry/myimage:latest"
```

### 742. Use BuildKit in CI
```
$ export DOCKER_BUILDKIT=1
$ docker build --progress=plain -t myimage .
```

### 743. Run tests in Docker during CI
```
$ docker run --rm myimage npm test
```

### 744. Security scan in CI with Trivy
```
$ docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image myimage:latest
```

### 745. Lint Dockerfile in CI
```
$ docker run --rm -i hadolint/hadolint < Dockerfile
```

### 746. Push to multiple registries
```
$ docker tag myimage:latest registry1.com/myimage:latest
$ docker tag myimage:latest registry2.com/myimage:latest
$ docker push registry1.com/myimage:latest
$ docker push registry2.com/myimage:latest
```

### 747. Conditional build based on branch
```bash
if [ "$BRANCH" == "main" ]; then
  docker build -t myimage:production .
else
  docker build -t myimage:dev .
fi
```

### 748. Tag image with version from file
```
$ VERSION=$(cat VERSION)
$ docker build -t myimage:$VERSION .
```

### 749. Clean up after CI build
```
$ docker system prune -af
$ docker volume prune -f
```

### 750. Parallel builds in CI
```
$ docker build -t service1 ./service1 &
$ docker build -t service2 ./service2 &
$ wait
```

---

## 11. DOCKER DESKTOP FEATURES

### 751. Check Docker Desktop version
```
$ docker version --format '{{.Server.Platform.Name}}'
```

### 752. Configure Docker Desktop resources (via UI or settings)
```json
{
  "cpus": 4,
  "memory": 8192,
  "swap": 2048
}
```

### 753. Enable Kubernetes in Docker Desktop
```
$ kubectl config get-contexts
```

### 754. Switch to Docker Desktop Kubernetes
```
$ kubectl config use-context docker-desktop
```

### 755. Access Docker Desktop VM (Mac)
```
$ docker run -it --privileged --pid=host debian nsenter -t 1 -m -u -n -i sh
```

### 756. Reset Docker Desktop
```
# Via UI: Troubleshoot -> Reset to factory defaults
```

### 757. Configure file sharing paths
```
# Settings -> Resources -> File Sharing
```

### 758. Enable Docker Desktop extensions
```
$ docker extension ls
```

### 759. Install Docker Desktop extension
```
$ docker extension install vendor/extension-name
```

### 760. Update extension
```
$ docker extension update vendor/extension-name
```

### 761. Remove extension
```
$ docker extension rm vendor/extension-name
```

### 762. Configure HTTP proxy
```json
{
  "proxies": {
    "http": "http://proxy.example.com:8080",
    "https": "http://proxy.example.com:8080"
  }
}
```

### 763. Enable experimental features
```json
{
  "experimental": true
}
```

### 764. Configure Docker Desktop daemon
```json
{
  "debug": true,
  "log-level": "debug"
}
```

### 765. Use WSL2 backend (Windows)
```
# Settings -> General -> Use WSL 2 based engine
```

### 766. Access Docker from WSL2
```
$ docker context use default
```

### 767. Configure Windows container mode
```
# Right-click Docker icon -> Switch to Windows containers
```

### 768. Enable Docker CLI plugins
```
$ docker scan --version
```

### 769. Run vulnerability scan (Docker Desktop)
```
$ docker scan myimage:latest
```

### 770. View Docker Desktop logs
```
# Troubleshoot -> View logs
```

---

## 12. CONTAINER ORCHESTRATION PATTERNS

### 771. Sidecar pattern: Logging container
```yaml
version: '3.8'
services:
  app:
    image: myapp
    volumes:
      - logs:/var/log
  logger:
    image: fluent/fluentd
    volumes:
      - logs:/var/log:ro
volumes:
  logs:
```

### 772. Ambassador pattern: Proxy container
```yaml
services:
  app:
    image: myapp
  ambassador:
    image: nginx
    links:
      - app
```

### 773. Adapter pattern: Monitoring exporter
```yaml
services:
  app:
    image: myapp
  exporter:
    image: prom/node-exporter
    volumes:
      - /proc:/host/proc:ro
```

### 774. Init container pattern with Docker Compose
```yaml
services:
  init:
    image: busybox
    command: sh -c "echo Initializing && sleep 5"
  app:
    image: myapp
    depends_on:
      - init
```

### 775. Blue-Green deployment preparation
```
$ docker service create --name blue --replicas 3 myapp:v1
$ docker service create --name green --replicas 3 myapp:v2
```

### 776. Switch traffic (blue to green)
```
$ docker service update --label-add version=green proxy
```

### 777. Canary deployment
```
$ docker service scale blue=9 green=1
```

### 778. Gradually shift traffic
```
$ docker service scale blue=5 green=5
```

### 779. Rolling update configuration
```
$ docker service create --update-delay 10s --update-parallelism 2 --replicas 10 myapp
```

### 780. Rollback on failure
```
$ docker service update --rollback-on-failure --update-failure-action rollback myapp
```

### 781. Health check pattern
```yaml
version: '3.8'
services:
  app:
    image: myapp
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
```

### 782. Circuit breaker with retry
```yaml
deploy:
  restart_policy:
    condition: on-failure
    delay: 5s
    max_attempts: 3
    window: 120s
```

### 783. Service mesh sidecar injection
```
$ docker run -d --network=mesh myapp
$ docker run -d --network=container:myapp envoyproxy/envoy
```

### 784. Multi-stage deployment
```bash
docker service create --name stage1 myapp:v1
docker service create --name stage2 myapp:v2
docker service create --name stage3 myapp:v3
```

### 785. Feature flag container
```
$ docker run -e FEATURE_NEW_UI=true -e FEATURE_BETA=false myapp
```

---

## 13. ADVANCED SECURITY & SCANNING

### 786. Scan image with Trivy
```
$ docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image myimage:latest
```

### 787. Scan with severity filter
```
$ docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image --severity HIGH,CRITICAL myimage:latest
```

### 788. Generate SBOM (Software Bill of Materials)
```
$ docker sbom myimage:latest
```

### 789. Scan with Clair
```
$ docker run -d --name clair quay.io/coreos/clair:latest
$ clairctl analyze myimage:latest
```

### 790. Scan with Anchore
```
$ docker run -d --name anchore anchore/inline-scan:latest
$ anchore-cli image add myimage:latest
$ anchore-cli image vuln myimage:latest all
```

### 791. Sign image with Notary
```
$ export DOCKER_CONTENT_TRUST=1
$ docker push myimage:latest
```

### 792. Verify image signature
```
$ docker trust inspect --pretty myimage:latest
```

### 793. Add signer to image
```
$ docker trust signer add --key cert.pem signer1 myimage
```

### 794. Remove signer
```
$ docker trust signer remove signer1 myimage
```

### 795. Rotate signing keys
```
$ docker trust key rotate myimage
```

### 796. Enable image verification
```
{
  "content-trust": {
    "mode": "enforced"
  }
}
```

### 797. Run rootless Docker
```
$ dockerd-rootless-setuptool.sh install
$ systemctl --user start docker
```

### 798. Check if rootless mode is active
```
$ docker context ls
```

### 799. Configure AppArmor profile
```
$ docker run --security-opt apparmor=docker-nginx nginx
```

### 800. Create custom AppArmor profile
```
$ aa-genprof docker-custom
```

### 801. Load AppArmor profile
```
$ sudo apparmor_parser -r -W /etc/apparmor.d/docker-custom
```

### 802. Run with custom seccomp profile
```
$ docker run --security-opt seccomp=custom-profile.json nginx
```

### 803. Generate seccomp profile
```
$ docker run --rm -it --security-opt seccomp=unconfined nginx
# Use oci-seccomp-bpf-hook to generate profile
```

### 804. Audit container security
```
$ docker run --rm -v /var/run/docker.sock:/var/run/docker.sock docker/docker-bench-security
```

### 805. Check for secrets in image
```
$ docker run --rm -v /var/run/docker.sock:/var/run/docker.sock trufflesecurity/trufflehog:latest image myimage:latest
```

### 806. Scan filesystem for malware
```
$ docker run --rm -v /:/rootfs:ro aquasec/clamscan /rootfs
```

### 807. Enable SELinux labels
```
$ docker run --security-opt label=type:svirt_apache_t nginx
```

### 808. Disable SELinux label separation
```
$ docker run --security-opt label=disable nginx
```

### 809. Set MLS/MCS labels
```
$ docker run --security-opt label=level:s0:c100,c200 nginx
```

### 810. Use gVisor for runtime isolation
```
$ docker run --runtime=runsc nginx
```

### 811. Configure Kata Containers for VM isolation
```
$ docker run --runtime=kata-runtime nginx
```

### 812. Enable user namespace in daemon
```
{
  "userns-remap": "default"
}
```

### 813. Map specific user/group
```
{
  "userns-remap": "testuser:testgroup"
}
```

### 814. Disable privilege escalation
```
$ docker run --security-opt=no-new-privileges nginx
```

### 815. Drop all capabilities
```
$ docker run --cap-drop=ALL nginx
```

### 816. Add only required capability
```
$ docker run --cap-drop=ALL --cap-add=NET_BIND_SERVICE nginx
```

### 817. List available capabilities
```
$ docker run --rm alpine cat /proc/1/status | grep Cap
```

### 818. Mask sensitive paths
```
$ docker run --tmpfs /run --tmpfs /tmp nginx
```

### 819. Use read-only containers
```
$ docker run --read-only --tmpfs /tmp nginx
```

### 820. Protect Docker socket
```
$ sudo chmod 660 /var/run/docker.sock
$ sudo chown root:docker /var/run/docker.sock
```

---

## 14. DOCKER EXTENSIONS & TOOLS

### 821. Install Docker Compose V2
```
$ docker compose version
```

### 822. Use Docker Compose V2 commands
```
$ docker compose up -d
```

### 823. Install docker-slim
```
$ docker run -it --rm -v /var/run/docker.sock:/var/run/docker.sock dslim/docker-slim build myimage
```

### 824. Optimize image with docker-slim
```
$ docker-slim build --target myimage:latest --tag myimage:slim
```

### 825. Install dive for layer analysis
```
$ docker run --rm -it -v /var/run/docker.sock:/var/run/docker.sock wagoodman/dive:latest myimage
```

### 826. Analyze image with dive
```
$ dive myimage:latest
```

### 827. Install ctop for container monitoring
```
$ docker run --rm -ti -v /var/run/docker.sock:/var/run/docker.sock quay.io/vektorlab/ctop:latest
```

### 828. Install lazydocker for TUI management
```
$ docker run --rm -it -v /var/run/docker.sock:/var/run/docker.sock lazyteam/lazydocker
```

### 829. Use Portainer for web UI
```
$ docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer-ce
```

### 830. Install Watchtower for auto-updates
```
$ docker run -d --name watchtower -v /var/run/docker.sock:/var/run/docker.sock containrrr/watchtower
```

### 831. Configure Watchtower schedule
```
$ docker run -d -e WATCHTOWER_SCHEDULE="0 0 4 * * *" containrrr/watchtower
```

### 832. Install Docker Registry UI
```
$ docker run -d -p 8080:80 -e REGISTRY_URL=http://registry:5000 joxit/docker-registry-ui:latest
```

### 833. Use Dockle for image linting
```
$ docker run --rm -v /var/run/docker.sock:/var/run/docker.sock goodwithtech/dockle:latest myimage
```

### 834. Install Hadolint for Dockerfile linting
```
$ docker run --rm -i hadolint/hadolint < Dockerfile
```

### 835. Use Container Structure Test
```
$ container-structure-test test --image myimage --config test.yaml
```

### 836. Install Skopeo for image operations
```
$ skopeo copy docker://nginx:latest docker://myregistry/nginx:latest
```

### 837. Inspect remote image with Skopeo
```
$ skopeo inspect docker://nginx:latest
```

### 838. Install Buildah for OCI builds
```
$ buildah bud -t myimage .
```

### 839. Install Podman as Docker alternative
```
$ podman run -d nginx
```

### 840. Use Docker Bench for security
```
$ docker run --rm --net host --pid host --cap-add audit_control -v /var/lib:/var/lib -v /var/run/docker.sock:/var/run/docker.sock -v /etc:/etc --label docker_bench_security docker/docker-bench-security
```

---

## 15. PRODUCTION BEST PRACTICES

### 841. Use specific image tags, not latest
```
$ docker pull nginx:1.21.6
```

### 842. Multi-stage build for smaller images
```
FROM node:16 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
```

### 843. Use .dockerignore file
```
node_modules
.git
*.md
.env
```

### 844. Minimize layer count
```
RUN apt-get update && apt-get install -y \
    package1 \
    package2 \
    && rm -rf /var/lib/apt/lists/*
```

### 845. Run as non-root user
```
FROM node:16-alpine
RUN addgroup -g 1001 -S nodejs && adduser -S nodejs -u 1001
USER nodejs
```

### 846. Use COPY instead of ADD
```
COPY package.json /app/
```

### 847. Set proper WORKDIR
```
WORKDIR /app
COPY . .
```

### 848. Use HEALTHCHECK instruction
```
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost/ || exit 1
```

### 849. Set resource limits in production
```
$ docker run -m 512m --cpus=1 --pids-limit=100 myapp
```

### 850. Use logging driver for centralized logs
```
$ docker run --log-driver=fluentd --log-opt fluentd-address=logs.example.com:24224 myapp
```

### 851. Implement graceful shutdown
```
FROM node:16
STOPSIGNAL SIGTERM
CMD ["node", "server.js"]
```

### 852. Use init process for signal handling
```
$ docker run --init myapp
```

### 853. Set proper labels for metadata
```
LABEL maintainer="team@example.com"
LABEL version="1.0"
LABEL description="Production app"
```

### 854. Scan images before deployment
```
$ docker scan myimage:latest
```

### 855. Use distroless base images
```
FROM gcr.io/distroless/nodejs:16
COPY --from=builder /app /app
CMD ["server.js"]
```

### 856. Implement health endpoints
```
app.get('/health', (req, res) => {
  res.status(200).send('OK');
});
```

### 857. Use environment-specific configs
```
$ docker run -e NODE_ENV=production -e DB_HOST=prod-db myapp
```

### 858. Implement proper logging
```
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log
```

### 859. Use secrets for sensitive data
```
$ docker secret create db_password ./password.txt
$ docker service create --secret db_password myapp
```

### 860. Implement proper monitoring
```
$ docker run -d --name prometheus -p 9090:9090 prom/prometheus
```

### 861. Set up alerts for containers
```
$ docker run -d --name alertmanager -p 9093:9093 prom/alertmanager
```

### 862. Use container orchestration for HA
```
$ docker service create --replicas 3 --update-delay 10s myapp
```

### 863. Implement proper backup strategy
```
$ docker run --rm -v dbdata:/data -v $(pwd):/backup ubuntu tar czf /backup/backup.tar.gz /data
```

### 864. Use volumes for persistent data
```
$ docker run -v db-data:/var/lib/postgresql/data postgres
```

### 865. Implement rate limiting
```
$ docker run --pids-limit 200 --ulimit nofile=1024:2048 myapp
```

### 866. Use CDN for static assets
```
# Build and upload static files separately
$ docker run --rm -v $(pwd)/dist:/dist myapp
```

### 867. Implement circuit breakers
```
restart_policy:
  condition: on-failure
  max_attempts: 3
```

### 868. Use APM for performance monitoring
```
$ docker run -e ELASTIC_APM_SERVER_URL=http://apm:8200 myapp
```

### 869. Implement proper caching
```
RUN --mount=type=cache,target=/root/.cache/pip pip install -r requirements.txt
```

### 870. Use vulnerability scanning in CI/CD
```
$ docker run aquasec/trivy image --exit-code 1 --severity CRITICAL myimage
```

### 871. Implement zero-downtime deployments
```
$ docker service update --update-parallelism 1 --update-delay 10s myapp
```

### 872. Use proper networking for microservices
```
$ docker network create --driver overlay --attachable microservices
```

### 873. Implement service mesh
```
$ docker service create --network mesh --name myapp envoy-proxy
```

### 874. Set up distributed tracing
```
$ docker run -d -p 9411:9411 openzipkin/zipkin
```

### 875. Use configuration management
```
$ docker config create app-config ./config.json
$ docker service create --config app-config myapp
```

### 876. Implement proper DNS resolution
```
$ docker run --dns=10.0.0.2 --dns-search=example.com myapp
```

### 877. Use automated restarts
```
$ docker run -d --restart=unless-stopped myapp
```

### 878. Implement log rotation
```
$ docker run --log-opt max-size=10m --log-opt max-file=3 myapp
```

### 879. Use proper time synchronization
```
$ docker run -v /etc/localtime:/etc/localtime:ro myapp
```

### 880. Implement proper shutdown hooks
```javascript
process.on('SIGTERM', () => {
  server.close(() => {
    process.exit(0);
  });
});
```

### 881. Monitor container metrics
```
$ docker stats --format "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}"
```

### 882. Use admission controllers
```
$ docker run --security-opt=no-new-privileges --cap-drop=ALL myapp
```

### 883. Implement proper tagging strategy
```
$ docker tag myapp:latest myapp:v1.2.3
$ docker tag myapp:latest myapp:production
```

### 884. Use image signing
```
$ export DOCKER_CONTENT_TRUST=1
$ docker push myapp:latest
```

### 885. Implement disaster recovery
```
$ docker run --rm -v data:/data -v backup:/backup ubuntu tar xzf /backup/latest.tar.gz -C /data
```

### 886. Use proper security headers
```
docker run -e SECURITY_HEADERS=true nginx
```

### 887. Implement rate limiting at proxy
```
$ docker run -e RATE_LIMIT=100 nginx-proxy
```

### 888. Use automated security updates
```
$ docker run -d containrrr/watchtower --schedule "0 0 2 * * *"
```

### 889. Implement proper access controls
```
$ docker run --user 1000:1000 --read-only myapp
```

### 890. Monitor Docker daemon health
```
$ docker system events --filter 'type=daemon'
```

### 891. Use proper SSL/TLS
```
$ docker run -v /etc/letsencrypt:/certs:ro nginx
```

### 892. Implement request tracing
```
$ docker run -e JAEGER_AGENT_HOST=jaeger myapp
```

### 893. Use proper environment isolation
```
$ docker network create --internal backend
```

### 894. Implement automated testing
```
$ docker run --rm myapp npm test
```

### 895. Use canary releases
```
$ docker service update --image myapp:v2 --update-parallelism 1 --update-delay 60s myapp
```

### 896. Implement A/B testing
```
$ docker service create --label version=a --replicas 5 myapp:v1
$ docker service create --label version=b --replicas 5 myapp:v2
```

### 897. Monitor application errors
```
$ docker logs --tail 100 myapp | grep ERROR
```

### 898. Use proper dependency injection
```
$ docker run -e DATABASE_URL=postgres://db:5432 myapp
```

### 899. Implement circuit breaking
```
restart_policy:
  condition: on-failure
  delay: 5s
  max_attempts: 3
  window: 120s
```

### 900. Use proper versioning
```
$ docker build -t myapp:$(git rev-parse --short HEAD) .
```

### 901. Implement automated rollbacks
```
$ docker service update --rollback myapp
```

### 902. Use proper database migrations
```
$ docker run --rm myapp npm run migrate
```

### 903. Implement feature toggles
```
$ docker run -e FEATURE_NEW_UI=true myapp
```

### 904. Use proper caching strategies
```
RUN --mount=type=cache,target=/var/cache/apt apt-get update
```

### 905. Monitor network performance
```
$ docker exec myapp ping -c 10 service2
```

### 906. Implement proper queuing
```
$ docker run -d --name redis redis:alpine
```

### 907. Use worker containers
```
$ docker run -d --name worker myapp npm run worker
```

### 908. Implement job scheduling
```
$ docker run -d --name scheduler myapp npm run scheduler
```

### 909. Use proper session management
```
$ docker run -e SESSION_STORE=redis://redis:6379 myapp
```

### 910. Implement API versioning
```
$ docker run -e API_VERSION=v2 myapp
```

### 911. Use proper CORS configuration
```
$ docker run -e CORS_ORIGIN=https://example.com myapp
```

### 912. Implement request validation
```
$ docker run -e VALIDATE_REQUESTS=true myapp
```

### 913. Use proper error handling
```
$ docker run -e ERROR_HANDLING=production myapp
```

### 914. Implement audit logging
```
$ docker run --log-driver=syslog --log-opt tag="myapp-audit" myapp
```

### 915. Use proper authentication
```
$ docker run -e AUTH_PROVIDER=oauth2 myapp
```

### 916. Implement authorization
```
$ docker run -e RBAC_ENABLED=true myapp
```

### 917. Use proper SSL termination
```
$ docker run -p 443:443 -v /etc/ssl:/ssl nginx
```

### 918. Implement load balancing
```
$ docker run -d --name lb -p 80:80 haproxy
```

### 919. Use sticky sessions
```
$ docker run -e SESSION_AFFINITY=true load-balancer
```

### 920. Implement connection pooling
```
$ docker run -e DB_POOL_SIZE=20 myapp
```

### 921. Use proper timeout settings
```
$ docker run -e REQUEST_TIMEOUT=30s myapp
```

### 922. Implement retry logic
```
$ docker run -e MAX_RETRIES=3 myapp
```

### 923. Use proper compression
```
$ docker run -e GZIP_ENABLED=true nginx
```

### 924. Implement CDN integration
```
$ docker run -e CDN_URL=https://cdn.example.com myapp
```

### 925. Use proper asset versioning
```
$ docker run -e ASSET_VERSION=$(git rev-parse --short HEAD) myapp
```

### 926. Implement cache busting
```
$ docker run -e CACHE_BUST=true myapp
```

### 927. Use proper database indexing
```
$ docker exec db psql -c "CREATE INDEX idx_user_email ON users(email);"
```

### 928. Implement query optimization
```
$ docker run -e DB_QUERY_CACHE=true myapp
```

### 929. Use proper connection strings
```
$ docker run -e DATABASE_URL=postgres://user:pass@db:5432/dbname myapp
```

### 930. Implement database replication
```
$ docker run -e DB_REPLICA_HOST=replica.example.com myapp
```

### 931. Use read replicas
```
$ docker run -e DB_READ_HOST=read-replica myapp
```

### 932. Implement sharding
```
$ docker run -e DB_SHARD_KEY=user_id myapp
```

### 933. Use proper backup verification
```
$ docker run --rm -v backup:/backup myapp verify-backup
```

### 934. Implement incremental backups
```
$ docker run --rm -v data:/data backup-tool incremental
```

### 935. Use point-in-time recovery
```
$ docker run --rm -v data:/data restore-tool --time "2024-01-01 12:00:00"
```

### 936. Implement proper monitoring dashboards
```
$ docker run -d -p 3000:3000 grafana/grafana
```

### 937. Use proper alerting rules
```
$ docker run -v alerting.yml:/etc/prometheus/alerting.yml prom/prometheus
```

### 938. Implement SLA monitoring
```
$ docker run -e SLA_TARGET=99.9 monitoring-agent
```

### 939. Use proper capacity planning
```
$ docker stats --no-stream --format "table {{.Container}}\t{{.CPUPerc}}\t{{.MemPerc}}"
```

### 940. Implement auto-scaling
```
$ docker service update --replicas-max-per-node 3 myapp
```

### 941. Use proper cost optimization
```
$ docker run --memory=256m --cpus=0.5 myapp
```

### 942. Implement resource quotas
```
$ docker run --memory-reservation=128m --memory=256m myapp
```

### 943. Use spot instances for non-critical workloads
```
# Cloud provider specific
```

### 944. Implement proper tagging for cost allocation
```
$ docker run --label cost-center=engineering --label project=webapp myapp
```

### 945. Use multi-tenancy isolation
```
$ docker run --network=tenant1-net --label tenant=tenant1 myapp
```

### 946. Implement proper data encryption
```
$ docker run -e ENCRYPTION_KEY=/run/secrets/encryption-key myapp
```

### 947. Use encrypted volumes
```
$ docker volume create --opt encrypted=true secure-data
```

### 948. Implement secrets rotation
```
$ docker secret create db_password_v2 ./new_password.txt
$ docker service update --secret-rm db_password --secret-add db_password_v2 myapp
```

### 949. Use proper compliance monitoring
```
$ docker run --rm compliance-scanner scan myapp
```

### 950. Implement audit trails
```
$ docker run --log-driver=splunk --log-opt splunk-token=XXX myapp
```

### 951. Use proper data retention policies
```
$ docker run --log-opt max-size=100m --log-opt max-file=10 myapp
```

### 952. Implement GDPR compliance
```
$ docker run -e DATA_RETENTION_DAYS=30 myapp
```

### 953. Use proper access logging
```
$ docker run -v access-logs:/var/log/nginx nginx
```

### 954. Implement security scanning in registry
```
$ docker scan --severity high myregistry/myapp:latest
```

### 955. Use proper image provenance
```
$ docker buildx build --provenance=true --sbom=true -t myapp .
```

### 956. Implement supply chain security
```
$ docker trust inspect --pretty myapp:latest
```

### 957. Use verified base images
```
FROM gcr.io/distroless/base-debian11
```

### 958. Implement proper patch management
```
$ docker run -d watchtower --schedule "0 0 3 * * *"
```

### 959. Use security benchmarks
```
$ docker run --rm docker/docker-bench-security
```

### 960. Implement penetration testing
```
$ docker run --rm -v $(pwd):/zap/wrk owasp/zap2docker-stable zap-baseline.py -t http://myapp
```

### 961. Use proper firewall rules
```
$ iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

### 962. Implement DDoS protection
```
$ docker run -e RATE_LIMIT=1000 cloudflare/cloudflared
```

### 963. Use proper WAF configuration
```
$ docker run -d -p 80:80 owasp/modsecurity-crs
```

### 964. Implement bot protection
```
$ docker run -e BOT_PROTECTION=true nginx-plus
```

### 965. Use proper GeoIP filtering
```
$ docker run -e GEOIP_ALLOW=US,CA nginx
```

### 966. Implement content filtering
```
$ docker run -e CONTENT_FILTER=strict proxy
```

### 967. Use proper SSL/TLS versions
```
$ docker run -e SSL_PROTOCOLS="TLSv1.2 TLSv1.3" nginx
```

### 968. Implement certificate pinning
```
$ docker run -e CERT_PINNING=true myapp
```

### 969. Use proper cipher suites
```
$ docker run -e SSL_CIPHERS="HIGH:!aNULL:!MD5" nginx
```

### 970. Implement HSTS headers
```
$ docker run -e HSTS_MAX_AGE=31536000 nginx
```

### 971. Use CSP headers
```
$ docker run -e CSP_POLICY="default-src 'self'" nginx
```

### 972. Implement XSS protection
```
$ docker run -e XSS_PROTECTION=true nginx
```

### 973. Use frame options
```
$ docker run -e FRAME_OPTIONS=DENY nginx
```

### 974. Implement referrer policy
```
$ docker run -e REFERRER_POLICY=no-referrer nginx
```

### 975. Use permissions policy
```
$ docker run -e PERMISSIONS_POLICY="geolocation=()" nginx
```

### 976. Implement subresource integrity
```
<script src="app.js" integrity="sha384-xxx"></script>
```

### 977. Use proper CORS policies
```
$ docker run -e CORS_ALLOW_ORIGIN=https://example.com nginx
```

### 978. Implement cookie security
```
$ docker run -e COOKIE_SECURE=true -e COOKIE_HTTPONLY=true myapp
```

### 979. Use SameSite cookies
```
$ docker run -e COOKIE_SAMESITE=strict myapp
```

### 980. Implement CSRF protection
```
$ docker run -e CSRF_ENABLED=true myapp
```

### 981. Use proper session timeout
```
$ docker run -e SESSION_TIMEOUT=1800 myapp
```

### 982. Implement account lockout
```
$ docker run -e MAX_LOGIN_ATTEMPTS=5 myapp
```

### 983. Use MFA enforcement
```
$ docker run -e MFA_REQUIRED=true myapp
```

### 984. Implement password policies
```
$ docker run -e MIN_PASSWORD_LENGTH=12 myapp
```

### 985. Use password hashing
```
$ docker run -e PASSWORD_HASH_ALGORITHM=bcrypt myapp
```

### 986. Implement rate limiting per user
```
$ docker run -e USER_RATE_LIMIT=100 myapp
```

### 987. Use IP whitelisting
```
$ docker run -e ALLOWED_IPS=192.168.1.0/24 myapp
```

### 988. Implement geo-blocking
```
$ docker run -e BLOCKED_COUNTRIES=CN,RU myapp
```

### 989. Use threat intelligence feeds
```
$ docker run -e THREAT_FEED_URL=https://feeds.example.com myapp
```

### 990. Implement anomaly detection
```
$ docker run -e ANOMALY_DETECTION=true monitoring-agent
```

### 991. Use behavioral analysis
```
$ docker run -e BEHAVIOR_ANALYSIS=true security-agent
```

### 992. Implement fraud detection
```
$ docker run -e FRAUD_DETECTION=true payment-service
```

### 993. Use machine learning for security
```
$ docker run -e ML_SECURITY=true ml-agent
```

### 994. Implement zero-trust architecture
```
$ docker run -e ZERO_TRUST=true service-mesh
```

### 995. Use micro-segmentation
```
$ docker network create --internal segment1
$ docker network create --internal segment2
```

### 996. Implement least privilege access
```
$ docker run --user 1000:1000 --cap-drop=ALL myapp
```

### 997. Use just-in-time access
```
$ docker run -e JIT_ACCESS=true privileged-app
```

### 998. Implement continuous compliance
```
$ docker run -d compliance-agent --continuous
```

### 999. Use automated remediation
```
$ docker run -e AUTO_REMEDIATE=true security-agent
```

### 1000. Implement comprehensive observability
```
$ docker run -d -p 3000:3000 \
  -e GF_INSTALL_PLUGINS=grafana-piechart-panel \
  grafana/grafana
```

---

## FINAL SUMMARY

This Part 2 advanced course covered:
- **490 additional commands** (511-1000)
- BuildKit and advanced build techniques
- Multi-architecture image creation
- Docker contexts and plugins
- Container runtimes (runc, kata, gVisor)
- Performance benchmarking
- Docker API and SDKs
- Advanced networking patterns
- Storage drivers and optimization
- CI/CD integration
- Docker Desktop features
- Container orchestration patterns
- Advanced security and scanning
- Production best practices
- Extensions and ecosystem tools

--- 
[JOIN FOR LIVE CLASSES AND SUPPORT](https://t.me/efxtv) 

[![Buy Me A Coffee](https://img.buymeacoffee.com/button-api/?text=Buy%20me%20a%20coffee&emoji=&slug=efxtv&button_colour=FFDD00&font_colour=000000&font_family=Cookie&outline_colour=000000&coffee_colour=ffffff)](https://buymeacoffee.com/efxtv)
