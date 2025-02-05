**Chapter 1: Introduction to Docker**
-
**1. What is Containerization?**

Containerization is a software deployment process that bundles application code with all the files and libraries needed to run on any infrastructure. Traditionally, to run any application on a computer, you have to install a version that matches your machine&#39;s operating system. For example, you need to install a Windows version of a software package on a Windows machine. However, with containerization, you can create a single software package, or container, that runs on all types of devices and operating systems.

**2. Various application deployment architectures**

![image](https://hackmd.io/_uploads/SklT5rnRp.jpg)
As you can see, for a Traditional Deployment we have three applications, all sharing the same software stack. Running virtual machines allow us to run three applications, running two completely different software stacks. This diagram gives us a lot of insight into the biggest key benefit of Docker, that is, there is no need for a complete operating system every time we need to bring up a new container, which cuts down on the overall size of containers. Since almost all the versions of Linux use the standard kernel models, Docker relies on using the host operating system&#39;s Linux kernel for the operating system it was built upon, such as Red Hat, CentOS, and Ubuntu. 

**3. What is Docker?**
![Docker-Logo](https://hackmd.io/_uploads/S1CM3B30T.png)

Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications. By taking advantage of Docker’s methodologies for shipping, testing, and deploying code quickly, you can significantly reduce the delay between writing code and running it in production.

**4. Alternative runtime for Container use**
- **Podman**
![podman-logo](https://hackmd.io/_uploads/rJ653HhCp.png)

Podman is a daemonless, open source, Linux native tool designed to make it easy to find, run, build, share and deploy applications using Open Containers Initiative (OCI) Containers and Container Images. Podman provides a command line interface (CLI) familiar to anyone who has used the Docker Container Engine. Most users can simply alias Docker to Podman (alias docker=podman) without any problems. Similar to other common Container Engines (Docker, CRI-O, containerd), Podman relies on an OCI compliant Container Runtime (runc, crun, runv, etc) to interface with the operating system and create the running containers. This makes the running containers created by Podman nearly indistinguishable from those created by any other common container engine.

- **Containerd**
![containerd-horizontal-color](https://hackmd.io/_uploads/B1FgaS3Rp.png)

Containerd in simple terms is a container runtime that is, Containerd is a software responsible for running and managing containers on a host system. It is a resource manager which manages the container processes, image, snapshots, container metadata and its dependencies. Going further, Containerd is a daemon for Linux and Windows, that manages the complete container life cycle of its host system from image transfer and storage to container execution and supervision and beyond. So basically Containerd is a complete package for the container lifecycle. Containerd is a CNCF (Cloud Native Community Foundation) graduated project. It was the fifth project to graduate from CNCF on 2019. In this article we will learn about Containerd and why it is consider the secret hero of the cloud native world. 
