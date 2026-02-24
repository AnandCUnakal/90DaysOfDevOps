### Task 1: Docker Images
1. Pull the `nginx`, `ubuntu`, and `alpine` images from Docker Hub
2. List all images on your machine — note the sizes
3. Compare `ubuntu` vs `alpine` — why is one much smaller?
4. Inspect an image — what information can you see?
5. Remove an image you no longer need


'''
PS D:\ANDYtws\projects\docker_proj> docker images
REPOSITORY                                  TAG                IMAGE ID       CREATED         SIZE       
ubuntu                                      latest             d1e2e92c075e   2 weeks ago     117MB      
nginx                                       latest             341bf0f3ce6c   2 weeks ago     237MB      
wordcounter                                 latest             b54c82d97796   3 months ago    93.8MB     
kindest/node                                v1.34.0            7416a61b42b1   6 months ago    1.45GB     
hello-world                                 latest             ef54e839ef54   6 months ago    20.3kB     
envoyproxy/envoy                            v1.32.6            16a413173825   9 months ago    224MB      
docker/desktop-cloud-provider-kind          v0.3.0-desktop.3   c321a5c92549   17 months ago   582MB      
docker/desktop-containerd-registry-mirror   v0.0.2             aadc48ce8960   18 months ago   46.4MB     
PS D:\ANDYtws\projects\docker_proj> docker pull alpine
Using default tag: latest
latest: Pulling from library/alpine
589002ba0eae: Pull complete
Digest: sha256:25109184c71bdad752c8312a8623239686a9a2071e8825f20acb8f2198c3f659
Status: Downloaded newer image for alpine:latest
PS D:\ANDYtws\projects\docker_proj> docker images
REPOSITORY                                  TAG                IMAGE ID       CREATED         SIZE       
ubuntu                                      latest             d1e2e92c075e   2 weeks ago     117MB      
nginx                                       latest             341bf0f3ce6c   2 weeks ago     237MB      
alpine                                      latest             25109184c71b   3 weeks ago     13MB       
wordcounter                                 latest             b54c82d97796   3 months ago    93.8MB     
jenkins/jenkins                             lts                f2519b99350f   4 months ago    820MB      
kindest/node                                v1.34.0            7416a61b42b1   6 months ago    1.45GB     
hello-world                                 latest             ef54e839ef54   6 months ago    20.3kB     
envoyproxy/envoy                            v1.32.6            16a413173825   9 months ago    224MB      
docker/desktop-cloud-provider-kind          v0.3.0-desktop.3   c321a5c92549   17 months ago   582MB      
docker/desktop-containerd-registry-mirror   v0.0.2             aadc48ce8960   18 months ago   46.4MB     
PS D:\ANDYtws\projects\docker_proj> docker inspect ubuntu
[
    {
        "Id": "sha256:d1e2e92c075e5ca139d51a140fff46f84315c0fdce203eab2807c7e495eff4f9",
        "RepoTags": [
            "ubuntu:latest"
        ],
        "RepoDigests": [
            "ubuntu@sha256:d1e2e92c075e5ca139d51a140fff46f84315c0fdce203eab2807c7e495eff4f9"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2026-02-10T16:49:57.226767398Z",
        "DockerVersion": "",
        "Author": "",
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 29737017,
        "GraphDriver": {
            "Data": null,
            "Name": "overlayfs"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:efafae78d70c98626c521c246827389128e7d7ea442db31bc433934647f0c791"
            ]
        },
        "Metadata": {
            "LastTagTime": "2026-02-24T15:17:31.160430665Z"
        },
        "Descriptor": {
            "mediaType": "application/vnd.oci.image.index.v1+json",
            "digest": "sha256:d1e2e92c075e5ca139d51a140fff46f84315c0fdce203eab2807c7e495eff4f9",
            "size": 6688
        },
        "Config": {
            "Cmd": [
                "/bin/bash"
            ],
            "Entrypoint": null,
            "Env": [
            ],
            "Labels": {
                "org.opencontainers.image.ref.name": "ubuntu",
                "org.opencontainers.image.version": "24.04"
            },
            "OnBuild": null,
            "User": "",
            "Volumes": null,
            "WorkingDir": ""
        }
    }
]
PS D:\ANDYtws\projects\docker_proj> docker inspect alpine
[
    {
        "Id": "sha256:25109184c71bdad752c8312a8623239686a9a2071e8825f20acb8f2198c3f659",
        "RepoTags": [
            "alpine:latest"
        ],
        "RepoDigests": [
            "alpine@sha256:25109184c71bdad752c8312a8623239686a9a2071e8825f20acb8f2198c3f659"
        ],
        "Parent": "",
        "Comment": "buildkit.dockerfile.v0",
        "Created": "2026-01-28T01:18:04.977843834Z",
        "DockerVersion": "",
        "Author": "",
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 3872672,
        "GraphDriver": {
            "Data": null,
            "Name": "overlayfs"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:989e799e634906e94dc9a5ee2ee26fc92ad260522990f26e707861a5f52bf64e"
            ]
        },
        "Metadata": {
            "LastTagTime": "2026-02-24T17:09:25.809624681Z"
        },
        "Descriptor": {
            "mediaType": "application/vnd.oci.image.index.v1+json",
            "digest": "sha256:25109184c71bdad752c8312a8623239686a9a2071e8825f20acb8f2198c3f659",
            "size": 9218
        },
        "Config": {
            "Cmd": [
                "/bin/sh"
            ],
            "Entrypoint": null,
            "Env": [
            ],
            "OnBuild": null,
            "User": "",
            "Volumes": null,
            "WorkingDir": "/"
        }
    }
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
PS D:\ANDYtws\projects\docker_proj> docker ps -a
CONTAINER ID   IMAGE                 COMMAND                  CREATED        STATUS
44db94363dd5   jenkins/jenkins:lts   "/usr/bin/tini -- /u…"   3 months ago   Exited (255) 3 months ago   
0.0.0.0:8080->8080/tcp, 0.0.0.0:50000->50000/tcp   jenkins
e6d0a6db4dd7   wordcounter:latest    "httpd-foreground"       3 months ago   Exited (0) 3 months ago     
                                                   quirky_cerf
PS D:\ANDYtws\projects\docker_proj>

'''

