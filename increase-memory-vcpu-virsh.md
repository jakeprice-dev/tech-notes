---
id: 20220221225005
uuid: 37ada0cf-74b3-4c04-9648-f5cf8901f46d
title: Increase Memory & Virtual CPU on Virtual Machine
date: 2022-02-21 22:50:05
modified: 
types: tech-note
categories: libvirt
link: 
pinned: false
tags: [virsh, vm, domain, memory, ram, vcpu, cpu, max, maximum]
private: false
draft: false
---

```sh
# Shutdown domain:
virsh shutdown <domain>

# Increase max memory:
virsh setmaxmem <domain> 4G --config

# Increase memory:
virsh setmem <domain> 4G --config

# ---

# Set max vcpu:
virsh setvcpus --domain <domain> --maximum 2 --config

# Set vcpu:
virsh setvcpus --domain <domain> --count 2 --config

# Restart domain:
virsh start <domain>
```

