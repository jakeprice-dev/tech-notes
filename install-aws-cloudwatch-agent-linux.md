---
id: 20210111145045
uuid: 8e50e9c6-3e97-4a2c-b171-bb47c2896c7f
title: Install AWS CloudWatch Agent on Linux
date: 2021-01-11 14:50:45
modified: 
types: tech-note
categories: aws
pinned: false
tags: [aws, cloudwatch, linux, monitoring, metrics]
private: false
draft: false
---

## Download & Install

```sh
wget https://s3.amazonaws.com/amazoncloudwatch-agent/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb --output-document=/tmp/amazon-cloudwatch-agent.deb
sudo dpkg --install --skip-same-version /tmp/amazon-cloudwatch-agent.deb
```

## Create IAM Role Profile

Create an EC2 IAM role with `CloudWatchAgentServerPolicy` permissions.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricData",
                "ec2:DescribeVolumes",
                "ec2:DescribeTags",
                "logs:PutLogEvents",
                "logs:DescribeLogStreams",
                "logs:DescribeLogGroups",
                "logs:CreateLogStream",
                "logs:CreateLogGroup"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetParameter"
            ],
            "Resource": "arn:aws:ssm:*:*:parameter/AmazonCloudWatch-*"
        }
    ]
}
```

Then modify the IAM role associated to the instance you are installing the agent on, and select the role you have just created.

The EC2 instance should immediately pick up the role, but you can check by querying the metadata service. You should be able to retrieve credentials, as below.

```sh
$ curl http://169.254.169.254/latest/meta-data/iam/security-credentials/
System1CloudWatchAgentRole

$ curl http://169.254.169.254/latest/meta-data/iam/security-credentials/System1CloudWatchAgentRole
{
  "Code" : "Success",
  "LastUpdated" : "2021-01-13T09:29:39Z",
  "Type" : "AWS-HMAC",
  "AccessKeyId" : "ASIAYABNLTXMBAX4YQQV",
  "SecretAccessKey" : "VoP6rvW2W7/us/WYm9WhSA2pF/N9cg3u+a0U5xKT",
  "Token" : "IQoJb3JpZ2luX2VjEAoaCWV1LXdlc3QtMiJGMEQCIH4bEcXryBIVMKWmIz7g8mPdm5fN7BtMjMwMuzIOpapyAiBWCVY0idap2fsiev0NVtR4PsWKi98PSEOgNC8wjDfT8yq9AwjT//////////8BEAIaDDU0OTg1MDk0NzAzMiIMQzhqeBnnb5mSzZQDKpEDp27ezlrzO21qFmjZdw82mP5elFS5n4MkfY+bGNrbJGqSel9bcGSh75uec3MkuoScrbvc7dwRzEldXwxhKM/KvwbdHcBPmbwxiisRZZvZ/43tRRFwyEJIMvW5FXOlvd6ZTCljZJ2nvPRy+3KeJI2hqtLkmgj+Y0qvkOYXhQH7Mx2YyMcwudR1ox/6vEVn+X07Y/hofM+gCMF7/uHc0o8AVeAuClC3XXiIS0hGa4y9MNa5Je7xmK5AfC+v/5aOok1mSwO+GhnfL6F4pcXMfOZmj2odnLxHnF99IobhyuUf4p+lx21XdWWklJoHQHK2V8ifnkagyfPs1YACHmvNHIw5S+3IlN3cRwSu1iga6GH8LcSOsGgIt5Z911Nzm/rG7aH5+VhFrIBp7/tr3EotxellhQ+tdLHwJnFyxYQ9r2vC0pydp5x1NKN2Yz4uUiCiE0TGUCqUH3eMtfk0EfdHicX4xcVUD/3Jyd2SGBg42LHS3OnDo+auKpp3tjXNt6V2digHguRQzQMf/TnTnL35+GqgIkEwmfv6/wU67AFjP7mfNZqLV+GFG21VMiAZGa8cMGdPvje0Yfku78I/5ca2V6Fp1uo9G9OCrO3hy3znV16KvC4gdyh7raO32C6CU0uwxXesqIUnB+FwRopohnwIX+tot5auJcNFzQYfRAj1XSIJ0AgdXjv1FLFGr5E1iscN9bNAIcK9GU4CLu9thBgtIAZcjFSOTB9INjyglokc65F2fNpavm13WL+Iu6CjZw1X72m6AR1+8Gg7N7czgdwDyhNVxeNBIqNmr96XqLxIuNwpA3InjSgk5XlNCOgzbXoirEiIO2++pYDi6PQYb3A/2JpugN9rpuNacg==",
  "Expiration" : "2021-01-13T16:05:01Z"
}
```

## Start the Agent

```sh
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json
```

## Check the Status of the Agent

```sh
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a status -m ec2 -s -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json
```

This will return a result in JSON.

```sh
{
  "status": "running",
  "starttime": "2021-01-13T09:11:04+00:00",
  "version": "1.247346.1b249759"
}
```

## Check the CloudWatch Agent Log

The log is very helpful for checking that the agent is indeed working correctly.

```sh
tail -50f /opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log
```

## Check the CloudWatch Console

You should start seeing the logs you have specified in the `config.json` in the CloudWatch Console GUI.
