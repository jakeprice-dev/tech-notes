---
id: 20210302162313
uuid: 78a322a3-53b5-46ec-9c3d-096257c90e88
title: AWS CloudWatch Insights Average
date: 2021-03-02 16:23:13
modified: 
types: tech-note
categories: aws
pinned: false
tags: [syntax, cloudwatch, logs, insights, average]
private: false
draft: false
---

[CloudWatch Logs Insights Query Syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_QuerySyntax.html)

Parsing the below log entry.

```log
2021-03-02 00:04:29,532 INFO  [backendTaskScheduler-2] control.NodeHealthMonitor - 1609@ip-10-0-1-104#vFR NodeHealth[updated=2021-03-02T00:04:29.532, host='10.0.1.104', pid='1609', maxMemory=12884901886, usedMemory=418226480, usedMemoryOneHourMax=421286304, usedMemoryOneDayMax=427718856, member='[uuid:ab992f4b-305e-4100-8bf7-390168f5dd30,port:5701,lite:false]', mode=AS_SERVER, joined=2021-03-01T16:49:04.346, left=null, state=READY]
```

The below will plot the average used memory and output a line graph visualisation.

```sql
parse @message "usedMemory=*," as usedmem
| stats avg(usedmem) by bin(5m)
```

The below will output the average used memory as a single figure only.

```sql
parse @message "usedMemory=*," as usedmem
| avg(usedmem)
```
