---
id: 20191010121200
uuid: de0896b8-638b-4235-9edb-cbfef28edac7
title: Find The Most Recent AWS AMI
date: 2019-10-10 12:12:00
modified: 
types: tech-note
categories: aws
pinned: false
tags: [aws, ami]
private: false
draft: false
---

The below command will return the most recent AWS AMI in the region you specify. You can update the `--owners` parameter to match one of the below common 'owners' to find the AMI for the distribution you wish to use.

  - `099720109477` = Ubuntu
  - `679593333241` = CentOS
  - `309956199498` = Red Hat

To make sure you only find the version of the distro you want, the easiest way to do it is to filter the `Name` key value. You would need to update the `Name=name,Values=<version-string-filter>`. Below are some examples for the most recent versions of the above owners distributions.

  - `ubunt?/images/hvm-ssd/ubuntu--18.04-amd64-server` = Ubuntu 18.04
  - `CentOS Linux 7*` = CentOS 7
  - `RHEL-8*` = Red Hat 8

The actual command is below.

``` sh
aws ec2 describe-images \
  --owners <owner-id> \
  --query "sort_by(Images, &CreationDate)[*]" \
  --filters "Name=name,Values=<version-string-filter>" "Name=root-device-type,Values=ebs" "Name=architecture,Values=x86_64" \
  --region <aws-region> \
  | sed "s/.000Z/Z/" \
  | jq -r "[.[] | select(.CreationDate | . == null or fromdateiso8601 > <unix-time>).ImageId] | reverse [0]"
```

How the command is structured below.

``` sh
# Call AWS CLI to describe available AMIs.
aws ec2 describe-images

# Specify an AMI Owner ID.
--owners <owner-id>

# Using JMESPATH sort AMIs by creation date.
--query "sort_by(Images, &CreationDate)[*]"

# Filter the returned JSON to narrow down the options.
--filters "Name=name,Values=<version-string-filter>" "Name=root-device-type,Values=ebs" "Name=architecture,Values=x86_64"

# Specify the AWS Region.
--region <aws-region>

# We pipe the output to sed, as jq doesn't fully support the time format AWS returns for the creation date of an AMI. To fix this we simply strip the unsupported section out, and replace it supported content.
sed "s/.000Z/Z/"

# We pipe it again, this time into jq. Firstly we create a new array, and reverse it to easily allow us to make sure we select the most recent AMI date. Next we use select on the creation date key, and grab any key that is older than a unix time stamp we specify. The -r strips the quotes, and we receive an AMI string back.
jq -r "[.[] | select(.CreationDate | . == null or fromdateiso8601 > <unix-time>).ImageId] | reverse [0]"
```

If in doubt, it may be helpful to swap out part of the `jq` string. If you replace `ImageId` with `CreationDate` then the command will return the actual date as a string. That way you can easily tell if it's actually working or not, as you should see a date newer than the unix date you specify (rather than older).

The below command is slightly different, it is best run with the `--output` set to `table`. This allows you to see a nice visual output of results, especially handy if you are trying to decipher the common names, owners may use for their AMI's.

Additionally you could run it with a less restrictive version name for `Name=name,Values=` under `--filters` which would return all versions available to you, based on other filters. For example instead of searching for `CentOS Linux 7*` you could just search for `CentOS Linux*` and see older releases. This can come in handy if you want the most recent AMI for an older distro such as Ubuntu 16.04.

Here's a real-life example.

``` sh
aws ec2 describe-images --owners 679593333241 --query "sort_by(Images, &CreationDate)[*].{Date:CreationDate,ImageName:Name,Image:ImageId,Description:Description,OwnerID:OwnerId,ImageType:Architecture,DeviceType:RootDeviceType,Virtualization:VirtualizationType}" --filters "Name=name,Values=CentOS Linux*" "Name=root-device-type,Values=ebs" "Name=architecture,Values=x86_64" --region eu-west-1 --output table
```

You would get a nice table back like the below.

