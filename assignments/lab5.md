# Lab 5

## Goals

* allow SSH and HTTP traffic but nothing else
* test firewall is working
* Answer questions about firewalls/iptables

## Install NGINX

Install NGINX as it'll default as a running service with a hosted web page on port 80 (HTTP traffic) on your machine. You'll use this to test your firewall.

## Configure Firewall

You should now use iptables and/or UFW to enable SSH/HTTP but disallow all other ports/traffic.

### Testing

You should be able to trace the rules with the following:

```bash
$ iptables -L -v -n
```

Verify that traffic other than HTTP/SSH is not allowed.

## Answer Questions about IPTables/UFW


1. What is *iptables*?
2. What is *UFW*?
3. Why might you want to use UFW over iptables?
4. Why might you want to disallow all traffic other than specified?
5. Why are both important if you are administrating a machine?

## Submitting Assignment

Submit answers to the questions as well as commands used to Canvas.