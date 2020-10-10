---
id: 20201010115730
uuid: 7357063d-0081-4aed-bb46-b7046ab97161
title: Hide Tomcat Version Details on 404 Page
date: 2020-10-10 11:57:30
modified: 
types: tech-note
categories: tomcat
tags: [tomcat, apache, version, security]
private: false
draft: false
---

It's important to hide the version of Tomcat that is being used to ensure the baddies are unsure!

Create a new set of directories under the Tomcat `lib` directory:

```sh
mkdir -p /opt/apache/tomcat/lib/org/apache/catalina/util
```

Then create a new file under the newly created `util` directory called `ServerInfo.properties`, and copy the below into it:

```ini
server.info=
```

The `server.info` property takes a string, but it's better to have no information on version at all, so the above hides it completely.

Then restart Tomcat, and if you enter a bad request the version will not be included.

