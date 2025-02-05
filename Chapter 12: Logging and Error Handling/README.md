**Chapter 12: Logging and Error Handling**
-
The docker logs command displays information logged by running containers. The docker service logs command displays information logged by all containers participating in a service. The information logged and the format of the log depend almost entirely on the container&#39;s final command.

By default, the docker logs command displays command output as it would in the terminal. However, in some cases, the information displayed may not be useful. For example, if you use a logging driver that sends logs elsewhere or if a containerized application doesn&#39;t send output to STDOUT and STDERR. In such cases, you need to take additional steps.

One commonly used solution is to create a symbolic link from the log file to STDOUT and STDERR. This can be done by changing the application configuration in the Docker image. For example, the official nginx image creates a symbolic link from the log file to the appropriate custom device.

However, it&#39;s important to remember that docker logs only show what is sent to STDOUT and STDERR. If there are other settings for logging, it may require a different way to view the logs

**1. Log Check**

- **Check history image**
```bash
student@pod-jani01-node01:~$ docker history registry.adinusa.id/btacademy/nginx:latest
```

- **Run nginx**
```bash
student@pod-jani01-node01:~$ docker run -dit -p 80:80 –name nginx1 registry.adinusa.id/btacademy/nginx
```

- **Check the filesystem on nginx**
```bash
student@pod-jani01-node01:~$ docker diff nginx1
```

- **Check log**
```bash
student@pod-jani01-node01:~$ docker logs –details nginx1
student@pod-jani01-node01:~$ docker logs –timestamps nginx1
student@pod-jani01-node01:~$ docker logs nginx1
```
