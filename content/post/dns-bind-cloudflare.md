+++
date = '2026-10-23T18:28:33+08:00'
draft = true
title = 'BIND and Cloudflare DNS'
+++

# What is DNS

Reference: [What is DNS](https://www.cloudflare.com/learning/dns/what-is-dns/)

DNS, Domain Name System, answers one question:

> "Given a domain name, what is the IP address corresponding to it?"

Why do we need such a service? Well, since computers interact through IP addresses, and the IP addresses are usually a sequence of numbers like `192.168.0.1` (IPv4), it could be hard to memorize for human.

On arch linux, one can use

## BIND

[BIND](https://www.isc.org/bind/) is a DNS.

To install BIND on FreeBSD, do

```bash
# First check the current version, it is bind920 when writing this post.
sudo pkg install bind920
```

### Set up BIND and make it run

## Cloudflare DNS
