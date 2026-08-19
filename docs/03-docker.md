# 03. Docker 점검 및 컨테이너 운영

> [← 목록으로](../README.md)

---

- Docker 버전 확인
```bash
(사용자) codyssey % docker --version
```

```
Docker version 28.5.2, build ecc6942
```
<br>
<br>

- Docker 데몬 동작 여부 확인
```bash
(사용자) codyssey % docker info
```

```
Client:
 Version:    28.5.2
 Context:    orbstack
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.29.1
    Path:     /Users/(사용자)/.docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.40.3
    Path:     /Users/(사용자)/.docker/cli-plugins/docker-compose

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 0
 Server Version: 28.5.2
 Storage Driver: overlay2
  Backing Filesystem: btrfs
  Supports d_type: true
  Using metacopy: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 CDI spec directories:
  /etc/cdi
  /var/run/cdi
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 1c4457e00facac03ce1d75f7b6777a7a851e5c41
 runc version: d842d7719497cc3b774fd71620278ac9e17710e0
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.17.8-orbstack-00308-g8f9c941121b1
 Operating System: OrbStack
 OSType: linux
 Architecture: x86_64
 CPUs: 6
 Total Memory: 15.67GiB
 Name: orbstack
 ID: (생략)
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  ::1/128
  127.0.0.0/8
 Live Restore Enabled: false
 Product License: Community Engine
 Default Address Pools:
   Base: 192.168.97.0/24, Size: 24
   Base: 192.168.107.0/24, Size: 24
   Base: 192.168.117.0/24, Size: 24
   Base: 192.168.147.0/24, Size: 24
   Base: 192.168.148.0/24, Size: 24
   Base: 192.168.155.0/24, Size: 24
   Base: 192.168.156.0/24, Size: 24
   Base: 192.168.158.0/24, Size: 24
   Base: 192.168.163.0/24, Size: 24
   Base: 192.168.164.0/24, Size: 24
   Base: 192.168.165.0/24, Size: 24
   Base: 192.168.166.0/24, Size: 24
   Base: 192.168.167.0/24, Size: 24
   Base: 192.168.171.0/24, Size: 24
   Base: 192.168.172.0/24, Size: 24
   Base: 192.168.181.0/24, Size: 24
   Base: 192.168.183.0/24, Size: 24
   Base: 192.168.186.0/24, Size: 24
   Base: 192.168.207.0/24, Size: 24
   Base: 192.168.214.0/24, Size: 24
   Base: 192.168.215.0/24, Size: 24
   Base: 192.168.216.0/24, Size: 24
   Base: 192.168.223.0/24, Size: 24
   Base: 192.168.227.0/24, Size: 24
   Base: 192.168.228.0/24, Size: 24
   Base: 192.168.229.0/24, Size: 24
   Base: 192.168.237.0/24, Size: 24
   Base: 192.168.239.0/24, Size: 24
   Base: 192.168.242.0/24, Size: 24
   Base: 192.168.247.0/24, Size: 24
   Base: fd07:b51a:cc66:d000::/56, Size: 64

WARNING: DOCKER_INSECURE_NO_IPTABLES_RAW is set
```
<br>
<br>

- 이미지 다운로드
```bash
(사용자) codyssey % docker pull nginx
```

```
Using default tag: latest
latest: Pulling from library/nginx
26c307b5e35a: Pull complete 
3c55dc422a81: Pull complete 
d84ae7b21412: Pull complete 
c0df8d325117: Pull complete 
b8b80b9bc028: Pull complete 
f5de6e85ac74: Pull complete 
5a4222b844e8: Pull complete 
Digest: sha256:8541484afbc9c8a5a8a99b379568ebbc957f658583ec9448fc43104229c03cf8
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
```
<br>
<br>

- 이미지 목록 확인
```bash
(사용자) codyssey % docker images
```

```
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
nginx        latest    5253dc86cc93   2 weeks ago   161MB
```
<br>
<br>

