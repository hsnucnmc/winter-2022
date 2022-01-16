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
  - MUA / MTA / MDA / MRA
  - Protocols
  - How It Works
  - DKIM
  - SPF
  - SRS
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

# Mail - Protocols

In mail system, there are three important protocols.

- `SMTP` - It is used to send and receives mails through mail servers.
- `IMAP` - It is used to allow users get their mails from anywhere.
- `POP3` - It is used to allow users get their mails from anywhere.

The difference between `IMAP` and `POP3` is that the the former still stores the mails after users read them, while the latter removes them after being read.

They can all be encrypted and become `SMTPS`, `IMAPS` and `POP3S`.

---

# Mail - MUA / MTA / MDA / MRA

`MUA`, `MTA`, `MDA` and `MRA` play important roles in mail system.

- `MUA` - Mail User Agent. It is a software between users and mail server. Users can use it to write email and send it. For example, `Outlook`, `Thunderbird` and webmail are all MUA.
- `MTA` - Mail Transfer Agent. It is so-called "mail server". It receives and sends (relays) emails from or to other mail servers, and gives users their own emails. For example, `Postfix` and built-in `sendmail` are MTA.
- `MDA` - Mail Delivery Agent. It decides what to do to emails. It can receives them to INBOX, sends to another MTA, etc. For example, `Procmail`.
- `MRA` - Mail Retrieval Agent. It gets email from remote server and allows users to access their mailboxes. For example, `Dovecot`.

---

# Mail - How It Works

![](/MxA.png)

<!--

User writes mail on MUA -> MUA sends it to MTA -> MTA transfer to MDA -> MDA do filter and other things to it -> MDA deliver it to MRA

-->

---

# Mail - Open Relay

A mail server can sends mail to other mail servers when it finds that the mail does not belong to it.

However, if a mail server relays all mails to other mail servers, it may overwhelms other mail servers.

It is considered to be an open relay server and will be banned.

To prevent this, every mail server should configure correctly that which domain it should relay.

---

# Mail

Next we are talking about security.

Before getting started, we have to know that `SMTP` is not secure.

An email is just a text file, and after user logins the mail server, he or she can send mail with any address and sender.

Therefore, it is easy to send a fake mail to anyone.

However, people must not allow this thing happens, so there are many mechanisms to solve this problem.

[關於 email security 的大小事 — 原理篇](https://tech-blog.cymetrics.io/posts/crystal/email-sec-theory/)

---

# Mail - SPF

SPF stands for Sender Policy Framework. The mechanism makes all domain has one DNS record that records which IP addresses its mail servers have.

By doing so, if a bad guy uses his or her own mail server to send mail as someone in other domain, the mail will not pass SPF test and thus being classifies as not safe or spam.

However, if the mail is intercepted and the content is changed by a bad guy, SPF is useless.

---

# Mail - SRS

SPF is good, however, if the mail server want to forward the mail, SPF test will fail since the sender domain and forwarder domain is not the same.

SRS, standing for Sender Rewriting Scheme, is used in order to solve this problem.

It can rewrite the sender and make it pass SPF test. After passing SPF, the destination server can convert it back to original sender and show it to receivers.

---

# Mail - DKIM

To prevent the situation mentioned above, DKIM is purposed.

DKIM stands for DomainKeys Identified Mail. The mechanism encrypts the some of the headers (specified by mail server) and content and add its hash to header.

In this way, if the mail is intercepted and modified by others, DKIM can detect it and mark the mail unsafe.

However, the sender shown on MUA is `header.from`, and SPF and DKIM check `smtp.MailFrom`.

Therefore, if a bad guy setup a mail server and set `header.from` as fake one (for example, `gmail.com`), `smtp.MailFrom` as original one, both of SPF and DKIM cannot detect this.

---

# Mail - DMARC

DMARC stands for Domain-based Message Authentication, Reporting and Conformance.

It will check whether `header.from` and `smtp.MailFrom` are the same, and the process is called "alignment".

Besides, it will also check whether SPF and DKIM are passed. If any one of them are not passed, the action admin specifies will be performed.

<!--

If SPF or DKIM is not used, DMARC will not check that.

The action can be `reject`, `quarantine`, `none`.

-->
