**Chapter 10: Security**
-
**1. Kernel Namespaces**
Docker containers are very similar to LXC containers, and have similar security features. When starting a container by running docker, behind the scenes, Docker creates a set of namespaces and control groups for the container. Namespaces provide the first and easiest form of isolation: processes running inside a container cannot see, and do not even affect, processes running in other containers, or on the host system. Each container also gets its own networking stack, meaning that a container doesn&#39;t get privileged access to another container&#39;s sockets or interfaces. Of course, if the host system is set up correctly, containers can interact with each other through their respective network interfaces - just as they interact with external hosts. When you specify a public port for your containers or use a link, IP traffic is allowed between containers. They can ping each other, send/receive UDP packets, and establish TCP connections, but can be restricted if necessary. From a network architecture perspective, all containers on a Docker host reside on a bridge interface. This means that they are like physical machines connected via a common Ethernet switch; nothing more, nothing less.

**2. Control Groups**
Control Groups are another key component of Linux Containers. This group enforces calculations and resource limitations. These groups provide many useful metrics, but also help ensure that each container gets its fair share of memory, CPU, disk I/O; and, more importantly, that a single container cannot crash the system by consuming any of these resources. So, while they don&#39;t play a role in preventing one container from accessing or affecting another container&#39;s data and processes, they are critical to fending off some denial-of-service attacks. They are especially important on multi-tenant platforms, such as public and private PaaS, to guarantee consistent uptime (and performance) even when some applications start to misbehave.

Docker containers, by default, are quite secure, especially if you run your processes as an unprivileged user inside the container.

**3. CIS Docker Benchmark**
CIS creates best practices for cyber security and defense. CIS uses crowdsourcing to determine its security recommendations. CIS Benchmark is one of the most popular tools.

Organizations can use CIS Benchmark for Docker to validate that their Docker containers and Docker runtime are configured as securely as possible. There are commercial and open source tools that can automatically check your Docker environment against the recommendations set out in the CIS Benchmark for Docker to identify unsafe configurations.

CIS Benchmark for Docker provides a number of useful configuration checks, but organizations should consider it a starting point and go beyond the CIS checks to ensure best practices are implemented. Setting resource limits, reducing privileges, and ensuring images run in read-only mode are some examples of additional checks you need to run on your container files.

**4. Secure Computing Mode**
Safe computing mode (seccomp) is a Linux kernel feature. You can use it to limit the actions available within a container. The seccomp() system call operates on the seccomp state of the calling process. You can use this feature to restrict your app&#39;s access.

**5. Secret**
Secrets are blobs of data that should not be sent over a network or stored unencrypted in a Dockerfile or in your application&#39;s source code.