- 컨테이너 실행
```bash
(사용자) codyssey % docker run -d -p 8090:80 --name nginx-test nginx
```

```
f5b49420fb4cc519420ebae186562f4a4be8734185b0dc141903588f7a184dbd
```
<br>
<br>

- 리소스 확인
```bash
(사용자) codyssey % docker stats --no-stream
```

```
CONTAINER ID   NAME         CPU %     MEM USAGE / LIMIT     MEM %     NET I/O         BLOCK I/O         PIDS
f5b49420fb4c   nginx-test   0.00%     10.01MiB / 15.67GiB   0.06%     1.66kB / 126B   16.7MB / 8.19kB   7
```
<br>
<br>

- 로그 확인
```bash
(사용자) codyssey % docker logs nginx-test
```

```
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/08/19 04:47:53 [notice] 1#1: using the "epoll" event method
2026/08/19 04:47:53 [notice] 1#1: nginx/1.31.3
2026/08/19 04:47:53 [notice] 1#1: built by gcc 14.2.0 (Debian 14.2.0-19) 
2026/08/19 04:47:53 [notice] 1#1: OS: Linux 6.17.8-orbstack-00308-g8f9c941121b1
2026/08/19 04:47:53 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 20480:1048576
2026/08/19 04:47:53 [notice] 1#1: start worker processes
2026/08/19 04:47:53 [notice] 1#1: start worker process 29
2026/08/19 04:47:53 [notice] 1#1: start worker process 30
2026/08/19 04:47:53 [notice] 1#1: start worker process 31
2026/08/19 04:47:53 [notice] 1#1: start worker process 32
2026/08/19 04:47:53 [notice] 1#1: start worker process 33
2026/08/19 04:47:53 [notice] 1#1: start worker process 34
```
<br>
<br>

- 컨테이너 중지
```bash
(사용자) codyssey % docker stop nginx-test
```

```
nginx-test
```
<br>
<br>

- 컨테이너 목록 확인
```bash
(사용자) codyssey % docker ps -a
```

```
CONTAINER ID   IMAGE     COMMAND                   CREATED         STATUS                      PORTS     NAMES
f5b49420fb4c   nginx     "/docker-entrypoint.…"   6 minutes ago   Exited (0) 58 seconds ago             nginx-test
```
<br>
<br>

- hello-world 실행 실습
```bash
(사용자) codyssey % docker run hello-world
```

```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
4f55086f7dd0: Pull complete 
Digest: sha256:5dd0d3e6e255913fc30f90b9f2b1d359cc2cbdb48090cc4b65f1676e203243cc
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```
<br>
<br>

- ubuntu 컨테이너 내부 명령
```bash
(사용자) codyssey % docker run -it ubuntu bash
root@[컨테이너ID]:/#
```

```
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
06e9d71331fb: Pull complete 
f3db1cd94078: Pull complete 
Digest: sha256:6df9e8dd1eac389ebfef692c9648449adeb815d01e16e29cd6f3e50fe64ba9a6
Status: Downloaded newer image for ubuntu:latest
```
<br>
<br>

- OS 정보 확인
```bash
root@[컨테이너ID]:/# cat /etc/os-release
```

```
PRETTY_NAME="Ubuntu 26.04 LTS"
NAME="Ubuntu"
VERSION_ID="26.04"
VERSION="26.04 LTS (Resolute Raccoon)"
VERSION_CODENAME=resolute
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=resolute
LOGO=ubuntu-logo
```
<br>
<br>

- 컨테이너 종료/나가기
```bash
root@[컨테이너ID]:/# exit
(사용자) codyssey %
```

```
exit
(사용자) codyssey %
```
<br>
<br>

- 컨테이너 유지 상태로 빠져나오기 (Detach)

```bash
(사용자) codyssey % docker run -it --name detach-test ubuntu bash
root@[컨테이너ID]:/#
```

