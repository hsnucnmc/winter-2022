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

# HTTP

HTTP, standing for HyperText Transfer Protocol, is a protocol for fetching resources such as HTML documents.

It is the foundation of any data exchange on the Web.

It is one of the protocols in application layer, and it is sent over TCP.

---

# HTTP

![](/http-layers.png)

---

# HTTP - Client-Server Architecture

![](/http-client-server.png)

https://nasa.cs.nctu.edu.tw/sa/2021/slides/18_Web.pdf (P9)

<!--

The first step is client sends request to server.png

-->
