# Devo In-House Relay Docker image
Devo In-House relay Docker image built on top of CentOS.

## Overview
The Devo In-House relay software is a Syslog collector written in Java that detects and receives inbound events, applies processing rules to the events, then forwards them over a secure channel using SSL/TLS encryption to the Devo Cloud. This Docker image pulls the latest official build of  [**CentOS 8**](https://hub.docker.com/_/centos) and installs the Devo relay along with rsyslog, Python, and some additional tools.

## Installation

### Fetch the image
```
$ docker pull rcmelendez/devo-relay
```

### Clone the GitHub repo
```
$ git clone https://github.com/rcmelendez/devo-relay-docker
```

### Run/Stop the container with Docker Compose
Use this command to run the container:
```
$ docker-compose up -d
```

To stop the container execute this command:
```
$ docker-compose down 
```

## Tips

### Update Docker and Docker Compose
It's always a good practice to use the latest version to get updated features and security patches. This image was built and tested using the latest Docker and Docker Compose versions available:
```
$ docker --version
Docker version 19.03.12, build 48a66213fe
$ docker-compose --version
docker-compose version 1.26.1, build f216ddbf
```

### Setting up the timezone for your Docker container
The timezone value by default in any Docker container is **UTC**. In case you want your Devo relay container's timezone to be in sync with the host machine's timezone, modify the [`docker-compose.yml`](https://github.com/rcmelendez/devo-relay-docker/blob/64902beee05205ef3418789cb05e1a4e66cbc812/docker-compose.yml#L32) file. Uncomment this line and replace the value with your own timezone:
```
# - TZ=America/New_York
```

If you also want the Devo relay logs to have a custom timezone, uncomment this line in the [`conf/install.sh`](https://github.com/rcmelendez/devo-relay-docker/blob/64902beee05205ef3418789cb05e1a4e66cbc812/conf/install.sh#L9) file:
```
#export TZ=America/New_York
```

### Using UDP ports
Per Docker Compose [documentation](https://docs.docker.com/compose/reference/port/), **TCP** is the default protocol. If you want to expose UDP ports as well, then uncomment this line in the [`docker-compose.yml`](https://github.com/rcmelendez/devo-relay-docker/blob/f1169299207eae3f733ceae60f71c4dd28fe8349/docker-compose.yml#L35) file:
```
# - "12999-13030:12999-13030/udp"
```

### Troubleshooting
If you have any technical issues, review the [Relay troubleshooting tips](https://docs.devo.com/confluence/ndt/sending-data-to-devo/the-devo-in-house-relay/relay-troubleshooting-tips).

## A note about systemd
Per CentOS docker repo, systemd is now included in both the centos:7 and centos:latest base containers, but it is not active by default. For simplicity, I didn't enable it, since the only required services for the Devo relay are **scoja** (a Java process handled by `/etc/init.d/devo-relay`) and **rsyslog** (which does not come with an init script). To handle rsyslog, I created the bash script `/etc/init.d/rsyslog`. If you need to manually start/stop these services run these commands:
```
$ /etc/init.d/devo-relay --help
Usage: /etc/init.d/devo-relay { start | stop | restart }
$ /etc/init.d/rsyslog --help
Usage: /etc/init.d/rsyslog { start | stop | restart | status }
```

## Official Devo Resources
- Documentation: [Install on Docker](https://docs.devo.com/confluence/ndt/sending-data-to-devo/the-devo-in-house-relay/installing-the-devo-relay/install-on-docker)
- DockerHub repo: [DevoInc/devo-relay](https://hub.docker.com/r/devoinc/devo-relay)
- GitHub repo: [DevoInc/devo-relay](https://github.com/DevoInc/devo-relay)

## Contact
@rcmelendez on [LinkedIn](https://www.linkedin.com/in/rcmelendez/), [Medium](https://medium.com/@rcmelendez), and [GitHub](https://github.com/rcmelendez). You can also send me your feedback at my [email](mailto:roberto.melendez@devo.com).

## License
Check the [`LICENSE`](https://github.com/rcmelendez/devo-relay-docker/blob/master/LICENSE) file for more details.