### Task 2: Image Layers
1. Run `docker image history nginx` — what do you see?
                         PS D:\ANDYtws\projects\docker_proj> docker image history nginx
                IMAGE          CREATED       CREATED BY                                      SIZE      COMMENT
                341bf0f3ce6c   2 weeks ago   CMD ["nginx" "-g" "daemon off;"]                0B        buildkit.dockerfile.v0
                <missing>      2 weeks ago   STOPSIGNAL SIGQUIT                              0B        buildkit.dockerfile.v0
                <missing>      2 weeks ago   EXPOSE map[80/tcp:{}]                           0B        buildkit.dockerfile.v0
                <missing>      2 weeks ago   ENTRYPOINT ["/docker-entrypoint.sh"]            0B        buildkit.dockerfile.v0
                <missing>      2 weeks ago   COPY 30-tune-worker-processes.sh /docker-ent…   16.4kB    buildkit.dockerfile.v0
                <missing>      2 weeks ago   COPY 20-envsubst-on-templates.sh /docker-ent…   12.3kB    buildkit.dockerfile.v0
                <missing>      2 weeks ago   COPY 15-local-resolvers.envsh /docker-entryp…   12.3kB    buildkit.dockerfile.v0
                <missing>      2 weeks ago   COPY 10-listen-on-ipv6-by-default.sh /docker…   12.3kB    buildkit.dockerfile.v0
                <missing>      2 weeks ago   COPY docker-entrypoint.sh / # buildkit          8.19kB    buildkit.dockerfile.v0
                <missing>      2 weeks ago   RUN /bin/sh -c set -x     && groupadd --syst…   86.6MB    buildkit.dockerfile.v0
                <missing>      2 weeks ago   ENV DYNPKG_RELEASE=1~trixie                     0B        buildkit.dockerfile.v0
                <missing>      2 weeks ago   ENV PKG_RELEASE=1~trixie                        0B        buildkit.dockerfile.v0
                <missing>      2 weeks ago   ENV ACME_VERSION=0.3.1                          0B        buildkit.dockerfile.v0
                <missing>      2 weeks ago   ENV NJS_RELEASE=1~trixie                        0B        buildkit.dockerfile.v0
                <missing>      2 weeks ago   ENV NJS_VERSION=0.9.5                           0B        buildkit.dockerfile.v0
                <missing>      2 weeks ago   ENV NGINX_VERSION=1.29.5                        0B        buildkit.dockerfile.v0
                <missing>      2 weeks ago   LABEL maintainer=NGINX Docker Maintainers <d…   0B        buildkit.dockerfile.v0
                <missing>      3 weeks ago   # debian.sh --arch 'amd64' out/ 'trixie' '@1…   87.4MB    debuerreotype 0.17PS D:\ANDYtws\projec
                ts\docker_proj>

