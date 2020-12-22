---
id: 20201222163206
uuid: 5f4553da-c5d3-4603-bcdc-fa7981fb00be
title: Get IP Addresses Used by Amazon Route 53 Health Checks
date: 2020-12-22 16:32:06
modified: 
types: tech-note
categories: aws
pinned: false
tags: [aws, route53, ip, health, check, ipv4]
private: false
draft: false
---

To enable health checks on applications that are restricted to a certain list of IP addresses, we must also allow the list of IP addresses AWS provides, which are the IP addresses used by their Route 53 health checks.

More here: [IP address ranges of Amazon Route 53 servers - Amazon Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/route-53-ip-addresses.html)

The latest ranges can be downloaded [here](https://ip-ranges.amazonaws.com/ip-ranges.json)

You must then run the following to filter for only the IPv4 ranges we are interested in. 

```sh
jq --raw-output '.prefixes[] | select(.service == "ROUTE53_HEALTHCHECKS") | .ip_prefix' ip-ranges.json | sort --unique
```

You'll then be returned with a list, as below.

```sh
107.23.255.0/26
15.177.0.0/18
176.34.159.192/26
177.71.207.128/26
52.80.197.0/25
52.80.197.128/25
52.80.198.0/25
52.83.34.128/25
52.83.35.0/25
52.83.35.128/25
54.183.255.128/26
54.228.16.0/26
54.232.40.64/26
54.241.32.64/26
54.243.31.192/26
54.244.52.192/26
54.245.168.0/26
54.248.220.0/26
54.250.253.192/26
54.251.31.128/26
54.252.254.192/26
54.252.79.128/26
54.255.254.192/26
```
