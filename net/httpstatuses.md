



[toc]

### 301 和 302

301和301都是重定向的模式

301:  是永久重定向

302:  临时重定向

>  niginx 301/302配置

rewrite 301

```
//把来自a.com 的请求301跳到 www.b.com
if ($host == 'a.com') {
    rewrite ^/(.*)$ http://www.b.com/$1 permanent;
}
```

redirect 302

```
//把来自a.com 的请求302跳到 www.b.com
if ($host != 'a.com') {
    rewrite ^/(.*)$ http://www.b.com/$1 redirect;
}
```



Note: For historical reasons, a user agent MAY change the request method from POST to GET for the subsequent request. If this behavior is undesired, the [307 Temporary Redirect](https://httpstatuses.com/307) status code can be used instead.

### 307

随着网络安全越发重要，越来越多的网站都从http迁移到了https。但迁移到https之前分发出去的链接都是http，为了兼容以前的链接，就需要做一个redirect，将http的请求redirect到https中。

当请求是POST的时候，由于一些历史原因，一些浏览器和库收到301或302的时候，会将原本的POST请求转为GET请求发送。此时POST的body会丢失，导致请求出问题。
如果要redirect POST请求，则需要使用[307 Temporary Redirect](https://tools.ietf.org/html/rfc7231#section-6.4.7)来进行redirect。RFC 7231中对于307的一个描述”Note: This status code is similar to 302 (Found), except that it does not allow changing the request method from POST to GET. “

```
server {
    listen 80;
    server_name www.a.com;
    if ($request_method = POST) {
        return 307 https://$host$request_uri;
    }
    return 301 https://$host$request_uri;
}
```

### 444 

444 是nginx 的一个状态码

CONNECTION CLOSED WITHOUT RESPONSE

A non-standard status code used to instruct [nginx](https://nginx.org/) to close the connection without sending a response to the client, most commonly used to deny malicious or malformed requests.