3. Each line is a **layer**. Note how some layers show sizes and some show 0B
       PS D:\ANDYtws\projects\docker_proj> docker images
            REPOSITORY                                  TAG                IMAGE ID       CREATED         SIZE
            ubuntu                                      latest             d1e2e92c075e   2 weeks ago     117MB
            nginx                                       latest             341bf0f3ce6c   2 weeks ago     237MB
            alpine                                      latest             25109184c71b   3 weeks ago     13MB
            wordcounter                                 latest             b54c82d97796   3 months ago    93.8MB
            jenkins/jenkins                             lts                f2519b99350f   4 months ago    820MB
            kindest/node                                v1.34.0            7416a61b42b1   6 months ago    1.45GB
            hello-world                                 latest             ef54e839ef54   6 months ago    20.3kB
            envoyproxy/envoy                            v1.32.6            16a413173825   9 months ago    224MB
            docker/desktop-cloud-provider-kind          v0.3.0-desktop.3   c321a5c92549   17 months ago   582MB
            docker/desktop-containerd-registry-mirror   v0.0.2             aadc48ce8960   18 months ago   46.4MB
   
5. Write in your notes: What are layers and why does Docker use them?
   On base image going to run/add a layer on it.


### Task 3: Container Lifecycle
Practice the full lifecycle on one container:
1. **Create** a container (without starting it)
2. **Start** the container
3. **Pause** it and check status
4. **Unpause** it
5. **Stop** it
6. **Restart** it
7. **Kill** it
8. **Remove** it

Check `docker ps -a` after each step — observe the state changes.

'''
Excellent 👌 This is **core Docker operations knowledge** — exactly what interviewers ask.

We’ll practice the full lifecycle using an **Nginx** container.

---

# 🔁 Container Lifecycle – Step by Step

We’ll use:

**Nginx**

---

# ✅ 1️⃣ Create a Container (Without Starting It)

Use `docker create`.

```bash
docker create --name lifecycle-nginx -p 8085:80 nginx
```

### 🔎 What happens?

* Container is created
* Image is attached
* It is NOT running yet

Check:

```bash
docker ps -a
```

Status will show:

```
Created
```

---

# ✅ 2️⃣ Start the Container

```bash
docker start lifecycle-nginx
```

Now check:

```bash
docker ps
```

Status:

```
Up X seconds
```

You can open:

```
http://localhost:8085
```

---

# ✅ 3️⃣ Pause the Container

Pause freezes processes inside container.

```bash
docker pause lifecycle-nginx
```

Check status:

```bash
docker ps
```

You’ll see:

```
Up (Paused)
```

### 🔎 What pause does:

* Suspends all processes
* Uses Linux freezer cgroup
* Container still exists in memory

Browser may stop responding.

---

# ✅ 4️⃣ Unpause the Container

```bash
docker unpause lifecycle-nginx
```

Check again:

```bash
docker ps
```

Status returns to:

```
Up X seconds
```

---

# ✅ 5️⃣ Stop the Container

Graceful shutdown (sends SIGTERM).

```bash
docker stop lifecycle-nginx
```

Status:

```
Exited
```

---

# ✅ 6️⃣ Restart the Container

```bash
docker restart lifecycle-nginx
```

Equivalent to:

* Stop
* Start

Check:

```bash
docker ps
```

---

# ✅ 7️⃣ Kill the Container

Force stop (sends SIGKILL).

```bash
docker kill lifecycle-nginx
```

Difference:

| stop            | kill       |
| --------------- | ---------- |
| Graceful        | Immediate  |
| SIGTERM         | SIGKILL    |
| App can cleanup | No cleanup |

Used when container is stuck.

---

# ✅ 8️⃣ Remove the Container

Container must be stopped first.

```bash
docker rm lifecycle-nginx
```

Verify:

```bash
docker ps -a
```

It should be gone.

---

# 🧠 Full Lifecycle Diagram (Important for Interviews)

```
docker create
      ↓
   Created
      ↓
docker start
      ↓
   Running
   ↓      ↓
pause   stop
   ↓      ↓
Paused   Exited
   ↓        ↓
unpause   restart
            ↓
          Running
            ↓
          kill
            ↓
          Exited
            ↓
          rm
            ↓
         Deleted
```

---

# 🎯 Real DevOps Insight

This lifecycle is exactly what happens in:

* CI/CD test containers
* Kubernetes Pods
* ECS tasks
* Helm deployments

Kubernetes internally:

* Creates container
* Starts it
* Restarts on crash
* Kills if unhealthy
* Removes during scaling down

---

# 🔥 Interview Question They Ask

👉 Difference between:

* `docker run`
* `docker create`
* `docker start`

Answer:

* `docker run` = create + start
* `docker create` = only create
* `docker start` = start existing container

---

# 🚀 You’ve Now Mastered

✔ Container states
✔ Graceful vs force shutdown
✔ Pause behavior
✔ Full lifecycle management

---
'''



