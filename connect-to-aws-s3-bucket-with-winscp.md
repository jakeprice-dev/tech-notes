---
id: 20200302134351
uuid: d4b91bfc-8944-4da6-aa95-544ba8ea7b49
title: Connect to an AWS S3 Bucket with WinSCP
date: 2020-03-02 13:43:51
modified: 
types: tech-note
categories: aws
pinned: false
tags: [s3, aws, winscp, bucket]
private: false
draft: false
---

On the initial "Site Login" screen that opens when WinSCP is launched, fill in the below details:

```yaml
File protocol: Amazon S3
Host name: s3.amazonaws.com
Access key ID: <access-key>
Secret access key: <secret-access-key>
```

Then click on `Advanced`, navigate to `Environments > Directories` and fill in the remote directory (which is the S3 bucket name).

```yaml
Remote directory: /<bucket-name>
```
