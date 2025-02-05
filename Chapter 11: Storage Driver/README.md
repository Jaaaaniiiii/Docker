**Chapter 11: Storage Driver**
-
**1. Docker Storage Drivers**
The storage driver plays a role in providing users with access to the writeable container layer. The storage driver also plays a role in storing and managing images and containers on the user&#39;s Docker host. The following storage drivers can be used in Docker:

**overlay2**: driver for all Linux distributions and requires no additional configuration.
**aufs**: driver for Docker 18.06 and below, when running on Ubuntu which does not support overlay2.
**fuse-overlayfs**: to run Rootless Docker on hosts that do not provide support for rootless overlay2.
**devicemapper**:can be run but requires direct-lvm for production environments, because loopback-lvm, even with zero configuration, has very poor performance.
**btrfs and zfs**: storage is used if there is a supporting file system that allows advanced options, such as creating “snapshots”.
**vfs**: storage is intended for testing purposes, and for situations

**2. Configuring Storage Driver**

**Execute in node01**

- **Edit file daemon.json**
```bash
student@pod-jani01-node01:~$ sudo vim /etc/docker/daemon.json
```
![image](https://hackmd.io/_uploads/BywCx2XJA.png)

- **Restart service docker**
```bash
student@pod-jani01-node01:~$ sudo service docker restart
```

- **Check docker info**
```bash
student@pod-jani01-node01:~$ docker info
```

- **Check with docker pull**
```bash
student@pod-jani01-node01:~$ docker pull registry.adinusa.id/btacademy/ubuntu
```

- **Check the docker directory**
```bash
student@pod-jani01-node01:~$ sudo ls -al /var/lib/docker/vfs/dir/
total 60
drwx--x--- 15 root root 4096 Mar 29 03:13 .
drwx--x---  3 root root 4096 Mar 29 03:11 ..
drwxr-xr-x 17 root root 4096 Mar 29 03:13 1b598df1567f59ede588f8b94cfada8cdbee7959c00cd5ec8d8e07d753eb202f
drwxr-xr-x  5 root root 4096 Mar 29 03:12 1ec5e020a1ff0ed5709fb7cabd0a0fb2d648f5acd2441f7b8b9e698dcc8d5e84
student@pod-jani01-node01:~$ sudo du -sh /var/lib/docker/vfs/dir/
1.6G    /var/lib/docker/vfs/dir/
```
