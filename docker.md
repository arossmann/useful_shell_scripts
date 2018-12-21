# Example 1
```
$ docker ps -a
```
CONTAINER ID        IMAGE               COMMAND               CREATED             STATUS                   PORTS                               NAMES
2e23d01384ac        iperf-v1:latest     "/usr/bin/iperf -s"   10 minutes ago      Up 10 minutes            5001/tcp, 0.0.0.0:32768->5201/tcp   compassionate_goodall
# Append the container ID (CID) to the end of an inspect
```
$ docker inspect --format '{{ .NetworkSettings.IPAddress }}' 2e23d01384ac
```
172.17.0.1
