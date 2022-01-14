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

https://developer.mozilla.org/zh-TW/docs/Learn/JavaScript/First_steps/What_is_JavaScript

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
  - The protocol, like http, https, ftp and so on.
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