``` txt
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
|                                                                                                                             DescribeImages                                                                                                                              |
+--------------------------+--------------------------------------------+-------------+------------------------+----------------------------------------------------------------------------------------------------------+------------+---------------+------------------+
|           Date           |                Description                 | DeviceType  |         Image          |                                                ImageName                                                 | ImageType  |    OwnerID    | Virtualization   |
+--------------------------+--------------------------------------------+-------------+------------------------+----------------------------------------------------------------------------------------------------------+------------+---------------+------------------+
|  2015-10-13T20:24:47.000Z|  CentOS Linux 7 x86_64 HVM EBS 20150928_01 |  ebs        |  ami-33734044          |  CentOS Linux 7 x86_64 HVM EBS 20150928_01-b7ee8a69-ee97-4a49-9e68-afaee216db2e-ami-69327e0c.2           |  x86_64    |  679593333241 |  hvm             |
|  2016-02-26T21:22:50.000Z|  CentOS Linux 7 x86_64 HVM EBS 1602        |  ebs        |  ami-7abd0209          |  CentOS Linux 7 x86_64 HVM EBS 1602-b7ee8a69-ee97-4a49-9e68-afaee216db2e-ami-d7e1d2bd.3                  |  x86_64    |  679593333241 |  hvm             |
|  2017-05-10T18:54:59.000Z|  CentOS Linux 7 x86_64 HVM EBS 1704_01     |  ebs        |  ami-061b1560          |  CentOS Linux 7 x86_64 HVM EBS 1704_01-b7ee8a69-ee97-4a49-9e68-afaee216db2e-ami-d52f5bc3.4               |  x86_64    |  679593333241 |  hvm             |
|  2017-09-13T14:39:42.000Z|  CentOS Linux 7 x86_64 HVM EBS 1708_01     |  ebs        |  ami-5f76b626          |  CentOS Linux 7 x86_64 HVM EBS 1708_01-b7ee8a69-ee97-4a49-9e68-afaee216db2e-ami-0d8f9576.4               |  x86_64    |  679593333241 |  hvm             |
|  2017-12-05T14:49:45.000Z|  CentOS Linux 7 x86_64 HVM EBS 1708_11.01  |  ebs        |  ami-192a9460          |  CentOS Linux 7 x86_64 HVM EBS 1708_11.01-b7ee8a69-ee97-4a49-9e68-afaee216db2e-ami-95096eef.4            |  x86_64    |  679593333241 |  hvm             |
|  2018-01-12T20:39:08.000Z|  CentOS Linux 7 x86_64 HVM EBS 1801_01     |  ebs        |  ami-6e28b517          |  CentOS Linux 7 x86_64 HVM EBS 1801_01-b7ee8a69-ee97-4a49-9e68-afaee216db2e-ami-0a537770.4               |  x86_64    |  679593333241 |  hvm             |
|  2018-04-04T00:13:35.000Z|  CentOS Linux 7 x86_64 HVM EBS ENA 1803_01 |  ebs        |  ami-1caef165          |  CentOS Linux 7 x86_64 HVM EBS ENA 1803_01-b7ee8a69-ee97-4a49-9e68-afaee216db2e-ami-8274d6ff.4           |  x86_64    |  679593333241 |  hvm             |
|  2018-05-17T09:06:17.000Z|  CentOS Linux 7 x86_64 HVM EBS ENA 1804_2  |  ebs        |  ami-4c457735          |  CentOS Linux 7 x86_64 HVM EBS ENA 1804_2-b7ee8a69-ee97-4a49-9e68-afaee216db2e-ami-55a2322a.4            |  x86_64    |  679593333241 |  hvm             |
|  2018-06-13T15:56:35.000Z|  CentOS Linux 7 x86_64 HVM EBS ENA 1805_01 |  ebs        |  ami-3548444c          |  CentOS Linux 7 x86_64 HVM EBS ENA 1805_01-b7ee8a69-ee97-4a49-9e68-afaee216db2e-ami-77ec9308.4           |  x86_64    |  679593333241 |  hvm             |
|  2019-01-30T23:43:59.000Z|  CentOS Linux 7 x86_64 HVM EBS ENA 1901_01 |  ebs        |  ami-0ff760d16d9497662 |  CentOS Linux 7 x86_64 HVM EBS ENA 1901_01-b7ee8a69-ee97-4a49-9e68-afaee216db2e-ami-05713873c6794f575.4  |  x86_64    |  679593333241 |  hvm             |
+--------------------------+--------------------------------------------+-------------+------------------------+----------------------------------------------------------------------------------------------------------+------------+---------------+------------------+
```
