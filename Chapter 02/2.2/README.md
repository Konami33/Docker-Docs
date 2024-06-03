# Lab Scenario: Setting Up a Host-Like Environment Using Docker Containers

## Objective

The objective of this lab scenario is to demonstrate how to set up a Docker container environment that mimics a traditional host system. This involves running multiple processes and services within a single container, which is contrary to the typical "one service per container" philosophy.


## Steps

### Step 1: Pull the Phusion Base Image

Phusion Baseimage is a special Docker image designed to run multiple processes. It is a minimal Ubuntu-based image modified for Docker-friendliness.

```bash
docker pull phusion/baseimage
```

### Step 2: Run the Phusion Base Image

Start a new container using the Phusion Baseimage in the background. The container ID will be returned upon successful execution.

```bash
docker run -d phusion/baseimage
```

Example output:

```
3c3f8e3fb05d795edf9d791969b21f7f73e99eb1926a6e3d5ed9e1e52d0b446e
```

### Step 3: Access the Running Container

Use the container ID from the previous step to access the running container. The `docker exec` command is used to start a new process inside an already running container. The `-i` flag allows for interactive mode, and `-t` sets up a TTY for the terminal.

```bash
docker exec -i -t 3c3f8e3fb05d795 /bin/bash
```

### Step 4: Verify Running Processes

Once inside the container, you can check the running processes using the `ps -ef` command.

```bash
ps -ef
```

Example output:

```
UID     PID    PPID  C STIME TTY          TIME CMD
root       1       0  0 13:33 ?        00:00:00 /usr/bin/python3 -u /sbin/my_init
root       7       0  0 13:33 ?        00:00:00 /bin/bash
root     111       1  0 13:33 ?        00:00:00 /usr/bin/runsvdir -P /etc/service
root     112     111  0 13:33 ?        00:00:00 runsv cron
root     113     111  0 13:33 ?        00:00:00 runsv sshd
root     114     111  0 13:33 ?        00:00:00 runsv syslog-ng
root     115     112  0 13:33 ?        00:00:00 /usr/sbin/cron -f
root     116     114  0 13:33 ?        00:00:00 syslog-ng -F -p /var/run/syslog-ng.pid --no-caps
root     117     113  0 13:33 ?        00:00:00 /usr/sbin/sshd -D
root     125       7  0 13:38 ?        00:00:00 ps -ef
```

You will observe that the container starts up similar to a host, initializing services such as `cron` and `sshd`.

## Discussion

Using a host-like container is useful for scenarios where multiple services need to run within a single container. This approach can be beneficial for:

- Initial demonstrations for engineers new to Docker.
- Specific use cases requiring multiple processes in one container.

However, this approach is somewhat controversial because it deviates from the typical Docker practice of isolating workloads to "one service per container." Modern container orchestration tools like Kubernetes and Docker Compose facilitate running multiple containers that work together, making the host-like container approach less common.

## Conclusion

This lab scenario provided a step-by-step guide to setting up a Docker container environment that runs multiple processes, mimicking a traditional host system. While this method can be useful, it is important to consider modern container orchestration tools for more efficient and scalable solutions.