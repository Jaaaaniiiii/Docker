**Chapter 7: Docker Swarm &amp; Portainer**
-
**1. What is Swarm?**
Swarm is a collection of computers running Docker, either one or more machines. With swarm mode, Docker gets built-in orchestration and clustering capabilities. Features such as load balancing and service lifecycle management are included. Swarm mode is off by default when running containers with Docker commands, but if enabled, you can manage services rather than containers directly.

**About Docker Swarm**
Docker Swarm is a group of physical or virtual machines that run Docker applications and have been configured to join together in a cluster. Once a group of machines is grouped together, you can still run Docker commands as usual, but now they will be executed by the machines in your cluster. Cluster activity is controlled by the swarm manager, and the machines that have joined the cluster are called nodes.
![1 cGtPijc0aQU0lRYq2SEFpw](https://hackmd.io/_uploads/H1mYeKkkC.png)

Docker Swarm Features:
- Integrated cluster management with Docker Engine: Use the Docker Engine CLI to create clusters from Docker Engines where you can deploy application services without the need for additional orchestration software.
- Decentralized design: Docker Engine handles specialization at runtime, allowing you to deploy both types of nodes (managers and workers) from a single disk image.
- Declarative service model: Docker Engine uses a declarative approach to define the state of services in your application stack.
- Scaling: You can declare the number of tasks for each service and swarm manager automatically adjusts the number of tasks as needed.
- Desired state reconciliation: Swarm manager monitors the state of the cluster and resolves differences between the actual and desired state.
- Multi-host network: You can define an overlay network for your service and swarm manager automatically assigns addresses to containers.
- Service discovery: Each service in the swarm is given a unique DNS name and is selected to load running containers.
- Load balancing: You can expose service ports to an external load balancer and define how to distribute service containers between nodes internally.
- Security by default: Each node implements TLS mutual authentication and encryption to secure communications between nodes.
- Incremental updates: You can apply service updates to nodes in stages by controlling the delay between service deployments to each set of nodes.

**Alternative container orchestration**
- **Kubernetes**
![0 0upfXtjqscQ5NQfN](https://hackmd.io/_uploads/rJ0ZVFkkR.png)
Kubernetes is an open-source platform used to manage containerized application workloads, as well as providing declarative configuration and automation. Kubernetes sits within a large and fast-growing ecosystem. Kubernetes service, support, and tools are widely available.

- **OpenShift**
![OpenShift-1](https://hackmd.io/_uploads/SJ5eStkyR.png)
Red Hat OpenShift is a cloud-based Kubernetes platform that helps developers build applications. It offers automated installation, upgrades, and life cycle manageme

**2. Portainer for Docker**
**About Portainer**
Portainer is a popular container management tool with many users and extensive GitHub support. It lets you manage Docker containers and Swarm clusters with a light and simple interface. Portainer can run on multiple platforms and makes it easy to manage your Docker resources such as containers, images, and networking. With an easy-to-understand interface, Portainer is suitable for administrators and developers of all skill levels.
![Untitled](https://hackmd.io/_uploads/SJaTUK1k0.png)

**3. Create Swarm**

**Execute in node01**

- **Initialization Docker Swarm**
```bash
student@pod-jani01-node01:~$ docker swarm init --advertise-addr 10.7.7.10
Swarm initialized: current node (1y31gdei4uolojyh7elo6khc8) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-1eqb5cevnu8x1tl8e13xtv5yeftnp8usho573c631jpcszb3lf-7oou9hao4w2s2ozioavg0ch9l 10.7.7.10:2377

To add a manager to this swarm, run &#39;docker swarm join-token manager&#39; and follow the instructions.
```

**Execute in node02**

- **Copy command docker join**
```bash
student@pod-jani01-node02:~$ docker swarm join --token SWMTKN-1-1eqb5cevnu8x1tl8e13xtv5yeftnp8usho573c631jpcszb3lf-7oou9hao4w2s2ozioavg0ch9l 10.7.7.10:2377
This node joined a swarm as a worker.
```

**Execute in node01**

- **Check if node02 already join**
```bash
student@pod-jani01-node01:~$ docker node ls
ID                            HOSTNAME            STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
1y31gdei4uolojyh7elo6khc8 *   pod-jani01-node01   Ready     Active         Leader           26.0.0
u0wgeqh4czdr7wbjud4ep895u     pod-jani01-node02   Ready     Active                          26.0.0
```

**4. Deploy Service to Swarm**

**Execute in node01**

- **Create Service Nginx with 2 replica and expose port to 80**
```bash
student@pod-jani01-node01:~$ docker service create --name web --replicas 2 -p 80:80 registry.adinusa.id/btacademy/nginx:latest
```

- **Check Service Running Correctly in all node**
```bash
student@pod-jani01-node01:~$ docker service ls
ID             NAME      MODE         REPLICAS   IMAGE                                        PORTS
dq5mx5e6iz2e   web       replicated   2/2        registry.adinusa.id/btacademy/nginx:latest   *:80-&gt;80/tcp
```

- **Test Browsing From node01 and node02**
```bash
student@pod-jani01-node01:~$ curl http://10.7.7.10
student@pod-jani01-node02:~$ curl http://10.7.7.20
```

- **To check information about the service**
```bash
student@pod-jani01-node01:~$ docker service inspect --pretty web

ID:             dq5mx5e6iz2e6hityqnv5eblf
Name:           web
Service Mode:   Replicated
 Replicas:      2
Placement:
UpdateConfig:
 Parallelism:   1
 On failure:    pause
```

- **If we want to check where the service is running**
```bash
student@pod-jani01-node01:~$ docker service ps web
ID             NAME      IMAGE                                        NODE                DESIRED STATE   CURRENT STATE           ERROR     PORTS
ro4mrhf4vm10   web.1     registry.adinusa.id/btacademy/nginx:latest   pod-jani01-node02   Running         Running 7 minutes ago             
4puadtin6skr   web.2     registry.adinusa.id/btacademy/nginx:latest   pod-jani01-node01   Running         Running 7 minutes ago
```

- **Create container with limit cpu and limit Memory**
```bash
student@pod-jani01-node01:~$ docker service create --name sandyxd18 --reserve-cpu 1 --limit-cpu 1 --reserve-memory 256mb --limit-memory 128mb registry.adinusa.id/btacademy/httpd:latest
```

- **Check the container Spesification**
```bash
student@pod-jani01-node01:~$ docker service inspect --pretty sandyxd18

ID:             oja9dla6ks26g74xxmxfigc52
Name:           sandyxd18
Service Mode:   Replicated
 Replicas:      1
```

**5. Docker swarm Scale &amp; Update**

**Execute in node01**

- **Create Service Nginx with 3 replica**
```bash
student@pod-jani01-node01:~$ docker service create --name web2 --replicas 3 -p 80:80 registry.adinusa.id/btacademy/nginx:latest
```

- **We can change the replica of the service**
```bash
student@pod-jani01-node01:~$ docker service scale web2=1
web2 scaled to 1
```

- **Check if replica already change**
```bash
student@pod-jani01-node01:~$ docker service ps web2
ID             NAME      IMAGE                                        NODE                DESIRED STATE   CURRENT STATE           ERROR     PORTS
g8cjqi0omqf6   web2.1    registry.adinusa.id/btacademy/nginx:latest   pod-jani01-node02   Running         Running 2 minutes ago
```

- **Docker swarm can rolling update the service**
```bash
student@pod-jani01-node01:~$ docker service update --image sistyo/myweb web2
```
![image](https://hackmd.io/_uploads/HyEJ0Yy1R.png)

**6. Install Portainer and configuration**

**Execute in node01**

- **Install Portainer In Node01**
```bash
student@pod-jani01-node01:~$ docker volume create portainer_data
portainer_data
student@pod-jani01-node01:~$ docker run -d -p 8000:8000 -p 9000:9000 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```

- **Access Portainer Dashboard**
Open browser and access to http://10.10.10.11:9000
![image](https://hackmd.io/_uploads/HJWhkqJkA.png)

- **Connect Portainer Agent Node02 to Portainer Dashboard**
Click menu Environments -&gt; Add environmets
![image](https://hackmd.io/_uploads/rJ851-gJR.png)
Select Docker Swarm -&gt; Start Wizard
![image](https://hackmd.io/_uploads/SJl3kZx1C.png)
Select Agent -&gt; Copy command to run in node01
![image](https://hackmd.io/_uploads/BJtAgblJ0.png)

**7. Running Container from Portainer**
- **Access Portainer dashboard**
Access to http://10.10.10.11:9000
![image](https://hackmd.io/_uploads/BkXz-Wg10.png)

- **Running Container**
In Home, choose node02
Click Containers -&gt; Add Container
![image](https://hackmd.io/_uploads/B1WrGbgyC.png)
Fill in the name column with [username]-web
Fill the Image Coloumn with nginx:latest
Node pod-username-node02
![image](https://hackmd.io/_uploads/SkE-mZe1A.png)
Click Deploy the Container
![image](https://hackmd.io/_uploads/rytBQ-xkA.png)

- **Access to container**
Access to http://10.10.10.12:8080
![image](https://hackmd.io/_uploads/r16sQ-xk0.png)

**7. Build &amp; Push Image from Portainer**
- **Create image from Dockerfile**
Cick Menu Image -&gt; + Build a new Image
![image](https://hackmd.io/_uploads/rJ-_BZlkA.png)
Fill name coloumn with {username adinusa}/nginx:lab76
![image](https://hackmd.io/_uploads/SkzTSZlJR.png)
Select build method to Web Editor
![image](https://hackmd.io/_uploads/HJmDLZlkR.png)
Deployment to pod-username-node01 and Click Build the image
![image](https://hackmd.io/_uploads/SybYU-l1C.png)

- **Attach image to Docker Hub**
Click menu Registries -&gt; Add registry
![image](https://hackmd.io/_uploads/Hkxzw-xJ0.png)
In menu registry provider -&gt; Select DockerHub
Name: repo-{username adinusa}
Username: {username docker}
Password: {password docker}
Click Add Registry
![image](https://hackmd.io/_uploads/B1ddPblJC.png)
Back to Images, Open the previously created image
in Registry coloumn select repo-username adinusa
In Image coloumn fill with {username docker}/nginx:lab76 -&gt; Click Tag
Select Push to Registry
