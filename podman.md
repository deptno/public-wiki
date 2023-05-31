# podman

- [[docker]] 의 oci 버전
- docker 와 달리 daemon 이 없다
- docker 명령어와 호환 -> `alias docker=podman`

## command
```sh 
podman login [container registry]
podman image list
podman system connection list
```
## install
```sh 
$ brew install podman
$ pod image list
Cannot connect to Podman. Please verify your connection to the Linux system using `podman system connection list`, or try `podman machine init` and `podman machine start` to manage a new Linux VM
Error: unable to connect to Podman socket: Get "http://d/v4.3.1/libpod/_ping": dial unix ///var/folders/yr/lb2jlrrd1fs7h6hn30n_ksvr0000gn/T/podman-run--1/podman/podman.sock: connect: no such file or directory
```
manage 를 위한 vm 을 아래 명령어로 띄우도록한다
```sh
$ podman machine init
$ podman machine start start
 ~/w/sr/g/d/k8s-5950x  master ?1  podman machine start start                                                   125 err  16.15.0 node  03:42:55
Starting machine "start"
Waiting for VM ...
Mounting volume... /Users/deptno:/Users/deptno

This machine is currently configured in rootless mode. If your containers
require root permissions (e.g. ports < 1024), or if you run into compatibility
issues with non-podman clients, you can switch using the following command:

        podman machine set --rootful start

API forwarding listening on: /Users/deptno/.local/share/containers/podman/machine/start/podman.sock

The system helper service is not installed; the default Docker API socket
address can't be used by podman. If you would like to install it run the
following commands:

        sudo /opt/homebrew/Cellar/podman/4.3.1/bin/podman-mac-helper install
        podman machine stop start; podman machine start start

You can still connect Docker API clients by setting DOCKER_HOST using the
following command in your terminal session:

        export DOCKER_HOST='unix:///Users/deptno/.local/share/containers/podman/machine/start/podman.sock'

Machine "start" started successfully
```
## error
### Error: failed to connect: dial tcp [::1]:61310: connect: connection refused
```sh 
$ podman run -ti fathyb/carbonyl https://youtube.com                                                         127 err  16.15.0 node  17:46:50
Error: failed to connect: dial tcp [::1]:61310: connect: connection refused
```
---
### docker build 보다 느림
graph driver 에 대한 검색 결과가 노출되고 있음, docker 를 쓰는게 지금 마음이 편함

## related
- [[docker]]
- [[kubernetes]]
- [[harbor]]
