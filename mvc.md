---
theme: seriph
background: https://images.unsplash.com/photo-1491002052546-bf38f186af56?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1208&q=80
fonts:
    sans: 'Roboto'
    serif: 'Roboto'
    mono: 'JetBrains Mono'

---

# MVC

siriuskoan

---

# Outline

- Introduction
- Model
- View
- Controller

---

# Introduction

When developing, we should divide the program into several modules.

For example, a system monitoring program can be divided into
- Monitoring module, which is responsible for collecting the usage of CPU, memory, etc.
- GUI module, which is responsible for showing beautiful GUI to system admin.

---

# Introduction

Model-View-Controller, aka MVC, is a software design pattern.

It divides the program logic into three part - model, view and controller.

![](/mvc.png)

---

# Model

In model, it contains commercial logic, algorithms, etc.

It is also called "logical layer".

For example, it can update user information, write files, check password, etc.

---

# View

In view, it contains what client can see, i.e., UI.

It is also called presentation layer.

For example, HTML template.

---

# Controller

Controller is the bridge between model and view.

It receives requests from client, and forwards it to model. Then it gets response from model, and updates view.

There are many "buttons" on controller, and it can perform actions for clients.
