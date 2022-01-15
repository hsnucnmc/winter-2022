---
theme: seriph
background: https://images.unsplash.com/photo-1491002052546-bf38f186af56?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1208&q=80
fonts:
    sans: 'Roboto'
    serif: 'Roboto'
    mono: 'JetBrains Mono'

---

# Overview of Services

shenchris & siriuskoan

---

# Outline

- DNS
  - How It Works
  - Record Types
  - Todo
- Mail
  - MUA / MTA / MDA
  - How It Works
  - Todo
- LDAP
  - How It Works
  - Todo

---
layout: cover
background: https://images.unsplash.com/photo-1491002052546-bf38f186af56?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1208&q=80
---

# DNS

shenchris & siriuskoan

---

# DNS

- IP Address - Unique identifiers to every device connect to the Internet. Similiar to physical address of a house.
- Domain Name - A name that maps to a numeric IP address, used to access a website.
- DNS - Like photo book, it maps domain name to IP address.
  
  `www.hs.ntnu.edu.tw` -> `203.68.92.132`
  )
  just like

  `師大附中` -> `台北市大安區信義路三段143號`

- Master - Main server, the real one to do name resolution and get data from disk.
- Slave - It gets data from master server periodically.
- `nslookup` and `dig` are useful tools that can check DNS records.

<!--

Explanation

-->

---

# DNS - How It Works

DNS is hierarchical and decentralized (prevent single point failure).

![](/dns-hier.png)

---

# DNS - How It Works

- Root Nameserver - First stop in a recursive resolver query. It directs the recursive resolver to a TLD nameserver based on the extension of that domain (`.com`, `.org`, ...)
- TLD - Standing for Top Level Domain server. It maintains information for domain names that share a common domain extension.
- Authoritative Nameserver - Last stop in the query. It returns the IP address or alias for the requested domain name back to the DNS Recursor.
- Recursive Resolver (DNS Recursor) - Middleman between a client and a DNS nameserver.

<!--

Explanation

The path is Root Nameserver -> TLD -> Middle -> Authoritative Nameserver

-->

---

# DNS - How It Works

DNS has two kinds of ways to do recursive query. One is iterative query, and the other one is recursive query.

- Iterative Query - Resolver asks root nameserver for the target domain name, and the root nameserver tells it to find another domain name server (TLD). When the resolver gets the response, it asks the TLD for the target, and the TLD tells it next server to ask. The steps repeat until the resolver asks the authoritative nameserver and get the final result.
- Recursive Query - Resolver asks root nameserver for the target domain name, and the root nameserver asks the next one (TLD). The steps repeat until the the query reaches the authoritative nameserver and get the final result. After getting it, the query goes back and gives the result to the resolver.

---

# DNS - How It Works

![](/dns-iter-recur.png)

<!--

Red is iterative query and blue is recursive query.

-->

---

# DNS - Record Types

There are many types of DNS records, and each of them has its own usage.

- `A` record - It contains the IP address of the domain name. For example, `www.hs.ntnu.edu.tw` has `A` record `203.68.92.132`.
- `AAAA` record - It contains the IPv6 address of the domain name.
- `NS` record - It is the IP address of the zone's authoritative nameserver. For example, `hs.ntnu.edu.tw` has `NS` record `dns.hs.edu.tw`.

---

# DNS - Record Types

- `MX` record - It is the mail server. For example, `cs.nctu.edu.tw` has `MX` record `csmx[2,3].cs.nctu.edu.tw`, so email to `@cs.nctu.edu.tw` will be sent to `csmx[2,3].cs.nctu.edu.tw`.
- `CNAME` record - It is alias referring to the value of the specified record. For example, if `bar.example.com` has `CNAME` record `foo.example.com`, then requests to `bar.example.com` is actually sent to `foo.example.com`.
- `SOA` record - It is some information about a domain or a zone. It contains email of admin, serial number (date), TTL, etc.

<!--

Two MX record means load balance.

-->

---

# DNS - Todo

We have discussed some basic concept of DNS, and you can try to setup a DNS server by your own.

- Install `bind` (The Berkeley Internet Name Domain system)
- Setup master and slave.
- Learn more about
  - DNS sinkhole
  - RNDC
  - DNSSEC

---
layout: cover
background: https://images.unsplash.com/photo-1491002052546-bf38f186af56?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1208&q=80
---

# Mail

siriuskoan

---

# Mail - MUA / MTA / MDA

MUA, MTA and MDA are important things in mail system.

- MUA - Mail User Agent. It is 
