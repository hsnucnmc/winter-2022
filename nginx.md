---
theme: seriph
background: https://images.unsplash.com/photo-1491002052546-bf38f186af56?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1208&q=80
fonts:
    sans: 'Roboto'
    serif: 'Roboto'
    mono: 'JetBrains Mono'

---

# Web Server: Nginx

siriuskoan

---
layout: two-cols
---

# Outline

::left::

- Background knowledge
  - HTML / CSS / JS
  - URL
  - Static vs. Dynamic Pages
  - Proxy
  - Virtual Host
  - SNI
- HTTP
  - Client-Server Architecture
  - Status Code
  - Methods
  - Request and Response

::right::

- Nginx
  - Workflow
  - Config
  - Reverse Proxy
  - Load Balancing

---

# Background Knowledge - HTML / CSS / JS

- HTML: Standing for Hypertext Markup Language. It defines the structure of websites.
- CSS: Standing for Cascading Stylesheets. It is used to style and beautify websites.
- JS: Standing for JavaScript. It is a popular and versatile programming language and is used to make animation of websites, interact with client, communicate with server, etc.

[Demo](https://developer.mozilla.org/zh-TW/docs/Learn/JavaScript/First_steps/What_is_JavaScript)

---

# Background Knowledge - URL

URL, standing for Uniform Resource Locator, is the address of a resource on the Web. A resource can be a website (HTML file), a CSS document, a JavaScript file, an image, etc.

A valid URL consists of many parts
- Scheme
- Authority
- Path to resource
- Parameters
- Anchor

![](/url.png)

https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_URL

<!--

Many people cannot tell domain name from URL.

Domain name is just a part of URL.

-->

---

# Background Knowledge - URL

- Scheme
  - The protocol, like `http`, `https`, `ftp`, `file` and so on.
  - Followed by `://`.
- Authority
  - Including domain name and port which are separated by a colon.
- Path to resource
  - The path to the resource on the server.
- Parameters
  - Extra parameters provided to the server.
  - Starting with `?`.
  - Key and value are separated by `=`, and key value pairs are separated by `&`.
- Anchor
  - An anchor to another part of the resource itself.
  - Starting with `#`.

---
layout: two-cols
---

# Background Knowledge - Static vs. Dynamic Pages

::left::

Static Pages
- Pages remain same.
- Simple and fast.
- Elements are informational.
- Database is required.
- No backend.

::right::

Dynamic Pages
- The content depends on time, visitors, etc.
- Complicated and slow.
- Elements are functional and interactive.
- Database is required.
- Backend is writtern in PHP, Python, etc.

::end::

![](/static-dynamic-pages.jpg)

<!--

https://www.pluralsight.com/blog/creative-professional/static-dynamic-websites-theres-difference

-->

---
layout: two-cols
---

# Background Knowledge - Proxy

Proxy acts as a middleman, and there are two types of proxy.

::left::

Forward Proxy: Server don't know who is actual client.

Advantages
- Anonymous
- Cache
- Filtering

::right::

Reverse Proxy: Client don't know who is actual server.

Advantages
- Load balancing, HA
- Cache
- Server can change its address without influencing the service

---

# Background Knowledge - Proxy

![](/proxy.jpg)

https://medium.com/@mohsin061/forward-proxy-and-reverse-proxy-500b9bd4bf8e

---
layout: two-cols
---

# Background Knowledge - Virtual Host

The concept of virtual hosts allows more than one Web site on one system or Web server.

::left::

IP-based Virtual Host
- Every virtual host use a dedicated IP address.
- There are not so many IP addresses.
- For example, a server has two IP addresses `140.131.149.1` (external) and `10.1.1.1` (internal). By using virtual host, we can see different page when accessing the server with server IP addresses `140.131.149.1` and `10.1.1.1`.

::right::

Name-based Virtual Host
- Every virtual host has a domain name.
- Client must support HTTP 1.1 since server needs `Host` header.
- For example, a server has domain name `example.com`, and all its subdomain points to it. By using virtual host, we can see different page when accessing the server with domain names `a.example.com` and `b.example.com`.

<!--

We'll talk about HTTP header later.

-->

---

# Background Knowledge - SNI

SNI stands for Server Name Indication, and it is used to tell server the host name client want to connect.

When client uses HTTPS, the HTTP header is encrypted and server cannot read `Host` header, i.e., name-based virtual host cannot work, so server needs SNI to know which virtual host client want to connect.

<!--

Server is apartment. IP address is the street address. SNI is the apartment number.

When a package is sent to the apartment by its address, the mailer needs apartment number to make right one receive it.

-->

---

# HTTP

HTTP, standing for HyperText Transfer Protocol, is a protocol for fetching resources such as HTML documents.

It is the foundation of any data exchange on the Web.

It is one of the protocols in application layer, and it is sent over TCP.

---

# HTTP

![](/http-layers.png)

<!--

https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview

-->

---

# HTTP - Client-Server Architecture

![](/http-client-server.png)

https://nasa.cs.nctu.edu.tw/sa/2021/slides/18_Web.pdf (P9)

<!--

The first step is client sends request to server.png

-->

---

# HTTP - Status Code

HTTP status code manifests the status of response.

It has five types, starting with `1` to `5`.
- `100~199` - Informational response (the request was received and understood).
- `200~299` - Successful response.
- `300~399` - Redirects.
- `400~499` - Client errors.
- `500~599` - Server errors.

<!--

Not all number of them are used.

-->

---

# HTTP - Status Code

Let's see some common status code.

- `200` - OK.
- `301` - Moved Permanently. The server will also gives client a new URL to redirect, and client (browser) should redirect to the URL **permanently**.
- `302` - Found. The server will also gives client a new URL to redirect, and client (browser) should redirect to the URL **this time** and continue to use current URL in the future.
- `304` - Not Modified. The server tells client cache is still usable.
- `500` - Internal Server Error.
- `502` - Bad Gateway. When the server is the middleman, the error occurs when the upstream server fails.
- `503` - Service Unavailable. The server is not ready to handle the request, and the possible reason can be under maintenance or overload. The error is often temporary.

---

# HTTP - Status Code

- `400` - Bad Request. The client sends request with invalid syntax and server cannot understand.
- `401` - Unauthorized. The client must authenticate itself first. (Not login yet)
- `403` - Forbidden. The client doesn't have access right to the content.
- `404` - Not Found.
- `405` - Method Not Allowed.
- ~~`418` - I'm a teapot: The server refuses the attempt to brew coffee with a teapot.~~ It is a April Fool's Day joke and is defined in [RFC 2324](https://datatracker.ietf.org/doc/html/rfc2324). 
- `429` - Too Many Requests. The client has been over the rate limit.

[HTTP Status Code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

<!--

We'll talk about HTTP methods later.

418 is an interesting status code, you can check out the article and its RFC.

[搶救茶壺大作戰：418 I am a teapot](https://blog.techbridge.cc/2019/06/15/iam-a-teapot-418/)

-->

---

# HTTP - Methods

HTTP request methods indicate the desired action to be performed for a given resource.

- `GET` - Requests a representation of the specified resource.
- `HEAD` - Asks for a response only with header. The method is useful when a downloadable file is large, by checking the header first, you can know the size by `Content-Length` header.
- `POST` - Sends data to server, and server add a record.
- `PUT` - Replace, i.e., creates new resources or update current resources. (send full data)
- `DELETE` - Deletes specified resources.
- `CONNECT` - Establishes a tunnel to the server identified by the target resource.
- `OPTIONS` - Show what methods can be used.
- `TRACE` - Performs a message loop-back test along the path to the target resource. (Debug purpose)
- `PATCH` - Applies partial modifications to a resource (send partial data).

---

# HTTP - Methods


- Safe: It does not alter the state of the server.
- Idempotent: An identical request can be made several times in a row and leaves the server in the same state.

![](/http-method-compare.png)

<!--

Why `PATCH` is not idempotent?

Since it is how to change a resource, but `PUT` is what to change to what

-->

---

# HTTP - Request and Response

Request header

```http
GET /home.html HTTP/1.1
Host: developer.mozilla.org
User-Agent: Mozilla/5.0(Windows NT 6.1; Win64; x64; rv:47.0) Gecko/20100101 Firefox/47.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Connection: keep-alive
Cache-Control: max-age=0
```

<!--

User-Agent is the identification of the application, OS you use.

Connection is whether the network connection stay open after the current tranasction finishes.

-->

---

# HTTP - Request and Response

Response header

```http
200 OK
Access-Control-Allow-Origin: *
Connection: Keep-Alive
Content-Encoding: gzip
Content-Type: text/html; charset=utf-8
Date: Mon, 18 Jul 2016 16:06:00 GMT
Etag: "c561c68d0ba92bbeb8b0f612a9199f722e3a621a"
Keep-Alive: timeout=5, max=997
Last-Modified: Mon, 18 Jul 2016 02:36:04 GMT
Server: Apache
Set-Cookie: mykey=myvalue; expires=Mon, 17-Jul-2017 16:06:00 GMT; Max-Age=31449600; Path=/; secure
```

<!--

Etag is related to cache, when the etag changes, the resource is updated.

-->

---
