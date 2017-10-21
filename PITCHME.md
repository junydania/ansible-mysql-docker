#HSLIDE

## Docker container for <span style="color:#e49436">MySQL</span> Master/Slave
#### Without the Frustration 
(Thanks to <span style="color:#e49436">Ansible</span>)

#VSLIDE?image=assets/images/cali.jpg

### <span style="color:#ede8e7; background-color:#0e2a35BF">Daniel Guzman Burgos</span> 
##### <span style="color:#ede8e7; background-color:#0e2a35BF">electronic engineer</span> 
##### <span style="color:#ede8e7; background-color:#0e2a35BF">Tech Lead MySQL at Percona in Cali, Colombia</span> 
##### <span style="color:#ede8e7; background-color:#0e2a35BF">Managed services POD Team</span> 

#VSLIDE

## What is Percona?

- The only company that delivers enterprise-class support, consulting, managed services and software <!-- .element: class="fragment" data-fragment-index="1" -->
- MySQL® <!-- .element: class="fragment" data-fragment-index="2" -->
- MariaDB® <!-- .element: class="fragment" data-fragment-index="2" -->
- MongoDB® <!-- .element: class="fragment" data-fragment-index="2" -->
- And other open source databases across on-premise and cloud-based platforms. <!-- .element: class="fragment" -->
- 3000 clients worldwide / Employs a global network of experts with a staff of over 140 people <!-- .element: class="fragment" -->

#HSLIDE

# Percona Server and MySQL

#VSLIDE

## What is Percona Server?

Percona Server for MySQL® is a free, fully compatible, enhanced, open source drop-in replacement for MySQL 

<p style="max-width: 70%; margin: 0 auto;">
![Percona Server](assets/images/percona-server-web.png)
</p>

#VSLIDE

## What is MySQL?

- Is an open-source relational database management system (RDBMS) <!-- .element: class="fragment" data-fragment-index="1" -->
- With a lot of features<!-- .element: class="fragment" data-fragment-index="2" -->
- Being the replication the feature that we need to put focus today. <!-- .element: class="fragment" -->

<p style="width: 30%; height:30%; margin: 0 auto;">
![MySQL](assets/images/mysql.png)
</p>

#VSLIDE

```bash
mysql> show slave status\G
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: mysql1
```

<p style="max-width: 70%; margin: 0 auto;">
![Replication](assets/images/replication.png)
</p>
<!--
MySQL’s built-in replication is the foundation for building large, high-performance applications on top of MySQL, using the so-called “scale-out” architecture. Replication lets you configure one or more servers as replicas1 of another server, keeping their data synchronized with the master copy. This is not just useful for high-performance applications—it is also the cornerstone of many strategies for high availability, scala- bility, disaster recovery, backups, analysis, data warehousing, and many other tasks. In fact, scalability and high availability are related topics.
-->

#HSLIDE

# Docker

#VSLIDE

## Docker because is light
### But is not the best option. 
For infrastructure :)
(For databases)

#VSLIDE

## What is Docker?
### Spooky question...

- Not chroot (but isolates)
- Not LXC (and thus not Jails)
- For what is worth: is an isolated process. 
- With an ip. 
- And resources that can be limited independent (https://www.kernel.org/doc/Documentation/cgroup-v1/cgroups.txt)
- ...at a very high level.

#VSLIDE

## What is Docker for Mac?
### It's a hack

- It's a VM running the Linux Alpine distro (https://alpinelinux.org)
- So technically, the containers still runs on a Linux OS
- Drawbacks like: The host cannot see the containers via network (though there is https://github.com/moby/vpnkit)

#VSLIDE

## The Trinity (for test env)

<p style="width: 15%; height:15%; margin: 0 auto;">
![Docker](assets/images/docker.png)
![Vagrant](assets/images/vagrant.png)
![AWS EC2](assets/images/ec2.jpg)
</p>

#VSLIDE

- When Docker?
- When a VM with Vagrant (+ VirtualBox)?
- When EC2?
- ...and when on the customer's infrastructure???

#VSLIDE

## Docker images

- A bunch of layers. 
- As described in a "Dockerfile"
- We need a Docker image with MySQL

#VSLIDE

## Let's build an image
...or let's use an existent one! :)

#VSLIDE

## Percona Server Docker images
### Enter Docker Hub

- Images made by Docker (the company)
- Images made by users. Percona have official images
- No need to build anything
- Ready to pull

#VSLIDE?image=assets/images/percona-docker-hub.png

https://hub.docker.com/r/percona/percona-server/

#VSLIDE

## How does a Dockerfile looks like?

```
FROM debian:jessie
MAINTAINER Percona Development <info@percona.com>

RUN apt-get update && apt-get install -y --no-install-recommends \
                apt-transport-https ca-certificates \
                pwgen \
        && rm -rf /var/lib/apt/lists/*

RUN apt-key adv --keyserver ha.pool.sks-keyservers.net --recv-keys 8507EFA5
RUN echo 'deb https://repo.percona.com/apt jessie testing' > /etc/apt/sources.list.d/percona.list

# the numeric UID is needed for OpenShift
RUN useradd -u 1001 -r -g 0 -s /sbin/nologin \
            -c "Default Application User" mysql

ENV PERCONA_MAJOR 5.7
ENV PERCONA_VERSION 5.7.18-16-4.jessie
```
https://github.com/percona/percona-docker/blob/master/percona-server/Dockerfile

#VSLIDE

## Docker container (for real)
```
docker run --name container-name -e MYSQL_ROOT_PASSWORD=secret -d percona/percona-server:tag
```

#VSLIDE

## Docker container (for real)
### Ingredients

- An image (Done!)
- A network (not done)
- A shared volume (in this case, just a shared file)
- An exposed port (MySQL default port: 3306)

#VSLIDE

## Docker networking

- Make sure all the containers share the same subnet
- Take advantage of the built-in DNS
- Cleaner

```
docker network create <name>
```

#VSLIDE

## Volumes

- Shared directories-like
- We need a custom MySQL Configuration file (To set the binary log options for the replica)

#VSLIDE

## Exposed port

- Port forwarding 
- So we can connect to MySQL

#HSLIDE

# Ansible

#VSLIDE

## Ansible because i'm lazy

#VSLIDE

## What is Ansible?

- An IT automation engine
- Written in Python
- Idempotent
- No agents involved
- Simple language: YAML

```
Ansible works by connecting to your nodes 
and pushing out small programs, 
called "Ansible modules" to them. 
These programs are written to be resource models
of the desired state of the system. 
Ansible then executes these modules (over SSH by default), 
and removes them when finished.
```

#VSLIDE

## Ansible ingredients

- An inventory (list of hosts)
- A playbook (a bunch of task to execute)
- A task (a module with parameters)
- Modules (small python scripts that does the job)

#VSLIDE

## Ansible requirements

- SSH 
- Python 
- Git (this is optional)

#VSLIDE

## What are we using for our playbooks?
### Modules

- docker_container
- docker_network
- docker_image
- templates
- command
- ini_file

#VSLIDE

### A real example works better, so...

#HSLIDE

# Hands on!

#VSLIDE

## Get the code!
https://github.com/nethalo/ansible-mysql-docker

#HSLIDE

# Appendix

#VSLIDE
## Thanks!
## WE ARE HIRING!!

https://www.percona.com/careers
