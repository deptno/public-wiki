# podman

[[docker]] 의 oci 버전

## command
```sh 
podman login [container registry]
podman image list
podman system connection list
```
## install
```su 
$ brew install podman
$ pod image list
Cannot connect to Podman. Please verify your connection to the Linux system using `podman system connection list`, or try `podman machine init` and `podman machine start` to manage a new Linux VM
Error: unable to connect to Podman socket: Get "http://d/v4.3.1/libpod/_ping": dial unix ///var/folders/yr/lb2jlrrd1fs7h6hn30n_ksvr0000gn/T/podman-run--1/podman/podman.sock: connect: no such file or directory
```
manage 를 위한 vm 을 아래 명령어로 띄우도록한다
```su 
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


## related
- [[docker]]
- [[kubernetes]]
