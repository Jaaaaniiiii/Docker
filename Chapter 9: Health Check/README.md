**Chapter 9: Health Check**
-
The HEALTHCHECK instruction tells Docker how to test containers to check whether they are still functional. This can detect cases such as a web server being stuck in an infinite loop and unable to handle new connections, even though the server process is still running.

When a container has a health check defined, the container has a health state other than its normal state. This status is initially started. Each time a health check passes, the container will become healthy (whatever its previous state). After a number of consecutive failures, the container becomes unhealthy.

**1. Health Check**

**Execute in node01**

- **Create working directory**
```bash
student@pod-jani01-node01:~$ cd $HOME
student@pod-jani01-node01:~$ mkdir hc-latihan01
student@pod-jani01-node01:~$ cd hc-latihan01
student@pod-jani01-node01:~/hc-latihan01$
```

- **Create Dockerfile**
```bash
student@pod-jani01-node01:~/hc-latihan01$ vim Dockerfile
```
![image](https://hackmd.io/_uploads/HkXCd9X1A.png)

- **Create image from Dockerfile**
```bash
student@pod-jani01-node01:~/hc-latihan01$ docker build -t http-healthcheck .
```

- **Run image**
```bash
student@pod-jani01-node01:~/hc-latihan01$ docker run -d -p 80:80 --name http-healthcheck http-healthcheck
```

- **Check running container**
```bash
student@pod-jani01-node01:~/hc-latihan01$ docker ps -a
CONTAINER ID   IMAGE                                                COMMAND                  CREATED          STATUS                   PORTS                                                                                            NAMES
162ba8ed10a9   http-healthcheck                                     &#34;/app&#34;                   5 minutes ago    Up 5 minutes (healthy)   0.0.0.0:80-&gt;80/tcp, :::80-&gt;80/tcp                                                                http-healthcheck
```

- **Check with curl**
```bash
student@pod-jani01-node01:~/hc-latihan01$ curl http://localhost
&lt;h1&gt;A healthy request was processed by host: 162ba8ed10a9&lt;/h1&gt;
```

- **Check for unhealthy**
```bash
student@pod-jani01-node01:~/hc-latihan01$ curl http://localhost/unhealthy
```

**Health Check practice**

- **Create a new directory**
```bash
student@pod-jani01-node01:~$ cd $HOME
student@pod-jani01-node01:~$ mkdir hc-latihan02
student@pod-jani01-node01:~$ cd hc-latihan02
student@pod-jani01-node01:~/hc-latihan02$
```

- **Create server.js file**
```bash
student@pod-jani01-node01:~/hc-latihan02$ vim server.js
```
![image](https://hackmd.io/_uploads/HyaFGoQyR.png)

- **Create Dockerfile**
```bash
student@pod-jani01-node01:~/hc-latihan02$ vim Dockerfile
```
![image](https://hackmd.io/_uploads/r1KcSjX10.png)

- **Create image from dockerfile**
```bash
student@pod-jani01-node01:~/hc-latihan02$ docker build -t node-server .
```

- **Run image**
```bash
student@pod-jani01-node01:~/hc-latihan02$ docker run -d –name nodeserver -p 8080:8080 -p 8081:8081 node-server
```

- **Check running container**
```bash
student@pod-jani01-node01:~/hc-latihan02$ curl 127.0.0.1:8080
OK
student@pod-jani01-node01:~/hc-latihan02$ curl 127.0.0.1:8081
Shutting down…
```
