**Chapter 3: Managing Docker Container**
-
**1. Managing the Life Cycle of Containers - 1**
Docker has a number of commands that can be used to create and manage containers. The following is a summary of the most frequently used commands to change container state.
![H158FiRaa](https://hackmd.io/_uploads/HkwvmIC0p.png)
Apart from that, Docker also provides a number of commands to get information about running and stopped containers. The following is a summary of the most frequently used commands to retrieve information regarding Docker containers.
![ByJdKiC6T](https://hackmd.io/_uploads/SJzt7LRCp.png)

**2. Managing the Life Cycle of Containers - 2**
- **Managing Containers**

Docker provides the following commands for managing containers:
**docker ps** : This command is responsible for displaying the list of running containers.
**CONTAINER ID** : Each container, when created, gets a container ID, which is a hexadecimal number and looks like an image ID, but is actually unrelated.
**IMAGE** : The container image used to start the container.
**COMMAND** : Commands that are executed when the container starts.
**CREATED** : The date and time the container was started.
**STATUS** : The total time the container was running, if it is still running, or the time since it was closed.
**PORT** : The port exposed by the container or port forwarding, if configured.
**NAME** : Container name.

- **Docker Network**

One of the reasons for the power of Docker containers and services is their ability to interconnect or interface with non-Docker workloads. Docker containers and services don&#39;t need to know that they run on Docker or whether their counterparts also use Docker. Whether your Docker host runs on Linux, Windows, or both, you can manage it platform-agnostically with Docker.

In this discussion, we will define some basic concepts about Docker networking to prepare you to design and deploy applications to take full advantage of these capabilities.

**bridge** : The default network driver.
**host** : Remove network isolation between the container and the Docker host.
**overlay** : Overlay networks connect multiple Docker daemons together.
**ipvlan** : IPvlan networks provide full control over both IPv4 and IPv6 addressing.
**macvlan** : Assign a MAC address to a container.
**none** : Completely isolate a container from the host and other containers.

- **Docker Network Plugin**

You can also use third-party networking plugins with Docker. This plugin can be downloaded from Docker Hub or other vendors. 

**3. Mount Volume - part 1**

**Execute in node01**

- **Create working directory**
```bash
student@pod-jani01-node01:~$ cd $HOME
student@pod-jani01-node01:~$ mkdir -p latihan/latihan01-volume
student@pod-jani01-node01:~$ cd latihan/latihan01-volume
student@pod-jani01-node01:~/latihan/latihan01-volume$
```

- **Create file and container for data volume with name my-volume**
```bash
student@pod-jani01-node01:~/latihan/latihan01-volume$ for i in {1..10};do touch file-$i;done
student@pod-jani01-node01:~/latihan/latihan01-volume$ sudo docker create -v /my-volume --name my-volume registry.adinusa.id/btacademy/busybox
Unable to find image &#39;registry.adinusa.id/btacademy/busybox:latest&#39; locally
latest: Pulling from btacademy/busybox
4b35f584bb4f: Pull complete 
Digest: sha256:acaddd9ed544f7baf3373064064a51250b14cfe3ec604d65765a53da5958e5f5
Status: Downloaded newer image for registry.adinusa.id/btacademy/busybox:latest
6e27dc0e3407bca8e82b528b6c7c413ce9511c8b15c216476ba6243b004e35e7
student@pod-jani01-node01:~/latihan/latihan01-volume$ sudo docker cp . my-volume:/my-volume
Successfully copied 6.66kB to my-volume:/my-volume
```

- **Mount the volume into the container**
```bash
student@pod-jani01-node01:~/latihan/latihan01-volume$ docker run --volumes-from my-volume registry.adinusa.id/btacademy/ubuntu ls /my-volume
file-1
file-10
file-2
file-3
file-4
file-5
file-6
file-7
file-8
file-9
```

**4. Mount Volume with NFS Server**

**Execute in node01**

- **Create working directory**
```bash
student@pod-jani01-node01:~$ sudo mkdir -p /data/nfs-storage01/
```

- **Create the NFS Server with docker**
```bash
student@pod-jani01-node01:~$ docker run -itd --privileged --restart unless-stopped -e SHARED_DIRECTORY=/data -v /data/nfs-storage01:/data -p 2049:2049 registry.adinusa.id/btacademy/nfs-server-alpine:12
Unable to find image &#39;registry.adinusa.id/btacademy/nfs-server-alpine:12&#39; locally
12: Pulling from btacademy/nfs-server-alpine
bdf0201b3a05: Pull complete 
8e751f03d47e: Pull complete 
68ecfeaf6b18: Extracting [=================================================&gt; ]  4.981MB/5.022MB
```

**Execute in node02**

- **Install the necessary packages, mount the volume on the NFS client and then create a file to check out the volume**
```bash
student@pod-jani01-node02:~$ ssh 10.7.7.20
Warning: Permanently added &#39;10.7.7.20&#39; (ED25519) to the list of known hosts.
Welcome to Ubuntu 22.04.1 LTS (GNU/Linux 5.15.0-46-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Mon Mar 25 13:01:33 UTC 2024

  System load:  0.0                Users logged in:          1
  Usage of /:   18.3% of 17.87GB   IPv4 address for docker0: 172.17.0.1
  Memory usage: 8%                 IPv4 address for ens33:   192.168.22.12
  Swap usage:   0%                 IPv4 address for ens34:   10.10.10.12
  Processes:    222                IPv4 address for ens35:   10.7.7.20
```

```bash
student@pod-jani01-node02:~$ sudo apt install nfs-client -y
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Note, selecting &#39;nfs-common&#39; instead of &#39;nfs-client&#39;
The following additional packages will be installed:
  keyutils libnfsidmap1 rpcbind
Suggested packages:
  watchdog
The following NEW packages will be installed:
  keyutils libnfsidmap1 nfs-common rpcbind
```

```bash
student@pod-jani01-node02:~$ sudo mount -v -t nfs4 10.7.7.10:/ /mnt
mount.nfs4: timeout set for Mon Mar 25 13:10:45 2024
mount.nfs4: trying text-based options &#39;vers=4.2,addr=10.7.7.10,clientaddr=10.7.7.20&#39;
student@pod-jani01-node02:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           393M  1.3M  392M   1% /run
/dev/sda1        18G  3.3G   15G  19% /
tmpfs           2.0G     0  2.0G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
/dev/sda15      105M  5.3M  100M   5% /boot/efi
tmpfs           393M  4.0K  393M   1% /run/user/1000
10.7.7.10:/      18G  5.7G   13G  32% /mnt
```

```bash
student@pod-jani01-node02:~$ sudo touch /mnt/file1.txt
student@pod-jani01-node02:~$ sudo touch /mnt/file2.txt
```

**Execute in node01**

- **Verification**
```bash
student@pod-jani01-node01:~$ ls /data/nfs-storage01/
file1.txt  file2.txt
```

**5. Mount Volume with Read-only Mode**

**Execute in node01**

- **Create docker volume**
```bash
student@pod-jani01-node01:~$ docker volume create my-vol
my-vol
```

- **Display list docker volume**
```bash
student@pod-jani01-node01:~$ docker volume ls
DRIVER    VOLUME NAME
local     3bc31f280461ce4ade414ee60dbc950bd38843d183544fb460c8107e84ebe486
local     4e80ae9008cf859ae480cb78444994810b9d2d6a40f6f16af6bca363a351d0dd
local     my-vol
```

- **Display docker volume details**
```bash
student@pod-jani01-node01:~$ docker volume inspect my-vol
[
    {
        &#34;CreatedAt&#34;: &#34;2024-03-25T13:14:22Z&#34;,
        &#34;Driver&#34;: &#34;local&#34;,
        &#34;Labels&#34;: null,
        &#34;Mountpoint&#34;: &#34;/var/lib/docker/volumes/my-vol/_data&#34;,
        &#34;Name&#34;: &#34;my-vol&#34;,
        &#34;Options&#34;: null,
        &#34;Scope&#34;: &#34;local&#34;
    }
]
```

- **Running the container with volume read and write access**
```bash
student@pod-jani01-node01:~$ docker run -d --name=nginx-rwvol -v my-vol:/usr/share/nginx/html -p 4005:80 registry.adinusa.id/btacademy/nginx:latest
719b535e9523ebb4a620d51c29628d1a48a620c28c129860a4186ed6eea13e1c
```

- **Display the IP Address of the container**
```bash
student@pod-jani01-node01:~$ docker inspect nginx-rwvol | jq -r &#39;.[].NetworkSettings.IPAddress&#39;
172.17.0.3
```

- **Test browsing to IP Address of the container**
```bash
student@pod-jani01-node01:~$ curl 172.17.0.3
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
&lt;title&gt;Welcome to nginx!&lt;/title&gt;
&lt;style&gt;
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
```

- **Create index.html file and move it to the source volume directory**
```bash
student@pod-jani01-node01:~$ docker run -d --name=nginx-rovol -v my-vol:/usr/share/nginx/html:ro registry.adinusa.id/btacademy/nginx:latest
57b3cb68e9d7802baa501d16f23f5d376fe2193ec1ef290fb8b4bdbe9d173968
```

- **View the nginx-rovol container details and pay attention to the Mode in Mounts section**
```bash
student@pod-jani01-node01:~$ docker inspect nginx-rovol | jq &#39;.[].Mounts&#39;
[
  {
    &#34;Type&#34;: &#34;volume&#34;,
    &#34;Name&#34;: &#34;my-vol&#34;,
    &#34;Source&#34;: &#34;/var/lib/docker/volumes/my-vol/_data&#34;,
    &#34;Destination&#34;: &#34;/usr/share/nginx/html&#34;,
    &#34;Driver&#34;: &#34;local&#34;,
    &#34;Mode&#34;: &#34;ro&#34;,
    &#34;RW&#34;: false,
    &#34;Propagation&#34;: &#34;&#34;
  }
]
```

**6. Volume Driver**

**Execute in node02**

- **Create directory /share and Create index.html**
```bash
student@pod-jani01-node02:~$ sudo mkdir /share
student@pod-jani01-node02:~$ sudo chmod 777 /share
student@pod-jani01-node02:~$ echo &#34;Hello Adinusa!&#34; &gt; /share/index.html
```

**Execute in node01**

- **Instal plugin sshfs and set plugin sshfs**
```bash
student@pod-jani01-node01:~$ docker plugin install --grant-all-permissions vieux/sshfs
latest: Pulling from vieux/sshfs
Digest: sha256:1d3c3e42c12138da5ef7873b97f7f32cf99fb6edde75fa4f0bcf9ed277855811
52d435ada6a4: Complete 
Installed plugin vieux/sshfs
student@pod-jani01-node01:~$ docker plugin ls
ID             NAME                 DESCRIPTION               ENABLED
4eb5df45fc0b   vieux/sshfs:latest   sshFS plugin for Docker   true
student@pod-jani01-node01:~$ docker plugin disable 4eb5df45fc0b
4eb5df45fc0b
student@pod-jani01-node01:~$ docker plugin set vieux/sshfs sshkey.source=/home/student/.ssh/
student@pod-jani01-node01:~$ docker plugin enable 4eb5df45fc0b
4eb5df45fc0b
student@pod-jani01-node01:~$ docker plugin ls
ID             NAME                 DESCRIPTION               ENABLED
4eb5df45fc0b   vieux/sshfs:latest   sshFS plugin for Docker   true
```

- **Creating docker volume with driver sshfs**
```bash
student@pod-jani01-node01:~$ docker volume create --driver vieux/sshfs -o sshcmd=student@10.7.7.20:/share -o allow_other sshvolume
sshvolume
```

- **Test running container with the volume**
```bash
student@pod-jani01-node01:~$ docker run -d --name=nginxtest-sshfs -p 8090:80 -v sshvolume:/usr/share/nginx/html registry.adinusa.id/btacademy/nginx:latest
e53b51ad7f50ae48532614af3fd0a50b67f23ab2884732d2903cbab1b1ff9ef4
```

- **Test browsing**
```bash
student@pod-jani01-node01:~$ docker ps
CONTAINER ID   IMAGE                                                COMMAND                  CREATED          STATUS          PORTS                                       NAMES
e53b51ad7f50   registry.adinusa.id/btacademy/nginx:latest           &#34;/docker-entrypoint.…&#34;   47 seconds ago   Up 45 seconds   0.0.0.0:8090-&gt;80/tcp, :::8090-&gt;80/tcp       nginxtest-sshfs
student@pod-jani01-node01:~$ curl http://localhost:8090
Hello Adinusa!
```

**7. Default Bridge Network**

**Execute in node01**

- **Display a list of networks on the current docker network**
```bash
student@pod-jani01-node01:~$ sudo docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
34fcfe1fcfbc   bridge    bridge    local
ac3acf6f40a0   host      host      local
3116eb5918d0   none      null      local
```

- **Run 2 alpine container running ash shell**
```bash
student@pod-jani01-node01:~$ docker run -dit --name alpine1 registry.adinusa.id/btacademy/alpine ash
Unable to find image &#39;registry.adinusa.id/btacademy/alpine:latest&#39; locally
latest: Pulling from btacademy/alpine
f56be85fc22e: Pull complete 
Digest: sha256:b6ca290b6b4cdcca5b3db3ffa338ee0285c11744b4a6abaa9627746ee3291d8d
Status: Downloaded newer image for registry.adinusa.id/btacademy/alpine:latest
c6abede0026d33a89f9f287f97641db07e0ff73d1bf624bf26dd43c23cf4613c
student@pod-jani01-node01:~$ docker run -dit --name alpine2 registry.adinusa.id/btacademy/alpine ash
1ff4e262cc9bab08de7d500780524f0414f6d980dbe624df06a3e7f5bea324c8
student@pod-jani01-node01:~$ docker ps -a
CONTAINER ID   IMAGE                                                         COMMAND                  CREATED          STATUS                      PORTS                                       NAMES
1ff4e262cc9b   registry.adinusa.id/btacademy/alpine                          &#34;ash&#34;                    9 seconds ago    Up 8 seconds                                                            alpine2
c6abede0026d   registry.adinusa.id/btacademy/alpine                          &#34;ash&#34;                    14 seconds ago   Up 14 seconds                                                           alpine1
```

- **Create a new docker network and attach it to the alpine1 container**
```bash
student@pod-jani01-node01:~$ docker network create --driver bridge bridge1
ed7dfb02bacdc00a1649baf6059adbb8fd1ec1233762602b33eaae1efdf87815
student@pod-jani01-node01:~$ docker network connect bridge1 alpine1
```

- **Check the IP address of the alpine2 container**
```bash
student@pod-jani01-node01:~$ docker inspect alpine2 | jq -r &#39;.[].NetworkSettings.IPAddress&#39;
172.17.0.7
```

- **Access to alpine1 container**
```bash
student@pod-jani01-node01:~$ docker exec -it alpine1 sh
/ #
```

- **Display the IP address of container alpine1**
```bash
/ # ip add
1: lo: &lt;LOOPBACK,UP,LOWER_UP&gt; mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
16: eth0@if17: &lt;BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN&gt; mtu 1500 qdisc noqueue state UP 
    link/ether 02:42:ac:11:00:06 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.6/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:acff:fe11:6/64 scope link 
       valid_lft forever preferred_lft forever
21: eth1@if22: &lt;BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN&gt; mtu 1500 qdisc noqueue state UP 
    link/ether 02:42:ac:12:00:02 brd ff:ff:ff:ff:ff:ff
    inet 172.18.0.2/16 brd 172.18.255.255 scope global eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::42:acff:fe12:2/64 scope link 
       valid_lft forever preferred_lft forever
```

- **Ping test to the internet on container alpine1 (Success)**
```bash
/ # ping -c 3 8.8.8.8
PING 8.8.8.8 (8.8.8.8): 56 data bytes
64 bytes from 8.8.8.8: seq=0 ttl=127 time=228.150 ms
64 bytes from 8.8.8.8: seq=1 ttl=127 time=30.347 ms
64 bytes from 8.8.8.8: seq=2 ttl=127 time=24.894 ms

--- 8.8.8.8 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 24.894/94.463/228.150 ms
```

- **Ping test to IP Address of alpine2 container (Success)**
```bash
/ # ping -c 3 172.17.0.7
PING 172.17.0.7 (172.17.0.7): 56 data bytes
64 bytes from 172.17.0.7: seq=0 ttl=64 time=0.303 ms
64 bytes from 172.17.0.7: seq=1 ttl=64 time=0.415 ms
64 bytes from 172.17.0.7: seq=2 ttl=64 time=0.157 ms

--- 172.17.0.7 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.157/0.291/0.415 ms
```

- **Ping test to name of alpine2 container (failed)**
```bash
/ # ping -c 3 alpine2
ping: bad address &#39;alpine2&#39;
```

**8. Host Network**

**Execute in node01**

- **Run the container from the nginx image with the network host**
```bash
student@pod-jani01-node01:~$ docker run -itd --network host --name my_nginx registry.adinusa.id/btacademy/nginx
2ce71535f812fd1baa244d40b145a15a1e8636826750979966b2e12e674e1fb0
```

- **Test browsing to localhost**
```bash
student@pod-jani01-node01:~$ curl http://localhost
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
&lt;title&gt;Welcome to nginx!&lt;/title&gt;
&lt;style&gt;
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
&lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;h1&gt;Welcome to nginx!&lt;/h1&gt;
&lt;p&gt;If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.&lt;/p&gt;

&lt;p&gt;For online documentation and support please refer to
&lt;a href=&#34;http://nginx.org/&#34;&gt;nginx.org&lt;/a&gt;.&lt;br/&gt;
Commercial support is available at
&lt;a href=&#34;http://nginx.com/&#34;&gt;nginx.com&lt;/a&gt;.&lt;/p&gt;

&lt;p&gt;&lt;em&gt;Thank you for using nginx.&lt;/em&gt;&lt;/p&gt;
&lt;/body&gt;
&lt;/html&gt;
```

- **Verification IP Address and bound port 80**
```bash
student@pod-jani01-node01:~$ ip add
1: lo: &lt;LOOPBACK,UP,LOWER_UP&gt; mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: &lt;BROADCAST,MULTICAST,UP,LOWER_UP&gt; mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 00:0c:29:16:79:a9 brd ff:ff:ff:ff:ff:ff
    altname enp2s1
    inet 192.168.22.11/24 brd 192.168.22.255 scope global ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::20c:29ff:fe16:79a9/64 scope link 
       valid_lft forever preferred_lft forever
3: ens34: &lt;BROADCAST,MULTICAST,UP,LOWER_UP&gt; mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 00:0c:29:16:79:b3 brd ff:ff:ff:ff:ff:ff
    altname enp2s2
    inet 10.10.10.11/24 brd 10.10.10.255 scope global ens34
       valid_lft forever preferred_lft forever
    inet6 fe80::20c:29ff:fe16:79b3/64 scope link 
       valid_lft forever preferred_lft forever
4: ens35: &lt;BROADCAST,MULTICAST,UP,LOWER_UP&gt; mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 00:0c:29:16:79:bd brd ff:ff:ff:ff:ff:ff
    altname enp2s3
    inet 10.7.7.10/24 brd 10.7.7.255 scope global ens35
       valid_lft forever preferred_lft forever
    inet6 fe80::20c:29ff:fe16:79bd/64 scope link 
       valid_lft forever preferred_lft forever
5: docker0: &lt;BROADCAST,MULTICAST,UP,LOWER_UP&gt; mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:2c:b3:7d:d0 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:2cff:feb3:7dd0/64 scope link 
       valid_lft forever preferred_lft forever
student@pod-jani01-node01:~$ sudo netstat -tulpn | grep :80
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      8528/nginx: master  
tcp        0      0 0.0.0.0:8090            0.0.0.0:*               LISTEN      6297/docker-proxy   
tcp6       0      0 :::80                   :::*                    LISTEN      8528/nginx: master  
tcp6       0      0 :::8090                 :::*                    LISTEN      6303/docker-proxy
```
