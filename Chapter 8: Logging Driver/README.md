**Chapter 8: Logging Driver**
-
Driver logging is a feature in Docker that helps you view information from running containers. Every Docker has a default logging driver. Automatically, Docker uses the json-file logging driver, which stores container logs in JSON format. You can also use the driver logging plugin for more options.

By default, Docker does not perform log rotation, which can cause json-file log files to take up a lot of disk space if the container produces a lot of output.

Docker still uses json-files as default to maintain compatibility with older versions and when used with Kubernetes.


**1. Configuring Logging Driver**

**Execute in node01**

- **Create daemon.json file**
```bash
student@pod-jani01-node01:~$ sudo vim /etc/docker/daemon.json
```
![image](https://hackmd.io/_uploads/BkWK5Wl10.png)

- **Restart Daemon dan Docker**
```bash
student@pod-jani01-node01:~$ sudo systemctl daemon-reload
student@pod-jani01-node01:~$ sudo systemctl restart docker
```

- **Run container**
```bash
student@pod-jani01-node01:~$ docker run --log-driver json-file --log-opt max-size=10m registry.adinusa.id/btacademy/alpine echo hello world
```

- **Check the logs in the docker directory**
```bash
student@pod-jani01-node01:~$ sudo cat /var/lib/docker/containers/$(docker ps --no-trunc -a | grep &#39;alpine&#39; | awk &#39;{print $1}&#39;)/$(docker ps --no-trunc -a | grep &#39;alpine&#39; | awk &#39;{print $1}&#39;)-json.log | jq .
```
