**Chapter 2: Try Docker Deployment**
-
**1. Getting started with docker**
- **Docker Architecture**
![architecture-docker](https://hackmd.io/_uploads/H1lryU3Ap.svg)

**Docker daemon** functions to build, distribute and run Docker containers. Users cannot directly use the Docker Daemon, but to use the Docker Daemon the user uses the Docker client as an intermediary or CLI.
**Docker images** are read only templates. This template is actually an OS or OS that has various applications installed. Docker images function to create docker containers, with just 1 docker image we can create many docker containers.
**Docker container** can be said to be a folder, where the Docker container is created using the Docker daemon. Every time a Docker container is saved, a new layer will be formed right above the Docker image or base image above it. For example, let&#39;s say we use an Ubuntu image, then we create a container from the Ubuntu image with the name ubuntuu, then we install software such as nginx, then automatically the Ubuntu container will be above the image layer or base Ubuntu image. You can create multiple docker containers from 1 docker image. This Docker container can later be built so that it will produce a Docker image, and we can reuse the Docker images produced from this Docker container to create a new Docker container.
**Docker registry** is a collection of private and public docker images that you can access on Docker Hub. By using the docker registry, you can use docker images that have been created by other developers, making it easier for us to develop applications.

- **Install Docker**

**Execute in all node**

```bash
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg lsb-release -y
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo &#34;deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable&#34; | sudo tee /etc/apt/sources.list.d/docker.list &gt; /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

- **Display docker version**
```bash
student@pod-jani01-node01:~$ docker version
Client: Docker Engine - Community
 Version:           26.0.0
 API version:       1.45
 Go version:        go1.21.8
 Git commit:        2ae903e
 Built:             Wed Mar 20 15:17:48 2024
 OS/Arch:           linux/amd64
 Context:           default

Server: Docker Engine - Community
 Engine:
  Version:          26.0.0
  API version:      1.45 (minimum version 1.24)
  Go version:       go1.21.8
  Git commit:       8b79278
  Built:            Wed Mar 20 15:17:48 2024
  OS/Arch:          linux/amd64
  Experimental:     false
```

- **Display the docker installation details**
```bash
student@pod-jani01-node01:~$ docker info
Client: Docker Engine - Community
 Version:    26.0.0
 Context:    default
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.13.1
    Path:     /usr/libexec/docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.25.0
    Path:     /usr/libexec/docker/cli-plugins/docker-compose
```

- **Add user into docker group**
```bash
groupadd docker
sudo usermod -aG docker $USER
sudo chmod 666 /var/run/docker.sock
```

- **Test the docker installation**
```bash
student@pod-jani01-node01:~$ docker run registry.adinusa.id/btacademy/hello-world
Unable to find image &#39;registry.adinusa.id/btacademy/hello-world:latest&#39; locally
latest: Pulling from btacademy/hello-world
2db29710123e: Pull complete 
Digest: sha256:f54a58bc1aac5ea1a25d796ae155dc228b3f0e11d046ae276b39c4bf2f13d8c4
Status: Downloaded newer image for registry.adinusa.id/btacademy/hello-world:latest
```

- **Display the downloaded image**
```bash
student@pod-jani01-node01:~$ docker image ls
REPOSITORY                                  TAG       IMAGE ID       CREATED         SIZE
nginx                                       latest    1403e55ab369   15 months ago   142MB
httpd                                       latest    73c10eb9266e   15 months ago   145MB
registry.adinusa.id/btacademy/hello-world   latest    feb5d9fea6a5   2 years ago     13.3kB
```

- **Display all container (active or exit)**
```bash
student@pod-jani01-node01:~$ docker container ls -a
CONTAINER ID   IMAGE                                       COMMAND    CREATED          STATUS                      PORTS     NAMES
29966d10353a   registry.adinusa.id/btacademy/hello-world   &#34;/hello&#34;   20 minutes ago   Exited (0) 20 minutes ago             gifted_cannon
```

**2. Docker Run - part 1**

**Execute in node01**

- **Search image redis from dockerhub &amp; harbor**
```bash
student@pod-jani01-node01:~$ docker search redis
NAME                                DESCRIPTION                                     STARS     OFFICIAL
redis                               Redis is an open source key-value store that…   298       [OK]
redislabs/redisearch                Redis With the RedisSearch module pre-loaded…   63        
student@pod-jani01-node01:~$ skopeo list-tags docker://registry.adinusa.id/btacademy/redis
{
    &#34;Repository&#34;: &#34;registry.adinusa.id/btacademy/redis&#34;,
    &#34;Tags&#34;: [
        &#34;latest&#34;
    ]
}
```

- **Running image redis**
```bash
docker run registry.adinusa.id/btacademy/redis  # CTRL + c for quit
docker run -d registry.adinusa.id/btacademy/redis  # Running in background (Background)
docker run -d --name redis1 registry.adinusa.id/btacademy/redis # Giving the container a name
```

- **Display running container**
```bash
student@pod-jani01-node01:~$ docker ps
CONTAINER ID   IMAGE                                 COMMAND                  CREATED         STATUS         PORTS      NAMES
5a6293c16eae   registry.adinusa.id/btacademy/redis   &#34;docker-entrypoint.s…&#34;   5 minutes ago   Up 5 minutes   6379/tcp   redis1
83c185992a9c   registry.adinusa.id/btacademy/redis   &#34;docker-entrypoint.s…&#34;   5 minutes ago   Up 5 minutes   6379/tcp   busy_jemison 
student@pod-jani01-node01:~$ docker container ls
CONTAINER ID   IMAGE                                 COMMAND                  CREATED         STATUS         PORTS      NAMES
5a6293c16eae   registry.adinusa.id/btacademy/redis   &#34;docker-entrypoint.s…&#34;   6 minutes ago   Up 6 minutes   6379/tcp   redis1
83c185992a9c   registry.adinusa.id/btacademy/redis   &#34;docker-entrypoint.s…&#34;   6 minutes ago   Up 6 minutes   6379/tcp   busy_jemison
```

- **Display all docker container**
```bash
student@pod-jani01-node01:~$ docker ps -a
CONTAINER ID   IMAGE                                       COMMAND                  CREATED          STATUS                      PORTS      NAMES
5a6293c16eae   registry.adinusa.id/btacademy/redis         &#34;docker-entrypoint.s…&#34;   6 minutes ago    Up 6 minutes                6379/tcp   redis1
83c185992a9c   registry.adinusa.id/btacademy/redis         &#34;docker-entrypoint.s…&#34;   6 minutes ago    Up 6 minutes                6379/tcp   busy_jemison
2d41ea92d8cc   registry.adinusa.id/btacademy/hello-world   &#34;/hello&#34;                 11 minutes ago   Exited (0) 11 minutes ago              youthful_stonebraker
```

- **Display container description**
```bash
student@pod-jani01-node01:~$ docker inspect redis1
[
    {
        &#34;Id&#34;: &#34;5a6293c16eaeadb327c76bcfc84b7cd18d8d7abf7a01954dee68de533c96c1f0&#34;,
        &#34;Created&#34;: &#34;2024-03-24T22:37:49.933988143Z&#34;,
        &#34;Path&#34;: &#34;docker-entrypoint.sh&#34;,
        &#34;Args&#34;: [
            &#34;redis-server&#34;
        ],
```

- **Display content of the logs in container**
```bash
student@pod-jani01-node01:~$ docker logs redis1
1:C 24 Mar 2024 22:37:50.325 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 24 Mar 2024 22:37:50.326 # Redis version=7.0.11, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 24 Mar 2024 22:37:50.326 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 24 Mar 2024 22:37:50.327 * monotonic clock: POSIX clock_gettime
1:M 24 Mar 2024 22:37:50.328 * Running mode=standalone, port=6379.
1:M 24 Mar 2024 22:37:50.328 # Server initialized
```

- **Display live stream resource used in container**
```bash
student@pod-jani01-node01:~$ docker stats redis1
CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT     MEM %     NET I/O           BLOCK I/O   PIDS
5a6293c16eae   redis1    0.22%     2.523MiB / 3.832GiB   0.06%     1.36kB / 1.01kB   0B / 0B     5
```

- **Display running process in container**
```bash
student@pod-jani01-node01:~$ docker top redis1
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
lxd                 10103               10083               0                   22:37               ?                   00:00:01            redis-server *:6379
```

- **Shutdown the container**
```bash
student@pod-jani01-node01:~$ docker stop redis1
redis1
```

**Execute in node02**

- **Search image nginx From DockerHub &amp; Harbor**
```bash
student@pod-jani01-node02:~$ docker search nginx
NAME                               DESCRIPTION                                     STARS     OFFICIAL
nginx                              Official build of Nginx.                        52        [OK]
unit                               Official build of NGINX Unit: Universal Web …   25        [OK]
student@pod-jani01-node02:~$ skopeo list-tags docker://registry.adinusa.id/btacademy/nginx
{
    &#34;Repository&#34;: &#34;registry.adinusa.id/btacademy/nginx&#34;,
    &#34;Tags&#34;: [
        &#34;latest&#34;,
        &#34;mainline&#34;
    ]
}
```

- **Running image nginx and expose to port host**
```bash
student@pod-jani01-node02:~$ docker run -d --name nginx1 -p 80:80 registry.adinusa.id/btacademy/nginx:latest
Unable to find image &#39;registry.adinusa.id/btacademy/nginx:latest&#39; locally
latest: Pulling from btacademy/nginx
26c5c85e47da: Pull complete 
4f3256bdf66b: Pull complete 
2019c71d5655: Pull complete 
8c767bdbc9ae: Pull complete
```

- **Display a description of the nginx container**
```bash
student@pod-jani01-node02:~$ docker inspect nginx1
[
    {
        &#34;Id&#34;: &#34;0d4c45460226666b5c534b1ccfd6c04d3baeb33cfa0902fcfd8f41c90680e419&#34;,
        &#34;Created&#34;: &#34;2024-03-24T22:54:18.123243867Z&#34;,
        &#34;Path&#34;: &#34;/docker-entrypoint.sh&#34;,
        &#34;Args&#34;: [
            &#34;nginx&#34;,
            &#34;-g&#34;,
            &#34;daemon off;&#34;
        ],
```

- **Running image nginx and declare the container port**
```bash
student@pod-jani01-node02:~$ docker run -d --name nginx2 -p 80 registry.adinusa.id/btacademy/nginx:latest
015cad8af11f8607bb392970847faf596f4a5daaadf5d4d9afcfd59a6729c7f0
```

- Test browsing
```bash
student@pod-jani01-node02:~$ curl localhost:$(docker port nginx2 80 | cut -d : -f 2)
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
&lt;title&gt;Welcome to nginx!&lt;/title&gt;
&lt;style&gt;
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
```

- **Display container (activate/exit)**
```bash
student@pod-jani01-node02:~$ docker ps -a 
CONTAINER ID   IMAGE                                        COMMAND                  CREATED              STATUS                      PORTS                                     NAMES
015cad8af11f   registry.adinusa.id/btacademy/nginx:latest   &#34;/docker-entrypoint.…&#34;   About a minute ago   Up About a minute           0.0.0.0:32768-&gt;80/tcp, :::32768-&gt;80/tcp   nginx2
0d4c45460226   registry.adinusa.id/btacademy/nginx:latest   &#34;/docker-entrypoint.…&#34;   2 minutes ago        Up 2 minutes                0.0.0.0:80-&gt;80/tcp, :::80-&gt;80/tcp         nginx1
3126a1bcf6b5   registry.adinusa.id/btacademy/hello-world    &#34;/hello&#34;                 24 minutes ago       Exited (0) 24 minutes ago                                             heuristic_boyd
```

- **Display image docker**
```bash
student@pod-jani01-node02:~$ docker images
REPOSITORY                                  TAG       IMAGE ID       CREATED         SIZE
registry.adinusa.id/btacademy/nginx         latest    6efc10a0510f   11 months ago   142MB
nginx                                       latest    1403e55ab369   15 months ago   142MB
httpd                                       latest    73c10eb9266e   15 months ago   145MB
registry.adinusa.id/btacademy/hello-world   latest    feb5d9fea6a5   2 years ago     13.3kB
```

**3. Docker Run - part 2**

**Execute in node01**

- **Search image nginx from DockerHub &amp; Harbor**
```bash
student@pod-jani01-node01:~$ docker search nginx
NAME                               DESCRIPTION                                     STARS     OFFICIAL
nginx                              Official build of Nginx.                        52        [OK]
unit                               Official build of NGINX Unit: Universal Web …   25        [OK]
student@pod-jani01-node01:~$ skopeo list-tags docker://registry.adinusa.id/btacademy/nginx
{
    &#34;Repository&#34;: &#34;registry.adinusa.id/btacademy/nginx&#34;,
    &#34;Tags&#34;: [
        &#34;latest&#34;,
        &#34;mainline&#34;
    ]
}
```

- **Running an nginx image with the name nginx1 and expose port 8080**
```bash
student@pod-jani01-node01:~$ docker run -d --name nginx1 -p 8080:80 registry.adinusa.id/btacademy/nginx:latest
Unable to find image &#39;registry.adinusa.id/btacademy/nginx:latest&#39; locally
latest: Pulling from btacademy/nginx
26c5c85e47da: Already exists 
4f3256bdf66b: Pull complete 
2019c71d5655: Pull complete 
8c767bdbc9ae: Pull complete 
```

- **Displays a description of the nginx1 container**
```bash
student@pod-jani01-node01:~$ docker inspect nginx1
[
    {
        &#34;Id&#34;: &#34;3cbe2dd92d82afb00103029d8c0251d56c3b8e12795196feae2c70710baedc92&#34;,
        &#34;Created&#34;: &#34;2024-03-24T23:03:22.056021613Z&#34;,
        &#34;Path&#34;: &#34;/docker-entrypoint.sh&#34;,
        &#34;Args&#34;: [
            &#34;nginx&#34;,
            &#34;-g&#34;,
            &#34;daemon off;&#34;
        ],
```

- **Running an nginx image with the name nginx2 and expose port 8081**
```bash
student@pod-jani01-node01:~$ docker run -d --name nginx2 -p 8081:80 registry.adinusa.id/btacademy/nginx:latest
4933a893274dc161cc25af4bd84b5557735518ab7b58761d53fb4f99e75f3831
```

- **Display container (activate/exit)**
```bash
student@pod-jani01-node01:~$ docker ps -a
CONTAINER ID   IMAGE                                        COMMAND                  CREATED              STATUS                      PORTS                                   NAMES
4933a893274d   registry.adinusa.id/btacademy/nginx:latest   &#34;/docker-entrypoint.…&#34;   About a minute ago   Up About a minute           0.0.0.0:8081-&gt;80/tcp, :::8081-&gt;80/tcp   nginx2
3cbe2dd92d82   registry.adinusa.id/btacademy/nginx:latest   &#34;/docker-entrypoint.…&#34;   2 minutes ago        Up 2 minutes                0.0.0.0:8080-&gt;80/tcp, :::8080-&gt;80/tcp   nginx1
5a6293c16eae   registry.adinusa.id/btacademy/redis          &#34;docker-entrypoint.s…&#34;   28 minutes ago       Exited (0) 14 minutes ago                                           redis1
83c185992a9c   registry.adinusa.id/btacademy/redis          &#34;docker-entrypoint.s…&#34;   28 minutes ago       Up 28 minutes               6379/tcp                                busy_jemison
student@pod-jani01-node01:~$ docker container ls -a
CONTAINER ID   IMAGE                                        COMMAND                  CREATED              STATUS                      PORTS                                   NAMES
4933a893274d   registry.adinusa.id/btacademy/nginx:latest   &#34;/docker-entrypoint.…&#34;   About a minute ago   Up About a minute           0.0.0.0:8081-&gt;80/tcp, :::8081-&gt;80/tcp   nginx2
3cbe2dd92d82   registry.adinusa.id/btacademy/nginx:latest   &#34;/docker-entrypoint.…&#34;   2 minutes ago        Up 2 minutes                0.0.0.0:8080-&gt;80/tcp, :::8080-&gt;80/tcp   nginx1
5a6293c16eae   registry.adinusa.id/btacademy/redis          &#34;docker-entrypoint.s…&#34;   28 minutes ago       Exited (0) 14 minutes ago                                           redis1
83c185992a9c   registry.adinusa.id/btacademy/redis          &#34;docker-entrypoint.s…&#34;   28 minutes ago       Up 28 minutes               6379/tcp                                busy_jemison
```

- **Check nginx output on containers**
```bash
student@pod-jani01-node01:~$ curl localhost:8080
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
&lt;title&gt;Welcome to nginx!&lt;/title&gt;
&lt;style&gt;
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
student@pod-jani01-node01:~$ curl localhost:8081
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
&lt;title&gt;Welcome to nginx!&lt;/title&gt;
&lt;style&gt;
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
```

- **Accessing the container**
```bash
student@pod-jani01-node01:~$  docker exec -it nginx2 /bin/bash
root@4933a893274d:/#
```

- **Update and install editor on the container**
```bash
root@4933a893274d:/#  apt-get update -y &amp;&amp; apt-get install nano -y
Get:1 http://deb.debian.org/debian bullseye InRelease [116 kB]
Get:2 http://deb.debian.org/debian-security bullseye-security InRelease [48.4 kB]
Get:3 http://deb.debian.org/debian bullseye-updates InRelease [44.1 kB]
```

- **Edit index.html and move index.html to default directory nginx**
![image](https://hackmd.io/_uploads/r1kaME0Ra.png)
```bash
root@4933a893274d:/# mv index.html /usr/share/nginx/html
```

- **Restart service nginx**
```bash
root@4933a893274d:/# service nginx restart
Restarting nginx: nginx
student@pod-jani01-node01:~$
```

- **Rerun the container**
```bash
student@pod-jani01-node01:~$ docker start nginx2
nginx2
```

- **Display container**
```bash
student@pod-jani01-node01:~$ docker ps
CONTAINER ID   IMAGE                                        COMMAND                  CREATED          STATUS          PORTS                                   NAMES
4933a893274d   registry.adinusa.id/btacademy/nginx:latest   &#34;/docker-entrypoint.…&#34;   17 minutes ago   Up 10 minutes   0.0.0.0:8081-&gt;80/tcp, :::8081-&gt;80/tcp   nginx2
3cbe2dd92d82   registry.adinusa.id/btacademy/nginx:latest   &#34;/docker-entrypoint.…&#34;   19 minutes ago   Up 19 minutes   0.0.0.0:8080-&gt;80/tcp, :::8080-&gt;80/tcp   nginx1
83c185992a9c   registry.adinusa.id/btacademy/redis          &#34;docker-entrypoint.s…&#34;   45 minutes ago   Up 45 minutes   6379/tcp                                busy_jemison
```

- **Check nginx output on containers**
```bash
student@pod-jani01-node01:~$ curl localhost:8080
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
&lt;title&gt;Welcome to nginx!&lt;/title&gt;
&lt;style&gt;
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
student@pod-jani01-node01:~$ curl localhost:8081
hello, jani01.
```

- **Display a description of the container**
```bash
student@pod-jani01-node01:~$ docker inspect nginx1
[
    {
        &#34;Id&#34;: &#34;3cbe2dd92d82afb00103029d8c0251d56c3b8e12795196feae2c70710baedc92&#34;,
        &#34;Created&#34;: &#34;2024-03-24T23:03:22.056021613Z&#34;,
        &#34;Path&#34;: &#34;/docker-entrypoint.sh&#34;,
        &#34;Args&#34;: [
            &#34;nginx&#34;,
            &#34;-g&#34;,
            &#34;daemon off;&#34;
        ],
student@pod-jani01-node01:~$ docker inspect nginx2
[
    {
        &#34;Id&#34;: &#34;4933a893274dc161cc25af4bd84b5557735518ab7b58761d53fb4f99e75f3831&#34;,
        &#34;Created&#34;: &#34;2024-03-24T23:04:48.222606361Z&#34;,
        &#34;Path&#34;: &#34;/docker-entrypoint.sh&#34;,
        &#34;Args&#34;: [
            &#34;nginx&#34;,
            &#34;-g&#34;,
            &#34;daemon off;&#34;
        ],
```

- **Display the log content in the container**
```bash
student@pod-jani01-node01:~$ docker logs nginx1
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2024/03/24 23:03:22 [notice] 1#1: using the &#34;epoll&#34; event method
```

- **Displays the live resources used in the container**
```bash
student@pod-jani01-node01:~$ docker stats nginx1
CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT     MEM %     NET I/O           BLOCK I/O     PIDS
3cbe2dd92d82   nginx1    0.00%     4.094MiB / 3.832GiB   0.10%     4.39kB / 3.58kB   0B / 12.3kB   5
student@pod-jani01-node01:~$ docker stats nginx2
CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT     MEM %     NET I/O           BLOCK I/O    PIDS
4933a893274d   nginx2    0.00%     3.914MiB / 3.832GiB   0.10%     1.71kB / 1.68kB   0B / 4.1kB   5
```

- **Display running process in container**
```bash
student@pod-jani01-node01:~$ docker top nginx1
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                10470               10451               0                   23:03               ?                   00:00:00            nginx: master process nginx -g daemon off;
systemd+            10509               10470               0                   23:03               ?                   00:00:00            nginx: worker process
systemd+            10510               10470               0                   23:03               ?                   00:00:00            nginx: worker process
student@pod-jani01-node01:~$ docker top nginx2
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                11142               11121               0                   23:11               ?                   00:00:00            nginx: master process nginx -g daemon off;
systemd+            11170               11142               0                   23:11               ?                   00:00:00            nginx: worker process
systemd+            11171               11142               0                   23:11               ?                   00:00:00            nginx: worker process
```

**Docker run practice**

- **Search image ubuntu from DockerHub &amp; Harbor**
```bash
student@pod-jani01-node01:~$ docker search ubuntu
NAME                             DESCRIPTION                                     STARS     OFFICIAL
ubuntu                           Ubuntu is a Debian-based Linux operating sys…   16965     [OK]
websphere-liberty                WebSphere Liberty multi-architecture images …   298       [OK]
student@pod-jani01-node01:~$ skopeo list-tags docker://registry.adinusa.id/btacademy/ubuntu
{
    &#34;Repository&#34;: &#34;registry.adinusa.id/btacademy/ubuntu&#34;,
    &#34;Tags&#34;: [
        &#34;16.04&#34;,
        &#34;18.04&#34;,
        &#34;20.04&#34;,
        &#34;20.10&#34;,
        &#34;22.04&#34;,
        &#34;latest&#34;
    ]
}
```

- **Pull image ubuntu from harbor**
```bash
student@pod-jani01-node01:~$ docker pull registry.adinusa.id/btacademy/ubuntu
Using default tag: latest
latest: Pulling from btacademy/ubuntu
74ac377868f8: Pull complete 
Digest: sha256:ebc06404a3af2fe5b4e97f34b308bc4810e8a44cb6e59109eec81e7779b0c4b1
Status: Downloaded newer image for registry.adinusa.id/btacademy/ubuntu:latest
registry.adinusa.id/btacademy/ubuntu:latest
```

- **Running the ubuntu container and access to the console**
```bash
student@pod-jani01-node01:~$ docker run -it --name ubuntu1 registry.adinusa.id/btacademy/ubuntu
root@618ae9edaa74:/#
root@618ae9edaa74:/# exit
exit
student@pod-jani01-node01:~$ docker ps -a
CONTAINER ID   IMAGE                                        COMMAND                  CREATED              STATUS                         PORTS                                   NAMES
618ae9edaa74   registry.adinusa.id/btacademy/ubuntu         &#34;/bin/bash&#34;              About a minute ago   Exited (0) 25 seconds ago                                              ubuntu1
4933a893274d   registry.adinusa.id/btacademy/nginx:latest   &#34;/docker-entrypoint.…&#34;   29 minutes ago       Up 22 minutes                  0.0.0.0:8081-&gt;80/tcp, :::8081-&gt;80/tcp   nginx2
3cbe2dd92d82   registry.adinusa.id/btacademy/nginx:latest   &#34;/docker-entrypoint.…&#34;   30 minutes ago       Up 30 minutes                  0.0.0.0:8080-&gt;80/tcp, :::8080-&gt;80/tcp   nginx1
5a6293c16eae   registry.adinusa.id/btacademy/redis          &#34;docker-entrypoint.s…&#34;   56 minutes ago       Exited (0) 43 minutes ago                                              redis1
```

- **Run the ubuntu container and delete it when exiting the container**
```bash
student@pod-jani01-node01:~$ docker run -it --rm --name ubuntu2 registry.adinusa.id/btacademy/ubuntu 
root@df50ac710cd1:/# exit
exit
student@pod-jani01-node01:~$ docker ps -a
CONTAINER ID   IMAGE                                        COMMAND                  CREATED             STATUS                          PORTS                                   NAMES
618ae9edaa74   registry.adinusa.id/btacademy/ubuntu         &#34;/bin/bash&#34;              2 minutes ago       Exited (0) About a minute ago                                           ubuntu1
4933a893274d   registry.adinusa.id/btacademy/nginx:latest   &#34;/docker-entrypoint.…&#34;   30 minutes ago      Up 24 minutes                   0.0.0.0:8081-&gt;80/tcp, :::8081-&gt;80/tcp   nginx2
3cbe2dd92d82   registry.adinusa.id/btacademy/nginx:latest   &#34;/docker-entrypoint.…&#34;   32 minutes ago      Up 32 minutes                   0.0.0.0:8080-&gt;80/tcp, :::8080-&gt;80/tcp   nginx1
5a6293c16eae   registry.adinusa.id/btacademy/redis          &#34;docker-entrypoint.s…&#34;   57 minutes ago      Exited (0) 44 minutes ago                                               redis1
```

**3. Docker Run - part 3**

**Execute in node01**

- **Running mysql container with additional parameters**
```bash
student@pod-jani01-node01:~$ docker run -d --name my-mysql -e MYSQL_ROOT_PASSWORD=RAHASIA -e MYSQL_DATABASE=latihan05 -p 3306:3306 registry.adinusa.id/btacademy/mysql
Unable to find image &#39;registry.adinusa.id/btacademy/mysql:latest&#39; locally
latest: Pulling from btacademy/mysql
328ba678bf27: Pull complete 
f3f5ff008d73: Pull complete 
dd7054d6d0c7: Pull complete 
70b5d4e8750e: Pull complete 
cdc4a7b43bdd: Pull complete 
```

- **Pull image phpmyadmin from harbor**
```bash
student@pod-jani01-node01:~$ docker pull registry.adinusa.id/btacademy/phpmyadmin:latest
latest: Pulling from btacademy/phpmyadmin
26c5c85e47da: Already exists 
39c8021d1258: Pull complete 
dff43c2de684: Downloading [======&gt;                                            ]  11.89MB/91.64MB
383987c505e8: Download complete 
3fd742e8a904: Downloading [===========================&gt;                       ]  10.62MB/19.25MB
ccf9807e8362: Download complete 
11cc7ce10028: Download complete 
```

- **Running phpmyadmin container and connect it with mysql container**
```bash
student@pod-jani01-node01:~$ docker run --name my-phpmyadmin -d --link my-mysql:db -p 8090:80 registry.adinusa.id/btacademy/phpmyadmin
102dab1f0134ffd98dd954ab3c2d590fd5ee398a567436d9b495565753e9dd39
```

- **Test browsing**
open in browser http://10.10.10.11:8090 login with user: root dan password: RAHASIA
![image](https://hackmd.io/_uploads/BkT7i4RRT.png)

**Docker run practice**

- **Run ubuntu containers with names ubuntu1 &amp; ubuntu2**
```bash
student@pod-jani01-node01:~$ docker rm ubuntu1
student@pod-jani01-node01:~$ docker run -dit --name ubuntu1 registry.adinusa.id/btacademy/ubuntu
7ad2180269ec88c19b99aa9efb5d2390fb0d2d7ef91830192527d5ab7725f28a
student@pod-jani01-node01:~$ docker run -dit --name ubuntu2 registry.adinusa.id/btacademy/ubuntu
dbb9ff85d0c4ddbf83f57a07048689a0978b1d9e64f8a704877de4b9d087a386
```

- **Display list container**
```bash
student@pod-jani01-node01:~$ docker ps
CONTAINER ID   IMAGE                                        COMMAND                  CREATED              STATUS              PORTS                                                  NAMES
dbb9ff85d0c4   registry.adinusa.id/btacademy/ubuntu         &#34;/bin/bash&#34;              57 seconds ago       Up 56 seconds                                                              ubuntu2
7ad2180269ec   registry.adinusa.id/btacademy/ubuntu         &#34;/bin/bash&#34;              About a minute ago   Up About a minute                                                          ubuntu1
102dab1f0134   registry.adinusa.id/btacademy/phpmyadmin     &#34;/docker-entrypoint.…&#34;   4 minutes ago        Up 4 minutes        0.0.0.0:8090-&gt;80/tcp, :::8090-&gt;80/tcp                  my-phpmyadmin
b908f7583653   registry.adinusa.id/btacademy/mysql          &#34;docker-entrypoint.s…&#34;   8 minutes ago        Up 8 minutes        0.0.0.0:3306-&gt;3306/tcp, :::3306-&gt;3306/tcp, 33060/tcp   my-mysql
```

- **Pause container ubuntu**
```bash
student@pod-jani01-node01:~$ docker pause ubuntu1
ubuntu1
student@pod-jani01-node01:~$ docker pause ubuntu2
ubuntu2
```

- **Check in list container if the container status is paused**
```bash
student@pod-jani01-node01:~$ docker ps
CONTAINER ID   IMAGE                                        COMMAND                  CREATED             STATUS                  PORTS                                                  NAMES
dbb9ff85d0c4   registry.adinusa.id/btacademy/ubuntu         &#34;/bin/bash&#34;              2 minutes ago       Up 2 minutes (Paused)                                                          ubuntu2
7ad2180269ec   registry.adinusa.id/btacademy/ubuntu         &#34;/bin/bash&#34;              2 minutes ago       Up 2 minutes (Paused)                                                          ubuntu1
102dab1f0134   registry.adinusa.id/btacademy/phpmyadmin     &#34;/docker-entrypoint.…&#34;   6 minutes ago       Up 6 minutes            0.0.0.0:8090-&gt;80/tcp, :::8090-&gt;80/tcp                  my-phpmyadmin
b908f7583653   registry.adinusa.id/btacademy/mysql          &#34;docker-entrypoint.s…&#34;   9 minutes ago       Up 9 minutes            0.0.0.0:3306-&gt;3306/tcp, :::3306-&gt;3306/tcp, 33060/tcp   my-mysql
```

- **Check resource usage when the ubuntu container is paused**
```bash
student@pod-jani01-node01:~$ docker stats ubuntu1
CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT   MEM %     NET I/O         BLOCK I/O   PIDS
7ad2180269ec   ubuntu1   0.00%     848KiB / 3.832GiB   0.02%     1.87kB / 866B   0B / 0B     1
student@pod-jani01-node01:~$ docker stats ubuntu2
CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT   MEM %     NET I/O         BLOCK I/O   PIDS
dbb9ff85d0c4   ubuntu2   0.00%     836KiB / 3.832GiB   0.02%     1.29kB / 866B   0B / 0B     1
```

- **Unpause container ubuntu1**
```bash
student@pod-jani01-node01:~$ docker unpause ubuntu1
ubuntu1
```

**Docker run practice**

- **Create database containers with limited specifications**
```bash
student@pod-jani01-node01:~$ docker container run -d --name ch6_mariadb --memory 256m --cpu-shares 1024 --cap-drop net_raw -e MYSQL_ROOT_PASSWORD=test registry.adinusa.id/btacademy/mariadb:5.5
Unable to find image &#39;registry.adinusa.id/btacademy/mariadb:5.5&#39; locally
5.5: Pulling from btacademy/mariadb
a7344f52cb74: Downloading [=======&gt;                                           ]  9.731MB/67.19MB
515c9bb51536: Download complete 
e1eabe0537eb: Download complete 
4701f1215c13: Download complete 
1f47c10fd782: Download complete 
```

- **Create a wordpress container and connect it to the database container**
```bash
student@pod-jani01-node01:~$ docker container run -d -p 80:80 -P --name ch6_wordpress --memory 512m --cpu-shares 512 \
&gt; --cap-drop net_raw --link ch6_mariadb:mysql -e WORDPRESS_DB_PASSWORD=test \
&gt; registry.adinusa.id/btacademy/wordpress:5.0.0-php7.2-apache
Unable to find image &#39;registry.adinusa.id/btacademy/wordpress:5.0.0-php7.2-apache&#39; locally
5.0.0-php7.2-apache: Pulling from btacademy/wordpress
a5a6f2f73cd8: Downloading [=================&gt;                                 ]  8.027MB/22.49MB
633e0d1cd2a3: Download complete 
fcdfdf7118ba: Downloading [=====&gt;                                             ]  7.569MB/67.43MB
4e7dc76b1769: Download complete 
```

- **Check logs, running process, and resource**
```bash
student@pod-jani01-node01:~$ docker logs ch6_mariadb
Initializing database
240324 23:56:47 [Note] /usr/sbin/mysqld (mysqld 5.5.64-MariaDB-1~trusty) starting as process 73 ...
240324 23:56:48 [Note] /usr/sbin/mysqld (mysqld 5.5.64-MariaDB-1~trusty) starting as process 80 ...

PLEASE REMEMBER TO SET A PASSWORD FOR THE MariaDB root USER !
To do so, start the server, then issue the following commands:

&#39;/usr/bin/mysqladmin&#39; -u root password &#39;new-password&#39;
&#39;/usr/bin/mysqladmin&#39; -u root -h  password &#39;new-password&#39;

Alternatively you can run:
&#39;/usr/bin/mysql_secure_installation&#39;

which will also give you the option of removing the test
databases and anonymous user created by default.  This is
strongly recommended for production servers.

See the MariaDB Knowledgebase at http://mariadb.com/kb or the
MySQL manual for more instructions.
student@pod-jani01-node01:~$ docker logs ch6_wordpress
WordPress not found in /var/www/html - copying now...
Complete! WordPress has been successfully copied to /var/www/html
AH00558: apache2: Could not reliably determine the server&#39;s fully qualified domain name, using 172.17.0.10. Set the &#39;ServerName&#39; directive globally to suppress this message
AH00558: apache2: Could not reliably determine the server&#39;s fully qualified domain name, using 172.17.0.10. Set the &#39;ServerName&#39; directive globally to suppress this message
[Mon Mar 25 00:00:01.114803 2024] [mpm_prefork:notice] [pid 1] AH00163: Apache/2.4.25 (Debian) PHP/7.2.13 configured -- resuming normal operations
[Mon Mar 25 00:00:01.114928 2024] [core:notice] [pid 1] AH00094: Command line: &#39;apache2 -D FOREGROUND&#39;
student@pod-jani01-node01:~$ docker top ch6_mariadb
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
lxd                 12527               12504               0                   Mar24               ?                   00:00:00            mysqld
student@pod-jani01-node01:~$ docker top ch6_wordpress 
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                12871               12847               0                   Mar24               ?                   00:00:00            apache2 -DFOREGROUND
www-data            13090               12871               0                   00:00               ?                   00:00:00            apache2 -DFOREGROUND
www-data            13091               12871               0                   00:00               ?                   00:00:00            apache2 -DFOREGROUND
www-data            13092               12871               0                   00:00               ?                   00:00:00            apache2 -DFOREGROUND
student@pod-jani01-node01:~$ docker stats ch6_mariadb
CONTAINER ID   NAME          CPU %     MEM USAGE / LIMIT   MEM %     NET I/O           BLOCK I/O      PIDS
a0a6e14a010f   ch6_mariadb   0.13%     82.8MiB / 256MiB    32.34%    2.83kB / 1.67kB   4.6MB / 49MB   20
student@pod-jani01-node01:~$ docker stats ch6_wordpress
CONTAINER ID   NAME            CPU %     MEM USAGE / LIMIT   MEM %     NET I/O           BLOCK I/O     PIDS
e33b8f77419c   ch6_wordpress   0.01%     21.12MiB / 512MiB   4.13%     1.67kB / 1.75kB   0B / 42.6MB   6
```

- **Test browsing**
Access to http//10.10.10.11/
![image](https://hackmd.io/_uploads/ryOKyBCRa.png)
