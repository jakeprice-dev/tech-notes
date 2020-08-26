---
id: 20200826152549
uuid: c503f453-8d02-492c-afac-d5f7506a0fe2
title: Manage EC2 KeyPairs with the AWS CLI
date: 2020-08-26 15:25:49
modified: 
types: tech-note
categories: aws
pinned: false
tags: [aws, cli, ssh, keys, ec2]
private: false
draft: false
---

## Create a Key Pair

The below will create a keypair and save it to your SSH folder.

```sh
aws ec2 create-key-pair --profile "<profile>" --region "<region>" --query "KeyMaterial" --output "text" --key-name "<key_name>" > ~/.ssh/<key_name>
```

## Delete a Key Pair

```sh
aws ec2 create-key-pair --profile "<profile>" --region "<region>" --key-name "<key_name>"
```
