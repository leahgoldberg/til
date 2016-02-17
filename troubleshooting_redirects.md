# Troubleshooting Redirects with Dig and Curl

## 2/10/16

Dig, or domain information grouper, is very useful for troubleshooting DNS lookups and redirects. For example, say you needed to temporarily point a domain to a local server (which you could do by making an entry in `/etc/hosts`), you could use `dig` to doublecheck the address. 

For example, the output of `dig www.google.com` would be:

    ; <<>> DiG 9.8.3-P1 <<>> www.google.com
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 38880
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0
    
    ;; QUESTION SECTION:
    ;www.google.com.			IN	A
    
    ;; ANSWER SECTION:
    www.google.com.		222	IN	A	216.58.219.196
    
    ;; Query time: 126 msec
    ;; SERVER: 10.0.1.1#53(10.0.1.1)
    ;; WHEN: Wed Feb 17 17:05:33 2016
    ;; MSG SIZE  rcvd: 48
  
telling you, among many other things, that Google's address is 216.58.219.196. 

See [this link](https://mediatemple.net/community/products/dv/204644130/understanding-the-dig-command) for more details.

If you've set up a redirect and want to make sure it's working correctly, use `curl -I -L <domain>` to follow all redirects. For example, the output of `curl -I -L google.com` is:

    HTTP/1.1 301 Moved Permanently
    Location: http://www.google.com/
    Content-Type: text/html; charset=UTF-8
    Date: Wed, 17 Feb 2016 22:17:32 GMT
    Expires: Fri, 18 Mar 2016 22:17:32 GMT
    Cache-Control: public, max-age=2592000
    Server: gws
    Content-Length: 219
    X-XSS-Protection: 1; mode=block
    X-Frame-Options: SAMEORIGIN
    
    HTTP/1.1 200 OK
    Date: Wed, 17 Feb 2016 22:17:32 GMT
    Expires: -1
    Cache-Control: private, max-age=0
    Content-Type: text/html; charset=ISO-8859-1
    P3P: CP="This is not a P3P policy! See https://www.google.com/support/accounts/answer/151657?hl=en for more info."
    Server: gws
    X-XSS-Protection: 1; mode=block
    X-Frame-Options: SAMEORIGIN
    Set-Cookie: NID=76=kwY0_OLrxKAA2z7bLl13EVkoZjpMxmPQfyUww8Ymh6lS6WgwLji0effB-LpD1WSGMgCOSbP47KGNWLRJ6ZwZoM--5BMOmceF9llN7dAAqj_X0M7j0sgsDV30oD3L9zd7vdtiE90pQnERhg; expires=Thu, 18-Aug-2016 22:17:32 GMT; path=/; domain=.google.com; HttpOnly
    Transfer-Encoding: chunked
    Accept-Ranges: none
    Vary: Accept-Encoding
  
telling you that first, we're redirectly from `google.com` to `http://www.google.com`, then receiving a 200 OK.   
