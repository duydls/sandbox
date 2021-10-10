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

