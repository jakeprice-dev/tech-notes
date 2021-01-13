---
id: 20210113204421
uuid: 16db2b23-1918-4c98-9474-83cfe34b4800
title: Version Locking Ansible Roles in a Playbook
date: 2021-01-13 20:44:21
modified: 
types: tech-note
categories: ansible
pinned: false
tags: [roles, playbook, ansible, git]
private: false
draft: false
---

In example directory: `~/code/ansible-playbooks-system1/system1` create an additional directory, `roles`

```txt
system1/
├── ansible.cfg
├── system1_uat.yml
├── group_vars
│   └── all
└── roles
    └── requirements.yml
```

Within `roles/` create a file, `requirements.yml`.

Use `requirements.yml` to specify role requirements, and lock them to specific Git tags.

```yml
---    
- src: git+git@git.example.dev:ansible-roles/common.git
  version: v1.0.2
  name: common
```

In the playbook you can now use the role `name` without having to specify a local path. See below.

```yml
- name: Install & Configure Server
  hosts: "{{ host_group }}[0]"
  vars_files:
    - ../../terraform-environments-system1/system1/tf_ansible_variables_file.yml
  roles:
      - common
  become: true
```

You now need to install the role into `roles/` so we can use `-p` to do that.

```sh
$ ansible-galaxy install -r roles/requirements.yml -p roles/
- extracting common to /home/jprice/code/ansible-playbooks-system1/system1-2/roles/common
- common (v1.0.2) was installed successfully
```

When you run `ansible-playbook` for the play in this directory, it'll use this version of the role. You can override a role version using the `--force` flag. So if you wanted to downgrade, or upgrade you would do the below.

```sh
→ ansible-galaxy install -r roles/requirements.yml -p roles/ --force
- changing role common from v1.0.2 to v1.0.1
- extracting common to /home/jprice/code/ansible-playbooks-system1/system1-2/roles/common
- common (v1.0.1) was installed successfully
```

This way you can version lock roles using Git tags, and ensure that each environment only uses a role that is tested with that environment, just like we do with Terraform. It's just a bit more obtuse with Ansible.

Remember as well, that Terraform also downloads the roles into the environment directory, just as we're doing with the roles here. It's just within a hidden folder in Terraform.
