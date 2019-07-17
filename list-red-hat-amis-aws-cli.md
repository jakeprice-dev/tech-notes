---
id: 20190717161012
uuid: 3fee5dd8-4a3b-4d43-b3dc-500f0ecafd4d
title: List Red Hat AMIs using AWS CLI
date: 2019-07-17 16:10:12
modified: 
types: tech-note
categories: aws
pinned: false
tags: [redhat, aws, ami, cli]
private: false
draft: false
---

``` sh
aws ec2 describe-images --owners 309956199498 --query 'sort_by(Images, &CreationDate)[*].[CreationDate,Name,ImageId]' --filters "Name=name,Values=RHEL-8*" --region <region> --output table
```

Make sure you leave the `owners` as is (or update it for another distro). But you can change `filters` and `region` as required.
