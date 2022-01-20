---
theme: seriph
background: https://images.unsplash.com/photo-1491002052546-bf38f186af56?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1208&q=80
fonts:
    sans: 'Roboto'
    serif: 'Roboto'
    mono: 'JetBrains Mono'

---

# Docker

siriuskoan

---

# Outline
- Introduction
- Container v.s VM
- How It Works
- `Dockerfile`
- `docker-compose`

---

# Introduction

Docker is an open platform for developing, shipping, and running applications.

Docker enables developers to separate the applications from the infrastructure and setup a clean environment for it.

![](/docker-icon.png)

<!--

From https://zh.wikipedia.org/wiki/File:Docker_(container_engine)_logo.svg

-->

---

# Container v.s VM

Docker use a technology called "container" to separate the applications from the infrastructure.

Besides container, another technology called virtual machine (VM) can also accomplish this.

The differences are that VM fully create an independent machine with independent OS, library, etc., and container creates a "container" based on OS of infrastructure server.

Therefore
- Container is faster and more lightweight than VM.
- VM is safer than container.
- Container is more reusable than VM.

<!--

Container is more reusable since it uses image, we will talk about later.

-->

---

# Container v.s VM

![](/containers-vs-virtual-machines.jpg)

<!--

From https://www.weave.works/blog/a-practical-guide-to-choosing-between-docker-containers-and-vms

-->

---


