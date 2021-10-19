# HOWTO
# ex1.1 ....... goweb

1. compile
```
go build goweb.go
```
2. start web server
``` 
 ./goweb
```
3. http://localhost:8080
```
Hello, there
```
4. http://localhost:8080/header
````
Header field "Upgrade-Insecure-Requests", Value ["1"]
Header field "Accept", Value ["text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8"]
Header field "User-Agent", Value ["Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/14.0.3 Safari/605.1.15"]
Header field "Accept-Language", Value ["en-us"]
Header field "Accept-Encoding", Value ["gzip, deflate"]
Header field "Connection", Value ["keep-alive"]
````
5. http://localhost:8080/version   (return "not found" if no value)
```
Header field 'VERSION': "not found" 
```
6. http://localhost:8080/healthz
```
200
```
7. http://localhost:8080/log
```
Hello, [::1]:60094

```

8. check local log file
```
# cat access.log
2021/10/10 11:00:03 Handling request for /log/ from [::1]:60094, status: 200
```

9. TODO: show "404 page not found" when they try a wrong URL

# ex1.2 ....... goweb/Dockerfile
0. possible docker deamon permission issue & [solution](https://newbedev.com/got-permission-denied-while-trying-to-connect-to-the-docker-daemon-socket-at-unix-var-run-docker-sock-post-http-2fvar-2frun-2fdocker-sock-v1-24-auth-dial-unix-var-run-docker-sock-connect-permission-denied-code-example).
```
sudo chmod 666 /var/run/docker.sock
``` 
1. dockhub login
```
docker login
```
2. build docker image
```
docker build -t duydls/goweb .
```
3. push docker image
```
docker push duydls/goweb
```
4. start docker image (with port forwarding 8080->localhost:80)
```
docker run -p 80:8080 duydls/goweb
```
5. check docker image status
``` 
http://localhost
```
6. find container id
```
docker ps | grep goweb
325ff6acaccd   duydls/goweb           "./goweb"                14 minutes ago   Up 14 minutes   0.0.0.0:80->8080/tcp, :::80->8080/tcp   romantic_borg
```
7. get the main process id of the container
```
docker inspect -f '{{.State.Pid}}' 325ff6acaccd
2288575
```
8. check container net in namespaces by using nsenter
```
nsenter -t 2288575 -n ip a s
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
8: eth0@if9: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
```