```
root@378f59e9d4c1:/#
```

<br>

Ctrl + P -> Ctrl + Q

<br>
<br>
<br>

- 컨테이너 종료/유지 차이
  

동작 | 명령 | 컨테이너 상태 | `docker ps`
종료 | `exit` | Exited | 안 보임 (`docker ps -a`에만 보임)
유지 | `Ctrl+P` → `Ctrl+Q` | Up | 보임

`docker run -it ubuntu bash`로 실행하면 bash가 컨테이너의 메인 프로세스가 된다.
`exit`은 그 bash를 끝내는 것이라 메인 프로세스가 사라지고 컨테이너까지 종료된다.
`Ctrl+P → Ctrl+Q`는 터미널 연결만 끊는(detach) 것이라 bash는 계속 살아 있고 컨테이너도 Up 상태로 남는다.
다시 들어갈 때는 `docker attach [이름]` 또는 `docker exec -it [이름] bash`를 사용한다.

```bash
(사용자) codyssey % docker ps -a
```

```
CONTAINER ID   IMAGE         COMMAND                   CREATED          STATUS                      PORTS     NAMES
378f59e9d4c1   ubuntu        "bash"                    2 minutes ago    Up 2 minutes                          detach-test
6a96b50fff5c   ubuntu        "bash"                    6 minutes ago    Exited (0) 3 minutes ago              festive_hugle
e45571cf037d   hello-world   "/hello"                  8 minutes ago    Exited (0) 8 minutes ago              nervous_euler
f5b49420fb4c   nginx         "/docker-entrypoint.…"   15 minutes ago   Exited (0) 10 minutes ago             nginx-test
```
<br>
<br>

```bash
(사용자) codyssey % docker exec -it detach-test bash
```

```
root@378f59e9d4c1:/#
```
<br>
<br>

- 실습 컨테이너 정리
```bash
root@[컨테이너ID]:/# exit
(사용자) codyssey % docker rm -f nginx-test detach-test
```

```
exit
nginx-test
detach-test
```
<br>

이름을 지정하지 않고 실행한 컨테이너(hello-world, ubuntu)는 종료 후에도 남아 있으므로 함께 삭제한다.

```bash
(사용자) codyssey % docker container prune -f
```

```
Deleted Containers:
6a96b50fff5c55bb1c8de1413953c99c52d22940b6453b7841991c91f4577f3e
e45571cf037dc540be45db2a4f097f7606919aec99e5b11d725a4a6d1d8b754b

Total reclaimed space: 2.366kB
```
<br>

- 컨테이너 정리 확인
```bash
(사용자) codyssey % docker ps -a
```

```
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
<br>
<br>

- 이미지 정리 전 목록
```bash
(사용자) codyssey % docker images
```

```
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
ubuntu        latest    af52039db3f8   44 hours ago   100MB
nginx         latest    5253dc86cc93   2 weeks ago    161MB
hello-world   latest    e2ac70e7319a   4 months ago   10.1kB
```
<br>

- 이후 실습에서 사용하지 않는 이미지 삭제
```bash
(사용자) codyssey % docker rmi hello-world
```

```
Untagged: hello-world:latest
Untagged: hello-world@sha256:5dd0d3e6e255913fc30f90b9f2b1d359cc2cbdb48090cc4b65f1676e203243cc
Deleted: sha256:e2ac70e7319a02c5a477f5825259bd118b94e8b02c279c67afa63adab6d8685b
Deleted: sha256:897b3f2a7c1bc2f3d02432f7892fe31c6272c521ad4d70257df624504a3238b4
```
<br>

- 이미지 정리 확인
```bash
(사용자) codyssey % docker images
```

```
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
ubuntu       latest    af52039db3f8   44 hours ago   100MB
nginx        latest    5253dc86cc93   2 weeks ago    161MB
```
<br>
<br>

---

> [← 목록으로](../README.md)
