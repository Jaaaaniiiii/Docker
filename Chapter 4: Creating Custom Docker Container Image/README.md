**Chapter 4: Creating Custom Docker Container Image**
-
**Docker images**
What is Docker Images?
Docker images are like &#34;templates&#34; used to create containers. On a Linux system, everything is considered a file, including the operating system itself. So, a Docker image is basically like a big “package” that contains everything needed to run an application in a container. It&#39;s like having a large &#34;archive&#34; containing all the necessary components, such as files and folders, that &#34;stack&#34; on top of each other to form a complete picture.

**The layered filesystem**
The layered file system in a container image is like having several &#34;layers&#34; that are &#34;stacked&#34; on top of each other. Docker images, which are templates for containers, consist of several interrelated layers. The first layer, also called the base layer, is the basis of the image. This is similar to putting together several &#34;puzzles&#34; that make up the overall picture.
![HynH7xy0p](https://hackmd.io/_uploads/r1JHM-1y0.png)

Image as a stack of layers Each individual layer contains files and folders. Each layer only contains changes to the file system of the underlying layer. A storage driver handles the details regarding how these layers interact with each other. Various storage drivers are available that have advantages and disadvantages in different situations.

The layers of a container image are all immutable. Immutable means that once created, the layer can never be changed. The only operation that affects layers is their physical deletion. The stiffness of these layers is important because it opens up many opportunities, as we will see.
In the following image, we can see how a custom image for a web application, using Nginx as the web server, could look.
![S1Yqmey06](https://hackmd.io/_uploads/Sy_wG-1J0.png)

An example of an Alpine and Nginx based custom image
Our base layer here consists of the Alpine Linux distribution. Then, on top of that, we have the Add Nginx layer where Nginx is added on top of Alpine. Finally, the third layer contains all the files that make up the web application, such as HTML, CSS, and JavaScript files.
As mentioned before, each image starts with a base image. Typically, this base image is one of the official images found on Docker Hub, such as a Linux, Alpine, Ubuntu, or CentOS distro. However, it is also possible to create images from scratch.

**Docker Registry**
An overview of Docker Registry
In this section, we will discuss Docker Registry. Docker Registry is an application that you can run anywhere and is used to store your Docker images. We&#39;ll compare Docker Registry and Docker Hub, and discuss how to choose between the two. At the end of this section, you&#39;ll learn how to run Docker Registry yourself and determine if it&#39;s right for you.
![ByF8BeyR6](https://hackmd.io/_uploads/ryEpGZyy0.png)

Docker Registry is an open source application that you can use to store Docker images on the platform of your choice. This allows you to keep the images private or share them as needed.

Docker Registry is suitable for use if you want to have your own registry without having to pay for Docker Hub&#39;s private features. Now, let&#39;s compare Docker Hub and Docker Registry to help you choose the appropriate platform for storing your images.

Docker Registry Features:
- You can manage your own registry from where you can present all repositories as private, public, or a mix of both.
- You can scale the registry as needed, based on the number of images you host or the number of pull requests you serve.
- Everything can be run via the command line.

Docker Hub provides:
- Easy to use GUI based interface to manage your images.
- A place in the cloud that is set up to handle public and/or private images.
- You don&#39;t need to bother managing the server that hosts all your images.

**Docker Hub**
Docker Hub is the easiest way to build, manage, and deliver your team&#39;s container applications.
The Docker Hub repository lets you share container images with your team, customers, or the Docker community at large.

**1. Exploring Dockerfile**

**Execute in node01**

- **Clone repository for training**
```bash
student@pod-jani01-node01:~$ git clone https://github.com/spkane/docker-node-hello.git \
&gt; --config core.autocrlf=input latihan01
Cloning into &#39;latihan01&#39;...
remote: Enumerating objects: 60, done.
remote: Counting objects: 100% (13/13), done.
remote: Compressing objects: 100% (8/8), done.
remote: Total 60 (delta 6), reused 11 (delta 4), pack-reused 47
Receiving objects: 100% (60/60), 8.81 KiB | 2.20 MiB/s, done.
Resolving deltas: 100% (25/25), done.
```

- **Enter the directory**
```bash
student@pod-jani01-node01:~$ cd latihan01
student@pod-jani01-node01:~/latihan01$
```

- **Create an image of the previous repository and name it node-exercise01**
```bash
student@pod-jani01-node01:~/latihan01$ docker build -t node-latihan01 .
[+] Building 85.6s (4/12)                                                                                                                                                                                         docker:default
 =&gt; [internal] load build definition from Dockerfile                                                                                                                                                                        0.0s
 =&gt; =&gt; transferring dockerfile: 569B                                                                                                                                                                                        0.0s
 =&gt; [internal] load metadata for docker.io/library/node:18.13.0                                                                                                                                                             5.0s
 =&gt; [internal] load .dockerignore                                                                                                                                                                                           0.0s
 =&gt; =&gt; transferring context: 45B                                                                                                                                                                                            0.0s
```

- **Run the container with the previously created image and expose the port to 8080**
```bash
student@pod-jani01-node01:~/latihan01$ docker run -d --rm --name node-latihan01 -p 8080:8080 node-latihan01
15a35d9c5e39684852fbe00d7f30f613b0f83aa3b34085bcf36e8b116bdd56b6
```

- **Try access container**
```bash
student@pod-jani01-node01:~/latihan01$ curl localhost:8080
Hello World. Wish you were here.
```

**Dockerfile practice**

**Execute in node02**

- **Create latihan02 directory**
```bash
student@pod-jani01-node02:~$ cd $HOME
student@pod-jani01-node02:~$ mkdir latihan02
student@pod-jani01-node02:~$ cd latihan02
student@pod-jani01-node02:~/latihan02$
```

- **Create Dockerfile as below**
```bash
student@pod-jani01-node02:~/latihan02$ vim Dockerfile
```
![image](https://hackmd.io/_uploads/ryr-IZJyA.png)

- **Create image From Dockerfile**
```bash
student@pod-jani01-node02:~/latihan02$ docker build -t docker-whale .
[+] Building 8.0s (3/5)                                                                                                                              docker:default
 =&gt; [internal] load build definition from Dockerfile                                                                                                           0.1s
 =&gt; =&gt; transferring dockerfile: 174B                                                                                                                           0.0s
 =&gt; [internal] load metadata for registry.adinusa.id/btacademy/whalesay:latest                                                                                 4.1s
 =&gt; [internal] load .dockerignore                                                                                                                              0.0s
 =&gt; =&gt; transferring context: 2B                                                                                                                                0.0s
```

- **Display the image that has been created**
```bash
student@pod-jani01-node02:~/latihan02$ docker image ls
REPOSITORY                                  TAG       IMAGE ID       CREATED         SIZE
docker-whale                                latest    5cb6113789b2   6 seconds ago   278MB
```

- **Test run the image**
```bash
student@pod-jani01-node02:~/latihan02$ docker run docker-whale
 _________________________________ 
/ Men are always ready to respect \
| anything that bores them.       |
|                                 |
\ -- Marilyn Monroe               /
 --------------------------------- 
    \
     \
      \     
                    ##        .            
              ## ## ##       ==            
           ## ## ## ##      ===            
       /&#34;&#34;&#34;&#34;&#34;&#34;&#34;&#34;&#34;&#34;&#34;&#34;&#34;&#34;&#34;&#34;___/ ===        
  ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~   
       \______ o          __/            
        \    \        __/             
          \____\______/
```

- **Display the container**
```bash
student@pod-jani01-node02:~/latihan02$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
student@pod-jani01-node02:~/latihan02$ docker container ls -a
CONTAINER ID   IMAGE                                        COMMAND                  CREATED              STATUS                          PORTS     NAMES
f3b96d38f71d   docker-whale                                 &#34;/bin/sh -c &#39;/usr/ga…&#34;   About a minute ago   Exited (0) About a minute ago             adoring_leavitt
```

**2. Exploring Dockerfile (Flask Apps)**

**Execute in node01**

**Dockerfile practice**

- **If you don&#39;t have a Docker ID, register at https://hub.docker.com/signup**

- **Login with Docker ID**
```bash
student@pod-jani01-node01:~$ docker login
Log in with your Docker ID or email address to push and pull images from Docker Hub. If you don&#39;t have a Docker ID, head over to https://hub.docker.com/ to create one.
You can log in with your password or a Personal Access Token (PAT). Using a limited-scope PAT grants better security and is required for organizations using SSO. Learn more at https://docs.docker.com/go/access-tokens/

Username: sandyxd18
Password: 
WARNING! Your password will be stored unencrypted in /home/student/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```

- **Create directory latihan03**
```bash
student@pod-jani01-node01:~$ cd $HOME
student@pod-jani01-node01:~$ mkdir latihan03
student@pod-jani01-node01:~$ cd latihan03
student@pod-jani01-node01:~/latihan03$
```

- **Create file flask**
```bash
student@pod-jani01-node01:~/latihan03$ vim app.py
```
![image](https://hackmd.io/_uploads/SyAzFZkJR.png)

- **Create file requirements.txt**
```bash
student@pod-jani01-node01:~/latihan03$ vim requirements.txt
```
![image](https://hackmd.io/_uploads/HJuQT-k10.png)

- **Creating Dockerfile**
```bash
student@pod-jani01-node01:~/latihan03$ vim Dockerfile
```
![image](https://hackmd.io/_uploads/S1ePc-kkR.png)

- **Create image from Dockerfile**
```bash
student@pod-jani01-node01:~/latihan03$ docker build -t flask-latihan03 .
[+] Building 5.7s (4/11)                                                                                                                                                                                          docker:default
 =&gt; [internal] load build definition from Dockerfile                                                                                                                                                                        0.0s
 =&gt; =&gt; transferring dockerfile: 303B                                                                                                                                                                                        0.0s
 =&gt; [internal] load metadata for registry.adinusa.id/btacademy/ubuntu:16.04                                                                                                                                                 3.6s
 =&gt; [internal] load .dockerignore                                                                                                                                                                                           0.0s
 =&gt; =&gt; transferring context: 2B                                                                                                                                                                                             0.0s
```

- **Tag image with docker username**
```bash
student@pod-jani01-node01:~/latihan03$ docker tag flask-latihan03 sandyxd18/flask-latihan03:latest
```

- **Push image to dockerhub**
```bash
student@pod-jani01-node01:~/latihan03$ docker push sandyxd18/flask-latihan03:latest 
The push refers to repository [docker.io/sandyxd18/flask-latihan03]
387edf65731a: Pushing [==================================================&gt;]  5.748MB
5f70bf18a086: Pushing  1.024kB
ccc4e13922db: Pushing [==================================================&gt;]  4.608kB
```

- **Running the image**
```bash
student@pod-jani01-node01:~/latihan03$ docker run -d -p 5000:5000 --name flask03 sandyxd18/flask-latihan03
24de3a55da98c37dae6837f9ac3bf8152a75b453e2956ce8e97627f4e228eb6d
```

- **Test browsing**
```bash
student@pod-jani01-node01:~/latihan03$ curl localhost:5000
Hey, we have Flask in a Docker container!
```
