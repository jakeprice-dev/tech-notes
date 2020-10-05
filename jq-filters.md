---
id: 20201005181713
uuid: 972403d0-a665-44a4-b10d-3f913e38fae1
title: jq Filters
date: 2020-10-05 18:17:13
modified: 
types: tech-note
categories: cli
pinned: false
tags: [jq, json, aws, filter, query]
private: false
draft: false
---

The `jq` tool is a superb, and incredibly frustrating tool to use, all at the same time. It can take JSON as an input, filter and query it, then return it as a completely different, restructured JSON output.

As it's notoriously frustrating, and is not blessed with great documentation, it's vital to have a note like this with common filters.

## Construct a new array

```sh
.[] | {types: .ImageType, Status: .State}
pinned: false
```

### Output

```json
{
  "Type": "machine",
  "Status": "available"
}
{
  "Type": "machine",
  "Status": "available"
}
```

### Source

```json
[
    {
        "Architecture": "x86_64",
        "CreationDate": "2018-11-13T21:26:17.000Z",
        "ImageId": "ami-0839e2fdb9ef50c3a",
        "ImageLocation": "309956199498/RHEL-8.0_HVM_BETA-20181113-x86_64-1-Hourly2-GP2",
        "ImageType": "machine",
        "Public": true,
        "OwnerId": "309956199498",
        "PlatformDetails": "Red Hat Enterprise Linux",
        "UsageOperation": "RunInstances:0010",
        "State": "available",
        "BlockDeviceMappings": [
            {
                "DeviceName": "/dev/sda1",
                "Ebs": {
                    "DeleteOnTermination": true,
                    "SnapshotId": "snap-07931fdf71baedcd2",
                    "VolumeSize": 10,
                    "VolumeType": "gp2",
                    "Encrypted": false
                }
            }
        ],
        "Description": "Provided by Red Hat, Inc.",
        "EnaSupport": true,
        "Hypervisor": "xen",
        "Name": "RHEL-8.0_HVM_BETA-20181113-x86_64-1-Hourly2-GP2",
        "RootDeviceName": "/dev/sda1",
        "RootDeviceType": "ebs",
        "SriovNetSupport": "simple",
		"Tags": [
		  {
			"Key": "Name",
			"Value": "Red Hat 8"
		  },
		  {
			"Key": "Encryption",
			"Value": "True"
		  }
		],		
        "VirtualizationType": "hvm"
    },
    {
        "Architecture": "x86_64",
        "CreationDate": "2019-04-26T19:52:00.000Z",
        "ImageId": "ami-01a76e79ae757048d",
        "ImageLocation": "309956199498/RHEL-8.0.0_HVM-20190426-x86_64-1-Hourly2-GP2",
        "ImageType": "machine",
        "Public": true,
        "OwnerId": "309956199498",
        "PlatformDetails": "Red Hat Enterprise Linux",
        "UsageOperation": "RunInstances:0010",
        "State": "available",
        "BlockDeviceMappings": [
            {
                "DeviceName": "/dev/sda1",
                "Ebs": {
                    "DeleteOnTermination": true,
                    "SnapshotId": "snap-03f48e46a804d09f9",
                    "VolumeSize": 10,
                    "VolumeType": "gp2",
                    "Encrypted": false
                }
            }
        ],
        "Description": "Provided by Red Hat, Inc.",
        "EnaSupport": true,
        "Hypervisor": "xen",
        "Name": "RHEL-8.0.0_HVM-20190426-x86_64-1-Hourly2-GP2",
        "RootDeviceName": "/dev/sda1",
        "RootDeviceType": "ebs",
        "SriovNetSupport": "simple",
		"Tags": [
		  {
			"Key": "Name",
			"Value": "Red Hat 8"
		  },
		  {
			"Key": "Encryption",
			"Value": "True"
		  }
		],			
        "VirtualizationType": "hvm"
    }
]
```

## Select tag value

```sh
.[] | {ImageId: .ImageId, Name: .Name, Release: (.Tags[]|select(.Key=="Encryption")|.Value)}
```

### Output

```json
{
  "ImageId": "ami-0839e2fdb9ef50c3a",
  "Name": "RHEL-8.0_HVM_BETA-20181113-x86_64-1-Hourly2-GP2",
  "Release": "True"
}
{
  "ImageId": "ami-01a76e79ae757048d",
  "Name": "RHEL-8.0.0_HVM-20190426-x86_64-1-Hourly2-GP2",
  "Release": "True"
}
```

### Source

