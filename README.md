# DockerPwn.py
[![GitHub release](https://img.shields.io/github/v/release/AbsoZed/DockerPwn.py)](https://GitHub.com/AbsoZed/DockerPwn.py/releases/)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)


Automation for abusing an exposed Docker TCP Socket.

This will automatically create a container on the Docker host with the host's root filesystem mounted,
allowing arbitrary read and write of the host filesystem (which is bad).

Once created, the script will employ the method of your choosing for obtaining a root shell. All methods are
now working properly, and will return a reverse shell. Chroot is the least disruptive, but Useradd is the default.

## Installation:

It is recommended that you utilize the following for usage as opposed to static releases - Code in this repository may be updated frequently with minor improvements before releases are created.

``` git clone https://github.com/AbsoZed/DockerPwn.py && cd DockerPwn.py ```


## Methods:

- All shell I/O is logged to './DockerPwn.log' for all methods.

- UserPwn: Creates a 'DockerPwn' user, and adds them to /etc/sudoers with NOPASSWD. The handler automatically escalates to
           root using this privilege, and spawns a PTY.

- ShadowPwn: Changes root and any valid user passwords to 'DockerPwn' in /etc/shadow, authenticates with Paramiko, 
             and sends a reverse shell. The handler automatically escalates to root utilzing 'su', and spawns a PTY.

- ChrootPwn: Creates a shell.sh file which is a reverse shell, hosts on port 80. Downloads to /tmp, 
             Utilizes chroot in docker container to execute shell in the context of the host, providing 
             a container shell with interactivity to the host filesystem.

## Roadmap:
 
- SSL Support for :2376

## Usage:
```
DockerPwn.py [-h] [--target TARGET] [--port PORT] [--image IMAGE] [--method METHOD] [--c2 C2]

optional arguments:
  -h, --help       show this help message and exit
  --target TARGET  IP of Docker Host
  --port PORT      Docker API TCP Port
  --image IMAGE    Docker image to use. Default is Alpine Linux.
  --method METHOD  Method to use. Valid methods are shadowpwn, chrootpwn, userpwn. Default is userpwn.
  --c2 C2          Local IP and port in [IP]:[PORT] format to receive the shell.
