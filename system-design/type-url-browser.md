## What happens when you type a URL into your browser?

## URL Components
- scheme -> http or https.  This tells the browser to send a connection to the server using HTTP.
- domain -> example.com. This is the domain name of the site.
- path -> api/url. It is the path on the server to the requested resource: phone.
- resource -> phone.  It is the name of the resource user  wants to visit.


1. User types an url into the browser
2. Browser looks up IP in cache -> DNS Cache
2.1. Browser looks up IP using recursive DNS lookup. DNS Resolver -> recursive lookup -> DNS Server
3. Browser estabilhes TCP connection with the server
4. Browser sends HTTP request to the server
5. Server sends back HTTP response
6. Browser renders HTTP content


## References:
[Youtube](https://www.youtube.com/watch?v=AlkDbnbv7dk)
[What happens when you type a URL into your browser?](https://blog.bytebytego.com/p/what-happens-when-you-type-a-url?s=r)

