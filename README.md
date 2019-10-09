# docker-varnish-test
Testing varnish and its functions

## How to use it

1. Start the stack

```
docker-compose up
```

2. open your browser and verify you can access the nginx server directly:
http://localhost:8080

Check that for every 2 sec the browser is refreshing the page and should log things:
```
web_1      | 172.19.0.1 - - [09/Oct/2019:12:38:17 +0000] "GET / HTTP/1.1" 200 170 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36" "-"
web_1      | 172.19.0.1 - - [09/Oct/2019:12:38:17 +0000] "GET /images/img.jpg HTTP/1.1" 304 0 "http://localhost:8080/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36" "-"
web_1      | 172.19.0.1 - - [09/Oct/2019:12:38:20 +0000] "GET / HTTP/1.1" 304 0 "http://localhost:8080/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36" "-"
web_1      | 172.19.0.1 - - [09/Oct/2019:12:38:22 +0000] "GET / HTTP/1.1" 304 0 "http://localhost:8080/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36" "-"
web_1      | 172.19.0.1 - - [09/Oct/2019:12:38:24 +0000] "GET / HTTP/1.1" 304 0 "http://localhost:8080/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36" "-"
```

Now close that browser page and check this one:
http://localhost:80 (through varnish)

The only 2 things appearing now in the logs are:
```
web_1      | 172.19.0.3 - - [09/Oct/2019:12:39:38 +0000] "GET / HTTP/1.1" 200 170 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36" "172.19.0.1"
web_1      | 172.19.0.3 - - [09/Oct/2019:12:39:39 +0000] "GET /images/img.jpg HTTP/1.1" 200 1221501 "http://localhost/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36" "172.19.0.1"
```

the refresh now happens purely in varnish and doesn't hit the nginx server.