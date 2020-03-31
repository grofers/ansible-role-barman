grofers.ansible-role-barman [![Build Status](https://travis-ci.org/grofers/ansible-role-barman.svg?branch=master)](https://travis-ci.org/grofers/ansible-role-barman)
=========

💥 Battle-tested at [Grofers](https://grofers.com/)

Ansible role which installs and configures
[barman by 2ndQuadrant](http://www.pgbarman.org/) on debian and epel based distros only
(Tested with Ubuntu 14.04 only and Centos 7, but should work with other distros as well).

Installation
------------

This has been tested on Ansible 2.1.0 and higher.
To install:

```bash
ansible-galaxy install grofers.barman
```

Role Variables
--------------

define of server list for backup:

```yaml
barman_server_configuration:
```

Settings for server:

name of reserved server

```yaml
  - name: ssh
```

```yaml
description: "Example of PostgreSQL Database (via SSH)"
```

```yaml
conninfo: "host=pg user=barman dbname=postgres"
```

Define backup method (rsync|postgres)

```yaml
backup_method: "rsync"
```

If method rsync

```yaml
ssh_command: "ssh postgres@pg"
```

```yaml
# defaults file for ansible-role-barman
barman_client_only: no

## APT settings
barman_postgresql_apt_key_id: ACCC4CF8
barman_postgresql_apt_key_url: "https://www.postgresql.org/media/keys/ACCC4CF8.asc"
barman_postgresql_apt_repository: "deb http://apt.postgresql.org/pub/repos/apt/ {{ansible_distribution_release}}-pgdg main"
# Pin-Priority of PGDG repository
barman_postgresql_apt_pin_priority: 500

## Cron Configuration
barman_cron_disabled: false
# Run barman cron every minute
barman_cron_schedule:
  minute: "*"
  hour: "*"
  day: "*"
  weekday: "*"
  month: "*"

## Barman Config
barman_user: "barman"
barman_configuration_files_directory: "/etc/barman.d"
barman_home: "/var/lib/barman"
barman_log_directory: "/var/log/barman"
barman_log_file: "{{ barman_log_directory }}/barman.log"
barman_log_level: "INFO"

barman_server_configuration:
  - name: ssh
    description: "Example of PostgreSQL Database (via SSH)"
    ssh_command: "ssh postgres@pg"
    conninfo: "host=pg user=barman dbname=postgres"
    backup_method: "rsync"
    reuse_backup: "None"
    backup_options: "exclusive_backup"
    archiver: "on"
    archiver_batch_size: 50
    path_prefix: ''
    cron_disabled: false
    cron_schedule:
      minute: "0"
      hour: "0"
      day: "*"
      month: "*"
      weekday: "*"
```
There are a lot more optional variables, please see defaults/main.yml for all
of them.

Example Playbook
----------------

```yaml
    - name: Setup and configure barman
      become: yes
      roles:
        - grofers.barman
```

License
-------

[MIT](LICENSE)

Author Information
------------------

[vishesh92](github.com/vishesh92)
[Filip Krahl](https://github.com/FLiPp3r90)
