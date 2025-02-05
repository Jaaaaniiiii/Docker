**Chpater 5: Docker Compose**
-
**Introducing Docker Compose**
![MainImage-2](https://hackmd.io/_uploads/rk9iTG1JA.jpg)

Docker Compose is a tool provided by Docker for running and organizing containers on a single Docker host. It is used for development, continuous integration, automated testing, etc. The Docker Compose configuration file uses YAML format and is usually called docker-compose.yml. This file is used to describe and run a containerized application, perhaps with multiple containers. A declarative approach means we state what we want from the application without needing to specify specific steps. This differs from the imperative approach, which orders the system step by step.

Docker recommends a declarative approach because it provides ease in managing containerized applications. Docker Compose follows this approach in its configuration.

**1. Using Docker Compose**

**Execute in all node**

- **Download &amp; Install Compose**
```bash
student@pod--node01:~$ VERSION=$(curl --silent https://api.github.com/repos/docker/compose/releases/latest | jq .name -r) &amp;&amp; \
&gt; DESTINATION=/usr/bin/docker-compose &amp;&amp; \
&gt; sudo curl -sL https://github.com/docker/compose/releases/download/${VERSION}/docker-compose-$(uname -s)-$(uname -m) -o $DESTINATION 
student@pod--node01:~$ sudo chmod 755 $DESTINATION
```

- **Test installation Compose**
```bash
student@pod--node01:~$ docker-compose --version
Docker Compose version v2.26.0
```
**Execute in node01**

- **Create a directory my_wordpress and go to the directory**
```bash
student@pod--node01:~$ cd $HOME
student@pod--node01:~$ mkdir -p latihan/my_wordpress
student@pod--node01:~$ cd latihan/my_wordpress
student@pod--node01:~/latihan/my_wordpress$
```

- **Create docker-compose file**
```bash
student@pod--node01:~/latihan/my_wordpress$ vim docker-compose.yml
```
![image](https://hackmd.io/_uploads/r1BL0_y10.png)

- **Run compose**
```bash
student@pod--node01:~/latihan/my_wordpress$ docker-compose up -d
WARN[0000] /home/student/latihan/my_wordpress/docker-compose.yml: `version` is obsolete 
[+] Running 13/34
 ⠴ db [⠀⣿⣿⣿⣿⣿⡀⣿⠀⠀⠀] Pulling                                                                         12.5s 
   ⠋ e83e8f2e82cc Downloading  6.094MB/50.5MB                                                        8.0s 
   ✔ 0f23deb01b84 Download complete                                                                  0.8s  
```

- **Display the list of containers and test browsing to the wordpress page that has been created**
```bash
student@pod--node01:~/latihan/my_wordpress$ docker ps -a
CONTAINER ID   IMAGE                                                         COMMAND                  CREATED          STATUS                      PORTS                                       NAMES
4b2806747288   registry.adinusa.id/btacademy/wordpress:latest                &#34;docker-entrypoint.s…&#34;   14 seconds ago   Up 13 seconds               0.0.0.0:8000-&gt;80/tcp, :::8000-&gt;80/tcp       my_wordpress-wordpress-1
89a173f3434d   registry.adinusa.id/btacademy/mysql:5.7                       &#34;docker-entrypoint.s…&#34;   14 seconds ago   Up 13 seconds               3306/tcp, 33060/tcp                         my_wordpress-db-1
```
