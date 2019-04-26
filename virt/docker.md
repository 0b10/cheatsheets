# Net
[Docs](https://docs.docker.com/network/bridge/)

* IPv6 for containers is disabled by default
* Traffic from containers on the *default bridge* is not forwarded to the outside world. See the [docs](https://docs.docker.com/network/bridge/).

## Cheatsheet

```sh
docker network create foo
docker network ls
docker network connect <netname> <containername> # Change while running
docker network disconnect <netname> <containername>
docker network rm <netname>
docker run --name foo <imgname> # For DNS
```



## Bridged Networks

* Best choice for containers (on the same host) that need to communicate.
* Containers need to be on the same host (obvious fact).

### Default

(The default bridged network - simply called 'bridge')

* Not easily configurable - but it can be configured via the daemon.json file. See [docs](https://docs.docker.com/network/bridge/)
* This is the default network for all newly started containers, unless specified otherwise.
* Exposes all ports, for all containers - not good for isolation.
  * Access to ports must be restricted by the host.
  * Use a **user defined** bridged network for isolation.
* Containers can only connect to each other via an IP address - this is not portable.

### User Defined

```
docker create netwok foo
```

* Configurable
* Provides isolation from other networks - including the host, unless a port is exposed.
* Automatically exposes all ports to containers on the same bridged network.
* Automatic DNS resolution between containers using their names - each container can be given a name with `--name`.

## Host
* Insecure 
* Gives a container full access to the hosts networking services, such as DBUS.

# Configuration(?)
## ENV Variables

### Sharing
* Hosts on the default bridge joined with `--link` share environment variables.
* Use docker-compose to share configuration.
* Mount a file or directory containing the necessary information.