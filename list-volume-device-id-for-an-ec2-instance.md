---
id: 20191224131800
uuid: 18df39e5-cc29-4f05-943c-f8bd233ed4c8
title: List Volume Device ID for an EC2 Instance
date: 2019-12-24 13:18:00
modified: 
types: tech-note
categories: aws
pinned: false
tags: [aws-cli, ec2, ebs]
private: false
draft: false
---

List the ID of a volume attached to an EC2 instance, using the AWS CLI.

``` sh
$ aws ec2 describe-volumes --filters "Name=attachment.instance-id,Values=<instance-id>" --query "Volumes[].Attachments[].{DeviceName:Device}" --region <region> --output text
```
