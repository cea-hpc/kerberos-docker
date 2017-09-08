# README - Kerberos/Docker

[![Build Status](https://travis-ci.org/criteo/kerberos-docker.svg?branch=master)](https://travis-ci.org/criteo/kerberos-docker)

Kerberos/Docker is a project to run easily a **MIT Kerberos V5** architecture in a cluster of **docker containers**. It is really useful for running integration tests of project using Kerberos or for learning and testing Kerberos solution and administration.

<p align="center">
  <img src="./doc/kerberos-docker-logo.png" width=200/>
</p>

See: [MIT Kerberos V5](https://web.mit.edu/kerberos/) and [Docker](https://www.docker.com/).
  
## Prerequisites

Use an **operating system compatible with docker**.  
Install **GNU Make** (if not already available).  
Install **GNU Bash** (if not already available).  
Install **docker-ce** and **docker-compose** (without sudo for running docker command).   

This project is developed under Ubuntu 16.04.3 LTS.  
Different versions are:

~~~
$ docker --version
Docker version 17.06.1-ce, build 874a737
~~~

~~~
$ docker-compose --version
docker-compose version 1.8.0, build unknown
~~~

~~~
$ make --version
GNU Make 4.1
~~~

~~~
$ bash --version
GNU bash, version 4.3.48(1)-release (x86_64-pc-linux-gnu)
~~~

MIT Kerberos will be installed only in docker containers:

~~~
$ klist -V
Kerberos 5 version 1.13.2
~~~

## Usage

After installation, there are 3 docker containers with python web server on each one to check if it turns:

- `krb5-machine`: see http://localhost:5001
- `krb5-kdc-server`: see http://localhost:5002
- `krb5-service`: see http://localhost:5003

The goal is to connect from `krb5-machine` to `krb5-service` with ssh and Kerberos authentication (using GSSAPIAuthentication).

Here cluster architecture:

<p align="center">
  <img src="./doc/kerberos-docker-architecture.png" width=700/>
</p>


## Installation

Execute:

~~~
make install
~~~

See `Makefile` with `make usage` for all commands.

## Possible improvements

* Use that on CentOS, Arch Linux ... for container or host machine (not only Ubuntu)
* Add LDAP (or not) for Kerberos architecture
* Add other connector and service (postgresql, mongodb, nfs, hadoop) only OpenSSH for the moment
* Add Java or C code Application to connect to kerberos


## Test and Continous Integration

This project uses [Travis CI](https://www.travis-ci.org/) and 
[Bash Automated Testing System (BATS)](https://github.com/sstephenson/bats).


## Network analyser

You can create a [wireshark](https://www.wireshark.org/) instance running in a docker container built from docker image named `network-analyser`. 

See more details in `./network-analyser/README.md`.

## Debug and see traces

You can connect with interactive session to a docker container:

~~~
docker exec -it <container_name_or_id> bash
~~~

To debug kerberos (client or server):

~~~
export KRB5_TRACE=/dev/stdout
~~~

To debug ssh client:

~~~~
ssh -vvv username@host
~~~~

To debug ssh server:

~~~~
/usr/sbin/sshd -f /etc/ssh/sshd_config -d -e
~~~~

## Troubleshooting

On `krb5-kdc-server` docker container, there are 2 Kerberos services `krb5-admin-service` and `krb5-kdc`:

~~~
service --status-all
~~~

See all opened ports on a machine:

~~~
netstat -tulpn
~~~


Check that each machine has a synchronized time (with `ntp` protocol and `date` to check).

See [Troubleshooting](https://web.mit.edu/kerberos/krb5-1.13/doc/admin/troubleshoot.html) and
[Kerberos reserved ports](https://web.mit.edu/kerberos/krb5-1.5/krb5-1.5.4/doc/krb5-admin/Configuring-Your-Firewall-to-Work-With-Kerberos-V5.html).

## References

* ROBINSON Trevor (eztenia). **Kerberos**. Canonical Ltd. Ubuntu Article, November 2014. Link: https://help.ubuntu.com/community/Kerberos.
* MIGEON Jean. **Protocol, Installation and Single Sign On, The MIT Kerberos Admnistrator's how-to Guide**. MIT Kerberos Consortium, July 2008. p 62.
* BARRETT Daniel, SILVERMAN Richard, BYRNES Robert. **SSH, The Secure Shell: The Definitive Guide, 2nd Edition**. O'Reilly Media, June 2009. p. 672. Notes: Chapter 11. ISBN-10: 0596008953, ISBN-13: 978-0596008956
* GARMAN, Jason. **Kerberos: The Definitive Guide, 2nd Edition**. O'Reilly Media, March 2010. p. 272.  ISBN-10: 0596004036, ISBN-13: 978-0596004033.
* O’MALLEY Owen, ZHANG Kan, RADIA Sanjay, MARTI Ram, and HARRELL Christopher. **Hadoop Security Design**. Yahoo! Research Paper, October 2009. p 19.
*  MATTHIAS Karl, KANE Sean. **Docker: Up & Running**. O'Reilly Media, June 2015. p. 232. ISBN-10: 1491917571, ISBN-13: 978-1491917572.
