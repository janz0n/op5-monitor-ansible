Ansible Role for OP5 Monitor
=========

![GitHub](https://img.shields.io/github/license/janz0n/op5-monitor-ansible.svg)
![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-janz0n.op5monitor-blue.svg)
![GitHub tag (latest by date)](https://img.shields.io/github/tag-date/janz0n/op5-monitor-ansible.svg?label=version)
![Ansible Quality Score](https://img.shields.io/ansible/quality/40176.svg)

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
op5_timezone: Europe/Stockholm # timezone - use "timedatectl list-timezones", empty string will not config timezone
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
op5_ntp_servers:               # ntp servers to use, empty string will not config chronyd and ntp-servers
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

op5_monitor_version: 8       # 7 or 8 - will install latest micro release within major
op5_monitor_state: installed # installed or latest
---
op5_postconfig: true                          # true, false - import tasks to playbook

op5_smsd_device: "/dev/ttyS0"                 # serial port to use, empty string will not config smsd settings
op5_smsd_pin: ""                              # sim-pin, empty string equals no pin
op5_smsd_baudrate: 115200                     # baudrate for sms-modem: TC35 - 38400, TC65 115200
op5_postfix_relayhost: ""                     # [smtp.domain.tld] 
op5_postfix_fallback_relay: ""                # [smtp.domain.tld]
op5_postfix_postfix_inet_protocols: "ipv4"    # all, ipv4 ,ipv6
op5_postfix_smtp_tls_security_level: ""       # e.g encrypt
op5_postfix_smtp_tls_mandatory_ciphers: ""    # e.g high
op5_backup_storagepath: "/root/op5backups/"   # path where to store backups, empty string will not config op5backups
op5_backup_run: "59 1 * * *"                  # when to run op5-backup, cron format
op5_backup_rotate: "5 3 * * *"                # when to remove backups, cron format
op5_backup_rotate_days: "7"                   # older than X backup files, should be deleted
---
op5_optimize: false # true, false - import tasks to playbook

op5_synergy_sleep: 300  # e.g 60
op5_trapper: true  # true, false
op5_logger: true # true, false
op5_php_memory_limit: 128 # x MB
op5_php_max_execution_time: 30 # x seconds
op5_php_max_input_vars: 1000 # x vars
```

Dependencies
------------

None

New to Ansible
--------------

```
ansible-galaxy install --roles-path . janz0n.op5monitor
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
    - role: janz0n.op5monitor
      vars:
        op5_hostname: op501.test.local
        op5_timezone: Europe/Stockholm
```

**op5monitor only, preconfig and postconfig handled by other roles/tasks**

```
- name: Playbook::op5-monitor only
  hosts: op5-servers 
 
  roles:
    - role: janz0n.op5monitor
      vars: 
        op5_preconfig: false  
        op5_monitor: true
        op5_postconfig: false
```

**op5monitor update**
```
- name: Playbook::op5-monitor update
  hosts: op5-servers 
 
  roles:
    - role: janz0n.op5monitor
      vars:
        op5_preconfig: true
        op5_all_pkgs_state: installed
        op5_monitor_state: latest
        op5_postconfig: true
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
        License
        Certificate

    Verification
        Disk Usage
        CPU Usage
        Error logs
        NTP
        Email
        SMS
        Services enabled
```
