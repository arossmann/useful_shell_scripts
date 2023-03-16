# Docker Images

## find version
```
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
traefik             latest              96c63a7d3e50        2 months ago        85.7MB

$ IMAGE_ID=96c63a7d3e50

$ docker image inspect --format '{{json .}}' "$IMAGE_ID" | jq -r '. | {Id: .Id, Digest: .Digest, RepoDigests: .RepoDigests, Labels: .Config.Labels}'

```
Output:

```
{
  "Id": "sha256:96c63a7d3e502fcbbd8937a2523368c22d0edd1788b8389af095f64038318834",
  "Digest": null,
  "RepoDigests": [
    "traefik@sha256:5ec34caf19d114f8f0ed76f9bc3dad6ba8cf6d13a1575c4294b59b77709def39"
  ],
  "Labels": {
    "org.opencontainers.image.description": "A modern reverse-proxy",
    "org.opencontainers.image.documentation": "https://docs.traefik.io",
    "org.opencontainers.image.title": "Traefik",
    "org.opencontainers.image.url": "https://traefik.io",
    "org.opencontainers.image.vendor": "Containous",
    "org.opencontainers.image.version": "v1.7.20"
  }
}
```

## Delete all containers
```
    docker rm $(docker ps -a -q)
```
## Delete all images
``` 
    docker rmi $(docker images -q)
```

# Docker Networking
(based on: http://networkstatic.net/10-examples-of-how-to-get-docker-container-ip-address/)

## Example 1
```
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND               CREATED             STATUS                   PORTS                               NAMES
2e23d01384ac        iperf-v1:latest     "/usr/bin/iperf -s"   10 minutes ago      Up 10 minutes            5001/tcp, 0.0.0.0:32768->5201/tcp   compassionate_goodall
# Append the container ID (CID) to the end of an inspect
$ docker inspect --format '{{ .NetworkSettings.IPAddress }}' 2e23d01384ac
172.17.0.1
```

## Example 2
```
# Add -q to automatically parse and return the last CID created.
$ docker inspect --format '{{ .NetworkSettings.IPAddress }}' $(docker ps -q)
172.17.0.1
```

## Example 3
```
# As of Docker v1.3 you can attach to a bash shell
docker exec -it  2e23d01384ac  bash
# That drops you into a bash shell then use the 'ip' command to grab the addr
root@2e23d01384ac:/# ip add | grep global
    inet 172.17.0.1/16 scope global eth0
```

## Example 4
```
# Same as above but in a single line
$ docker exec -it  $(docker ps -q) bash
```

## Example 5
```
# Pop this into your ~/.bashrc (Linux) or ~/.bash_profile (Mac)
dockip() {
  docker inspect --format '{{ .NetworkSettings.IPAddress }}' "$@"
}
# Source it to re-read your bashrc/profile
source ~/.bash_profile
# Now run the function with the container ID you want to get the addr of:
$ dockip 2e23d01384ac
172.17.0.1
```

## Example 6
```
# Same as above but no argument needed and always return the latest container IP created.
dockip() {
  docker inspect --format '{{ .NetworkSettings.IPAddress }}' $(docker ps -q)
}
```

## Example 7
```
# Add to bashrc/bash_profile to docker exec in passing the CID to dock-exec. E.g dock-exec $(docker ps -q) OR dock-exec 2e23d01384ac
dock-exec() { docker exec -i -t $@ bash ;}
```

## Example 8
```
# Another little bash function you can pop into your bash profile
# Always docker exec into the latest container
dock-exec() { docker exec -i -t $(docker ps -l -q)  bash ;}
# The run ip addr
ip a
```

## Example 9
```
# Finally you can export the environmental variables from the running container
docker exec -i -t $(docker ps -l -q) env | grep ADDR
# Output --> CLOUDNETPERF_CARBON_1_PORT_2003_TCP_ADDR=172.17.0.229
```

## Example 10
```
# Or even run the ip address command as a parameter which fires off the ip address command and exits the exec
docker exec -i -t $(docker ps -l -q) ip a
# 1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default
#    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
#    inet 127.0.0.1/8 scope host lo
#       valid_lft forever preferred_lft forever
#    inet6 ::1/128 scope host
#       valid_lft forever preferred_lft forever
# 470: eth0: <BROADCAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
#    link/ether 02:42:ac:11:00:e9 brd ff:ff:ff:ff:ff:ff
#    inet 172.17.0.233/16 scope global eth0
#       valid_lft forever preferred_lft forever
#    inet6 fe80::42:acff:fe11:e9/64 scope link
#       valid_lft forever preferred_lft forever
 
```
