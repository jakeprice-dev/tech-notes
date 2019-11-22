---
id: 20191122173025
uuid: bef8e458-5553-45e3-8be7-ccdfc3e5231f
title: View and Log Detailed Bash Script Output
date: 2019-11-22 17:30:25
modified: 
types: tech-note
categories: bash
pinned: false
tags: [log, bash, script, stdout, stderr, debug, tee, background, process, run]
private: false
draft: false
---

Append `-xe` to `#!/bin/bash` in the script you wish to see detailed output for.

```sh
#!/bin/bash -xe
```

Then run it as below to see the debug output and stdout and stderr on the screen, _but_ also have it logged to a file.

``` sh
./script.sh 2>&1 | tee output.log
```
