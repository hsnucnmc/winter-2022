---
theme: seriph
background: https://images.unsplash.com/photo-1491002052546-bf38f186af56?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1208&q=80
fonts:
    sans: 'Roboto'
    serif: 'Roboto'
    mono: 'JetBrains Mono'

---

# PKI

siriuskoan

---

# Outline
- What Is PKI
- OpenSSL

---

# What Is PKI

PKI, standing for Public Key Infrastructure, consists of hardware, software, people, policies and so on.

It is used to create, manage, distribute, use, store, revoke digital certificates.

---
layout: two-cols
---

# What Is PKI

::left::

- Certificate - Signed by CA, it contains data of owner.
- CSR - Certificate Signing Request, sent by user who want CA to sign a certificate for him or her.
- CA - Certificate Authority, it is itself a certificate.
- RCA - Root Certificate Authority, it only signs CA (including itself), not users. All the other CAs which are not RCA are called Intermediate CA. In chain of trust, it is also called trust anchor.
- X.509 - An ITU standard defining the format of public key certificate.
- Chain of Trust - A hierarchy of certificates which is used to verify the validity of a certificate’s issuer, and it consists of a trust anchor, at least one intermediate CA and end-entity certificate.

::right::

![](/pki-hierarchy.png)

From https://www.sysadmins.lv/blog-en/certificate-chaining-engine-how-this-works.aspx

<!--

Explanation

-->

---

# What Is PKI

How to get a certificate from CA.

![](/ca-diagram.png)

From https://www.ssl.com/faqs/what-is-a-certificate-authority/

<!--

1. User use his own information and public key to generate CSR, and send it to CA

2. CA validates the information

3. Generate signed certificate with CA private key and send it back

-->

---

# OpenSSL

There are many ways to get a certificate.
- Buy one
  - Expensive
- Get one from [Let's Encrypt](https://letsencrypt.org/zh-tw/) with [certbot](https://certbot.eff.org/).
  - Free
  - Should be renewed every two months
- Sign one by yourself
  - Free
  - Expiration date is up to you
  - No one trust it

<!--

We will talk about the last method

-->

---

# OpenSSL

You can use `openssl` command to generate key and certificate.

`openssl req -x509 -newkey rsa:4096 -sha256 -nodes -keyout key.pem -out cert.pem -days 30`
- `req` - PKCS#10 X.509 Certificate Signing Request (CSR) Management.
- `-x509` - Output a x509 structure instead of a cert request.
- `-newkey` - Specify as type:bits (create a 4096 bits RSA key).
- `-sha256` - Use SHA256 to make output.
- `-nodes` - Do not encrypt key.
- `-keyout` - The output key file.
- `-out` - The output certificate file.
- `-days` - Number of days cert is valid for.

Then you will be asked some questions about you.

After doing that, you will get a certificate file `cert.pem` and a key file `key.pem`, and you can use them in your service such as web server and mail server.

---

# OpenSSL