### Task 4: Working with Running Containers
1. Run an Nginx container in detached mode
2. View its **logs**
3. View **real-time logs** (follow mode)
4. **Exec** into the container and look around the filesystem
5. Run a single command inside the container without entering it
6. **Inspect** the container — find its IP address, port mappings, and mounts


'''PS D:\ANDYtws\projects\docker_proj> docker run --name nginccont -p 9090:80 nginx 
docker: Error response from daemon: Conflict. The container name "/nginccont" is already in use by container "ad5f73ed41c5704
me.

Run 'docker run --help' for more information
PS D:\ANDYtws\projects\docker_proj> docker ps -a
CONTAINER ID   IMAGE                 COMMAND                  CREATED         STATUS                      PORTS
                                NAMES
ad5f73ed41c5   nginx                 "/docker-entrypoint.…"   2 minutes ago   Exited (0) 14 seconds ago
                                nginccont
44db94363dd5   jenkins/jenkins:lts   "/usr/bin/tini -- /u…"   3 months ago    Exited (255) 3 months ago   0.0.0.0:8080->8080/
tcp, 0.0.0.0:50000->50000/tcp   jenkins
e6d0a6db4dd7   wordcounter:latest    "httpd-foreground"       3 months ago    Exited (0) 3 months ago
                                quirky_cerf
PS D:\ANDYtws\projects\docker_proj> docker logs nginccont
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/02/24 17:38:49 [notice] 1#1: using the "epoll" event method
2026/02/24 17:38:49 [notice] 1#1: nginx/1.29.5
2026/02/24 17:38:49 [notice] 1#1: built by gcc 14.2.0 (Debian 14.2.0-19)
2026/02/24 17:38:49 [notice] 1#1: OS: Linux 6.6.87.2-microsoft-standard-WSL2
2026/02/24 17:38:49 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2026/02/24 17:38:49 [notice] 1#1: start worker processes
2026/02/24 17:38:49 [notice] 1#1: start worker process 28
2026/02/24 17:38:49 [notice] 1#1: start worker process 29
2026/02/24 17:38:49 [notice] 1#1: start worker process 30
2026/02/24 17:38:49 [notice] 1#1: start worker process 31
2026/02/24 17:40:49 [notice] 1#1: signal 2 (SIGINT) received, exiting
2026/02/24 17:40:49 [notice] 29#29: exiting
2026/02/24 17:40:49 [notice] 28#28: exiting
2026/02/24 17:40:49 [notice] 28#28: exit
2026/02/24 17:40:49 [notice] 29#29: exit
2026/02/24 17:40:49 [notice] 31#31: exit
2026/02/24 17:40:49 [notice] 30#30: exiting
2026/02/24 17:40:49 [notice] 30#30: exit
2026/02/24 17:40:49 [notice] 1#1: signal 17 (SIGCHLD) received from 30
2026/02/24 17:40:49 [notice] 1#1: worker process 30 exited with code 0
2026/02/24 17:40:49 [notice] 1#1: worker process 31 exited with code 0
2026/02/24 17:40:49 [notice] 1#1: signal 29 (SIGIO) received
2026/02/24 17:40:49 [notice] 1#1: signal 17 (SIGCHLD) received from 29
2026/02/24 17:40:49 [notice] 1#1: worker process 29 exited with code 0
2026/02/24 17:40:49 [notice] 1#1: exit
PS D:\ANDYtws\projects\docker_proj> docker real-time logs nginccont
docker: unknown command: docker real-time
Run 'docker --help' for more information
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
PS D:\ANDYtws\projects\docker_proj> docker ps -a
CONTAINER ID   IMAGE                 COMMAND                  CREATED         STATUS                          PORTS
                                    NAMES
ad5f73ed41c5   nginx                 "/docker-entrypoint.…"   3 minutes ago   Exited (0) About a minute ago
                                    nginccont
44db94363dd5   jenkins/jenkins:lts   "/usr/bin/tini -- /u…"   3 months ago    Exited (255) 3 months ago       0.0.0.0:8080->8
080/tcp, 0.0.0.0:50000->50000/tcp   jenkins
                                    quirky_cerf
PS D:\ANDYtws\projects\docker_proj> docker start ad5f73ed41c5
ad5f73ed41c5
PS D:\ANDYtws\projects\docker_proj> docker ps -a
ad5f73ed41c5   nginx                 "/docker-entrypoint.…"   3 minutes ago   Up 2 seconds                0.0.0.0:9090->80/tc
p, [::]:9090->80/tcp            nginccont
44db94363dd5   jenkins/jenkins:lts   "/usr/bin/tini -- /u…"   3 months ago    Exited (255) 3 months ago   0.0.0.0:8080->8080/
tcp, 0.0.0.0:50000->50000/tcp   jenkins
e6d0a6db4dd7   wordcounter:latest    "httpd-foreground"       3 months ago    Exited (0) 3 months ago
                                quirky_cerf
PS D:\ANDYtws\projects\docker_proj> docker ps   
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                     NA
MES
ad5f73ed41c5   nginx     "/docker-entrypoint.…"   3 minutes ago   Up 6 seconds   0.0.0.0:9090->80/tcp, [::]:9090->80/tcp   ng
inccont
PS D:\ANDYtws\projects\docker_proj> docker exec -it -d ad5f73ed41c5 bash
PS D:\ANDYtws\projects\docker_proj> docker exec -it ad5f73ed41c5 bash   
root@ad5f73ed41c5:/# ls
bin   dev                  docker-entrypoint.sh  home  lib64  mnt  proc  run   srv  tmp  var
boot  docker-entrypoint.d  etc                   lib   media  opt  root  sbin  sys  usr
root@ad5f73ed41c5:/# pwd
/
root@ad5f73ed41c5:/# tree
bash: tree: command not found
root@ad5f73ed41c5:/# ls -lrt
total 64
lrwxrwxrwx   1 root root    8 Jan  2 12:35 sbin -> usr/sbin
lrwxrwxrwx   1 root root    9 Jan  2 12:35 lib64 -> usr/lib64
lrwxrwxrwx   1 root root    7 Jan  2 12:35 lib -> usr/lib
drwxr-xr-x   2 root root 4096 Jan  2 12:35 home
drwxr-xr-x   2 root root 4096 Jan  2 12:35 boot
lrwxrwxrwx   1 root root    7 Jan  2 12:35 bin -> usr/bin
drwxr-xr-x   1 root root 4096 Feb  2 00:00 var
drwxr-xr-x   1 root root 4096 Feb  2 00:00 usr
drwxrwxrwt   2 root root 4096 Feb  2 00:00 tmp
drwxr-xr-x   2 root root 4096 Feb  2 00:00 srv
drwxr-xr-x   2 root root 4096 Feb  2 00:00 opt
drwxr-xr-x   2 root root 4096 Feb  2 00:00 mnt
drwxr-xr-x   2 root root 4096 Feb  2 00:00 media
-rwxr-xr-x   1 root root 1620 Feb  4 23:52 docker-entrypoint.sh
drwxr-xr-x   1 root root 4096 Feb  4 23:53 docker-entrypoint.d
dr-xr-xr-x  13 root root    0 Feb 24 17:06 sys
drwxr-xr-x   1 root root 4096 Feb 24 17:38 etc
dr-xr-xr-x 246 root root    0 Feb 24 17:42 proc
drwxr-xr-x   5 root root  340 Feb 24 17:42 dev
drwxr-xr-x   1 root root 4096 Feb 24 17:42 run
root@ad5f73ed41c5:/# exit
exit
PS D:\ANDYtws\projects\docker_proj> docker inspect ad5f73ed41c5
[
    {
        "Id": "ad5f73ed41c5704c85e14f692fe6fe25c04dd99d47d9d889a7a71bdd7b9f3198",
        "Created": "2026-02-24T17:38:48.362720127Z",
        "Path": "/docker-entrypoint.sh",
        "Args": [
            "nginx",
            "-g",
            "daemon off;"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 137561,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2026-02-24T17:42:11.843126604Z",
            "FinishedAt": "2026-02-24T17:40:49.330706838Z"
        },
        "Image": "sha256:341bf0f3ce6c5277d6002cf6e1fb0319fa4252add24ab6a0e262e0056d313208",
        "ResolvConfPath": "/var/lib/docker/containers/ad5f73ed41c5704c85e14f692fe6fe25c04dd99d47d9d889a7a71bdd7b9f3198/resolv
.conf",
        "HostnamePath": "/var/lib/docker/containers/ad5f73ed41c5704c85e14f692fe6fe25c04dd99d47d9d889a7a71bdd7b9f3198/hostname
",
        "HostsPath": "/var/lib/docker/containers/ad5f73ed41c5704c85e14f692fe6fe25c04dd99d47d9d889a7a71bdd7b9f3198/hosts",    
        "LogPath": "/var/lib/docker/containers/ad5f73ed41c5704c85e14f692fe6fe25c04dd99d47d9d889a7a71bdd7b9f3198/ad5f73ed41c57
04c85e14f692fe6fe25c04dd99d47d9d889a7a71bdd7b9f3198-json.log",
        "Name": "/nginccont",
        "RestartCount": 0,
        "Driver": "overlayfs",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": [
            "e71001c1a1a89797c1592f445576e5f7adddbf151a2f2cd9f6587c135c484e4e"
        ],
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "bridge",
            "PortBindings": {
                "80/tcp": [
                    {
                        "HostIp": "",
                        "HostPort": "9090"
                    }
                ]
            },
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "ConsoleSize": [
                14,
                125
            ],
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "private",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": [],
            "BlkioDeviceWriteBps": [],
            "BlkioDeviceReadIOps": [],
            "BlkioDeviceWriteIOps": [],
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": null,
            "PidsLimit": null,
            "Ulimits": [],
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/interrupts",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware",
                "/sys/devices/virtual/powercap"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": null,
            "Name": "overlayfs"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "ad5f73ed41c5",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": true,
            "AttachStderr": true,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.29.5",
                "NJS_VERSION=0.9.5",
                "NJS_RELEASE=1~trixie",
                "ACME_VERSION=0.3.1",
                "PKG_RELEASE=1~trixie",
                "DYNPKG_RELEASE=1~trixie"
            ],
            "Cmd": [
                "nginx",
                "-g",
                "daemon off;"
            ],
            "Image": "nginx",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": [
                "/docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers \u003cdocker-maint@nginx.com\u003e"
            },
            "StopSignal": "SIGQUIT",
            "StopTimeout": 1
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "2c424ce2034483d8610226f3533a8e40b71f6feb7e0a7d5b0ef53b5318eb3d90",
            "SandboxKey": "/var/run/docker/netns/2c424ce20344",
            "Ports": {
                "80/tcp": [
                    {
                        "HostIp": "0.0.0.0",
                        "HostPort": "9090"
                    },
                    {
                        "HostIp": "::",
                        "HostPort": "9090"
                    }
                ]
            },
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "aa7590c5c03db7c29ced22ae8615cb0a8b6b06d21e116e02c74ed9ad8ce5eae4",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "12:cb:9b:73:7e:7f",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "MacAddress": "12:cb:9b:73:7e:7f",
                    "DriverOpts": null,
                    "GwPriority": 0,
                    "NetworkID": "9ba71ed469a1d0269194624fc37e3ac4e9ad1ec3d9c66d0f755b5fd5d3659281",
                    "EndpointID": "aa7590c5c03db7c29ced22ae8615cb0a8b6b06d21e116e02c74ed9ad8ce5eae4",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "DNSNames": null
                }
            }
        },
        "ImageManifestDescriptor": {
            "mediaType": "application/vnd.oci.image.manifest.v1+json",
            "digest": "sha256:514a9c2814250e61396ef4d6125ece1a8fbb3b0964a2ab441e9f7acf0b66b8b5",
            "size": 2290,
            "annotations": {
                "com.docker.official-images.bashbrew.arch": "amd64",
                "org.opencontainers.image.base.digest": "sha256:346fa035ca82052ce8ec3ddb9df460b255507acdeb1dc880a8b6930e778a5
53c",
                "org.opencontainers.image.base.name": "debian:trixie-slim",
                "org.opencontainers.image.created": "2026-02-04T23:52:22Z",
                "org.opencontainers.image.revision": "ffe72978e08c5b0dacecd604e528f6d0741a9ae5",
                "org.opencontainers.image.source": "https://github.com/nginx/docker-nginx.git#ffe72978e08c5b0dacecd604e528f6d
0741a9ae5:mainline/debian",
                "org.opencontainers.image.url": "https://hub.docker.com/_/nginx",
                "org.opencontainers.image.version": "1.29.5"
            },
            "platform": {
                "architecture": "amd64",
                "os": "linux"
            }
        }
    }
]
PS D:\ANDYtws\projects\docker_proj> 
-------------------------------------------------------------------------------------------------------------------------------------
---
Excellent 👌 This is real-world Docker debugging knowledge — exactly what DevOps engineers do in production.

We’ll use **Nginx** again.

---

# ✅ 1️⃣ Run Nginx in Detached Mode

```bash
docker run -d -p 8088:80 --name debug-nginx nginx
```

* `-d` → detached (background)
* `-p 8088:80` → map host 8088 → container 80
* `--name` → easier debugging

Verify:

```bash
docker ps
```

Open browser:

```
http://localhost:8088
```

---

# ✅ 2️⃣ View Logs

```bash
docker logs debug-nginx
```

You’ll see:

* Nginx startup logs
* Access logs (after hitting browser)

---

# ✅ 3️⃣ View Real-Time Logs (Follow Mode)

```bash
docker logs -f debug-nginx
```

Now refresh browser.

You’ll see live access logs like:

```
172.17.0.1 - - [date] "GET / HTTP/1.1" 200 615 "-" "Mozilla..."
```

Stop follow mode with:

```
Ctrl + C
```

---

# ✅ 4️⃣ Exec Into the Container (Interactive)

```bash
docker exec -it debug-nginx bash
```

If bash not available:

```bash
docker exec -it debug-nginx sh
```

Now you are inside container.

Try:

```bash
ls /
ls /usr/share/nginx/html
cat /usr/share/nginx/html/index.html
whoami
```

You’ll see default Nginx HTML page file.

Exit:

```bash
exit
```

---

# ✅ 5️⃣ Run a Single Command Without Entering Container

Instead of full shell:

```bash
docker exec debug-nginx ls /usr/share/nginx/html
```

Or:

```bash
docker exec debug-nginx cat /etc/nginx/nginx.conf
```

This is very useful in:

* Production debugging
* Automation scripts
* CI pipelines

---

# ✅ 6️⃣ Inspect the Container

This is VERY important.

```bash
docker inspect debug-nginx
```

This outputs full JSON.

Let’s extract important info.

---

## 🔹 Find Container IP Address

```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' debug-nginx
```

Example output:

```
172.17.0.2
```

This is internal Docker network IP.

---

## 🔹 Find Port Mappings

```bash
docker inspect -f '{{.NetworkSettings.Ports}}' debug-nginx
```

You’ll see something like:

```
map[80/tcp:[{0.0.0.0 8088}]]
```

Meaning:

* Container 80 → Host 8088

---

## 🔹 Find Mounts

```bash
docker inspect -f '{{.Mounts}}' debug-nginx
```

If no volumes attached → empty array `[]`.

---

# 🧠 What `docker inspect` Actually Does

It shows:

* Container ID
* Image used
* Network configuration
* IP address
* Port mapping
* Volume mounts
* Restart policy
* Environment variables

This is gold for debugging.

---

# 🔥 Real DevOps Scenario

Production issue:

* Service not reachable
* You check:

  * `docker ps`
  * `docker logs`
  * `docker inspect`
  * `docker exec`

Exactly what you practiced.

In Kubernetes equivalent:

* `kubectl logs`
* `kubectl exec`
* `kubectl describe pod`
* `kubectl get pod -o wide`

---

# 🧹 Cleanup

Stop container:

```bash
docker stop debug-nginx
```

Remove container:

```bash
docker rm debug-nginx
```

---

# 🚀 You Now Know How To

✔ Debug running containers
✔ View logs (static + live)
✔ Execute inside containers
✔ Extract networking details
✔ Inspect configuration

---
---

'''