```json
[
    {
        "Architecture": "x86_64",
        "CreationDate": "2018-11-13T21:26:17.000Z",
        "ImageId": "ami-0839e2fdb9ef50c3a",
        "ImageLocation": "309956199498/RHEL-8.0_HVM_BETA-20181113-x86_64-1-Hourly2-GP2",
        "ImageType": "machine",
        "Public": true,
        "OwnerId": "309956199498",
        "PlatformDetails": "Red Hat Enterprise Linux",
        "UsageOperation": "RunInstances:0010",
        "State": "available",
        "BlockDeviceMappings": [
            {
                "DeviceName": "/dev/sda1",
                "Ebs": {
                    "DeleteOnTermination": true,
                    "SnapshotId": "snap-07931fdf71baedcd2",
                    "VolumeSize": 10,
                    "VolumeType": "gp2",
                    "Encrypted": false
                }
            }
        ],
        "Description": "Provided by Red Hat, Inc.",
        "EnaSupport": true,
        "Hypervisor": "xen",
        "Name": "RHEL-8.0_HVM_BETA-20181113-x86_64-1-Hourly2-GP2",
        "RootDeviceName": "/dev/sda1",
        "RootDeviceType": "ebs",
        "SriovNetSupport": "simple",
		"Tags": [
		  {
			"Key": "Name",
			"Value": "Red Hat 8"
		  },
		  {
			"Key": "Encryption",
			"Value": "True"
		  }
		],		
        "VirtualizationType": "hvm"
    },
    {
        "Architecture": "x86_64",
        "CreationDate": "2019-04-26T19:52:00.000Z",
        "ImageId": "ami-01a76e79ae757048d",
        "ImageLocation": "309956199498/RHEL-8.0.0_HVM-20190426-x86_64-1-Hourly2-GP2",
        "ImageType": "machine",
        "Public": true,
        "OwnerId": "309956199498",
        "PlatformDetails": "Red Hat Enterprise Linux",
        "UsageOperation": "RunInstances:0010",
        "State": "available",
        "BlockDeviceMappings": [
            {
                "DeviceName": "/dev/sda1",
                "Ebs": {
                    "DeleteOnTermination": true,
                    "SnapshotId": "snap-03f48e46a804d09f9",
                    "VolumeSize": 10,
                    "VolumeType": "gp2",
                    "Encrypted": false
                }
            }
        ],
        "Description": "Provided by Red Hat, Inc.",
        "EnaSupport": true,
        "Hypervisor": "xen",
        "Name": "RHEL-8.0.0_HVM-20190426-x86_64-1-Hourly2-GP2",
        "RootDeviceName": "/dev/sda1",
        "RootDeviceType": "ebs",
        "SriovNetSupport": "simple",
		"Tags": [
		  {
			"Key": "Name",
			"Value": "Red Hat 8"
		  },
		  {
			"Key": "Encryption",
			"Value": "True"
		  }
		],			
        "VirtualizationType": "hvm"
    }
]
```

## Select array's that match exact value

```
jq --raw-output '.prefixes[] | select(.service == "ROUTE53_HEALTHCHECKS") | .ip_prefix'
```

### Output

```json
{
  "ip_prefix": "3.5.140.0/22",
  "region": "ap-northeast-2",
  "service": "ROUTE53_HEALTHCHECKS",
  "network_border_group": "ap-northeast-2"
}
{
  "ip_prefix": "35.180.0.0/16",
  "region": "eu-west-3",
  "service": "ROUTE53_HEALTHCHECKS",
  "network_border_group": "eu-west-3"
}
```

### Source

```json
{
  "syncToken": "1608245058",
  "createDate": "2020-12-17-22-44-18",
  "prefixes": [
    {
      "ip_prefix": "52.93.178.234/32",
      "region": "us-west-1",
      "service": "AMAZON",
      "network_border_group": "us-west-1"
    },
    {
      "ip_prefix": "3.5.140.0/22",
      "region": "ap-northeast-2",
      "service": "ROUTE53_HEALTHCHECKS",
      "network_border_group": "ap-northeast-2"
    },
    {
      "ip_prefix": "52.95.36.0/22",
      "region": "ap-southeast-2",
      "service": "AMAZON",
      "network_border_group": "ap-southeast-2"
    },
    {
      "ip_prefix": "3.2.0.0/24",
      "region": "us-east-1",
      "service": "AMAZON",
      "network_border_group": "us-east-1-iah-1"
    },
    {
      "ip_prefix": "35.180.0.0/16",
      "region": "eu-west-3",
      "service": "ROUTE53_HEALTHCHECKS",
      "network_border_group": "eu-west-3"
    },    
    {
      "ip_prefix": "13.248.56.0/22",
      "region": "ap-east-1",
      "service": "AMAZON",
      "network_border_group": "ap-east-1"
    }
  ]
}
```
