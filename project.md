---
theme: seriph
background: https://images.unsplash.com/photo-1491002052546-bf38f186af56?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1208&q=80
fonts:
    sans: 'Roboto'
    serif: 'Roboto'
    mono: 'JetBrains Mono'

---

# Project

siriuskoan

---

# Outline

- Basic
- Web Application

---

# Basic

- Create two web pages that must be accessed with HTTPS
  - They have different hostname
    - If you don’t setup DNS server, you can edit `/etc/hosts`
  - One with valid certificate
    - Let’s encrypt & certbot
  - Another one with self-signed certificate

---

# Basic

- Optional
  - Write a script to analyze `/var/log/secure` and output the following three information to a web page
    - `{user} used sudo to do “{command}” on {date}`
    - `{source IP address} failed to log in {login faild times} times`
    - `{user} failed to log in {login failed times} times`
  - DNS Server
  - Mail Server
    - Postfix & Dovecot
    - Setup Roundcube

---

# Web Application

- Choose a license for the project
- Do not push directly, send PR
- It has another hostname
- You can use any web framework as you want
  - Flask
  - Django
  - Laravel

---

# Web Application

- Deployment
  - Use Docker to deploy your web application
  - Use `docker-compose` to connect web application server and database server
  - Use Nginx or other web server to redirect requests to host to container
    - Do reverse proxy
  - Follow GitOps
    - Put your code on any git server such as GitHub
    - When the repo is updated, the web application will be automatically updated
      - You can write a script and use cronjob to achieve this
      - When the application is updated, send an email to the admin
        - You can use gmail, built-in sendmail or self-hosted mail server

---

# Web Application

- Optional
  - Make a website that is useful for CNMC
  - Access the web application in container with HTTPS
  - Setup database migration
  - Make the script used for updating a service
    - Use `logger` command to log some information
  - Setup CI/CD
    - Do simple tests
    - You can choose any CI/CD tools to do this
      - GitHub Actions
      - CircleCI
      - Travis CI