### Task 5: Cleanup
1. Stop all running containers in one command
2. Remove all stopped containers in one command
3. Remove unused images
4. Check how much disk space Docker is using

'''
Excellent 👌 This is proper Docker hygiene — very important in CI servers and cloud VMs.

Let’s clean everything safely.

---

# ✅ 1️⃣ Stop All Running Containers (One Command)

```bash
docker stop $(docker ps -q)
```

### 🔎 What This Does:

* `docker ps -q` → gets IDs of running containers
* `docker stop` → stops them

If no containers are running → nothing happens.

---

# ✅ 2️⃣ Remove All Stopped Containers

```bash
docker container prune
```

Docker will ask:

```
Are you sure you want to continue? [y/N]
```

Type `y`.

---

### 🔥 Force Remove Without Prompt

```bash
docker container prune -f
```

---

# ✅ 3️⃣ Remove Unused Images

Remove dangling images:

```bash
docker image prune
```

Remove ALL unused images (not just dangling):

```bash
docker image prune -a
```

⚠️ Be careful: `-a` removes all images not used by containers.

---

# 🚀 One Big Cleanup Command (Advanced)

If you want to clean:

* Stopped containers
* Unused networks
* Unused images
* Build cache

```bash
docker system prune
```

Or aggressive cleanup:

```bash
docker system prune -a
```

---

# ✅ 4️⃣ Check How Much Disk Space Docker Is Using

```bash
docker system df
```

Example output:

```
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          5         2         1.2GB     800MB
Containers      3         0         100MB     100MB
Local Volumes   2         1         500MB     200MB
Build Cache     -         -         300MB     300MB
```

---

### 🔎 Detailed View

```bash
docker system df -v
```

Shows per-image and per-container usage.

---

# 🧠 Why Cleanup Is Important (DevOps Reality)

On:

* CI/CD runners
* Jenkins servers
* GitHub Actions self-hosted runners
* Cloud VMs

If you don’t clean:

* Disk fills up
* Builds fail
* Nodes crash
* Production incidents happen

---

# 🎯 Production Tip

Many teams run scheduled cleanup:

```bash
docker system prune -f
```

as a cron job on CI servers.

---

# 🚀 You Now Know

✔ Bulk stop containers
✔ Remove unused containers
✔ Remove unused images
✔ Free disk space
✔ Monitor Docker storage

---


'''

   
