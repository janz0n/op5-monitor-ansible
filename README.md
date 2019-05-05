Ansible Role for OP5 Monitor
=========

OP5 Monitor is a software product for server, network monitoring and management based on the Open Source project Naemon. This repository contains OP5 Monitor ansible role.

![OP5 Monitor Ansible Role](https://user-images.githubusercontent.com/48694372/56851072-2d796d00-690b-11e9-9fe8-999b1696f268.png)

> **Please be aware OP5 does not support this deployment type at this time.

Requirements
------------

* Target Host(s) Requires Internet Access
* Target Host(s) Supported OSes are CentOS7 and RHEL7.
* Ansible 2.7+

Role Variables
--------------

Available variables are listed below, along with default values (see defaults/main/):

```
---
op5_preconfig: true            # true, false - import tasks to playbook 

op5_hostname: op501.test.local # FQDN without trailing dot
op5_timezone: Europe/Stockholm # timezone - use "timedatectl list-timezones"
op5_all_pkgs_state: latest     # installed or latest 
op5_selinux_state: permissive  # enforcing, permissive, disabled
op5_firewalld_enabled: yes     # yes, no
op5_firewalld_state: started   # started, stopped, reloaded, restarted
op5_firewalld_allowed_ports:   # allowed ports, public zone
  - 80/tcp
  - 443/tcp
  - 5666/tcp
  - 15551/tcp
  - 161/udp
  - 162/udp
  - 514/udp
op5_ntp_servers:               # ntp servers to use
  - 0.pool.ntp.org
  - 1.pool.ntp.org
  - 2.pool.ntp.org
  - 3.pool.ntp.org
op5_opt_pkgs:                  # optional packages to install
  - libselinux-python
  - net-tools
  - telnet
  - vim
  - git
  - gzip
  - bzip2
  - unzip
  - open-vm-tools
---
op5_monitor: true            # true, false - import tasks to playbook

op5_monitor_version: 8       # 7 or 8 - latest micro release within major
op5_monitor_state: installed # installed or latest
---
op5_postconfig: true                          # true, false - import tasks to playbook

op5_smsd_device: "/dev/ttyS0"                 # serial port to use
op5_smsd_pin: ""                              # sim-pin, empty equals no pin
op5_smsd_baudrate: 115200                     # baudrate for sms-modem: TC35 - 38400, TC65 115200
op5_backup_storagepath: "/root/op5backups/"   # path where to store backups
op5_backup_run: "59 1 * * *"                  # when to run op5-backup, cron format
op5_backup_rotate: "5 3 * * *"                # when to remove backups, cron format
op5_backup_rotate_days: "7"                   # older than X backup files, should be deleted
---
op5_optional: false # true, false - import tasks to playbook
---
op5_optimize: false # true, false - import tasks to playbook
---
op5_custom: false # true, false - import tasks to playbook
```

Dependencies
------------

None

New to Ansible
--------------

```
cd op5monitor/tests
```

edit *hosts*

```
ansible-playbook --list-tags op5monitor.yml
ansible-playbook --list-tasks op5monitor.yml
ansible-playbook --ask-pass -u user --ask-become-pass --check op5monitor.yml
ansible-playbook --ask-pass -u user --ask-become-pass op5monitor.yml
ansible-playbook --ask-pass -u root op5monitor.yml
```

Example Playbooks
----------------

**op5monitor**

```
- name: Playbook::op5monitor
  hosts: op5-servers 
 
  roles:
    - role: op5monitor
```

**op5monitor only, preconfig and postconfig handled by other roles/tasks**

```
- name: Playbook::op5-monitor only
  hosts: op5-servers 
 
  roles:
    - role: op5monitor
      vars: 
        preconfig: false  
        op5_monitor: true
        postconfig: false
```

**op5monitor update**
```
- name: Playbook::op5-monitor update
  hosts: op5-servers 
 
  roles:
    - role: op5monitor
      vars:
        preconfig: true
        all_pkgs_state: installed
        op5_monitor_state: latest
        postconfig: true
```

License
-------

GPLv3

Author Information
------------------

**Henrik Janzon**

* [github/janz0n](https://github.com/janz0n)

Roadmap
-----------------

```
    PostConfig
        Cockpit
        PostFix
        License
        Certificate
        Optimizations 

    Verification
        Disk Usage
        CPU Usage
        Error logs
        NTP
        Email
        SMS
        Services enabled
```
