Nagios NRPE Server Config
=========

[![GitHub version](https://badge.fury.io/gh/jloh%2Fnagios-nrpe-server.svg)](http://badge.fury.io/gh/jloh%2Fnagios-nrpe-server) [![Build Status](https://travis-ci.org/jloh/nagios-nrpe-server.svg?branch=master)](https://travis-ci.org/jloh/nagios-nrpe-server)

An Ansible role to handle the installation and rollout of the Nagios NRPE Daemon.

This role currently supports:
  - Red Hat Enterprise Linux 7
  - CentOS 7
  - Fedora versions

Requirements
------------

RedHat based OS's must already have the EPEL repo installed and configured.

Role Information
--------------

This role gives you the ability to deploy plugins on a global and per-server basis. This can be done by:
* putting plugins into [`files/plugins/global`](files/plugins/global) or
* by creating a folder in `files/plugins/` that is the servers [FQDN](http://en.wikipedia.org/wiki/Fully_qualified_domain_name).

You can find out your servers FQDN by running the [Ansible Setup](http://docs.ansible.com/setup_module.html) module.

Role Variables
--------------

  * *nagios_nrpe_server_bind_address*: 127.0.0.1
  * *nagios_nrpe_server_port*: 5666
  * *nagios_nrpe_server_allowed_hosts*: 127.0.0.1
  * *nagios_nrpe_command*: see example playbook section

These are OS specific and likely wont want to be changed

RedHat:

  * *nagios_nrpe_server_pid*: /var/run/nrpe/nrpe.pid
  * *nagios_nrpe_server_user*: nrpe
  * *nagios_nrpe_server_group*: nrpe
  * *nagios_nrpe_server_repo_redhat*: epel
  * *nagios_nrpe_server_service*: nrpe
  * *nagios_nrpe_server_dir*: /etc/nagios

Dependencies
------------

N/A

Example Playbook
----------------

```yaml
- hosts: servers
  roles:
     - ISU-Ansible.nrpe
  vars:
    nagios_nrpe_server_allowed_hosts:
      - 192.168.0.1
      - 127.0.0.1
    nagios_nrpe_command:
      oracle_tnsping:
        script: check_oracle_health
        option: --mode tnsping
      oracle_connection-time:
        script: check_oracle_health
        option: --mode connection-time
```

License
-------
MIT

Author Information
------------------
* Barry Britt

This role was adapted from the nagios-nrpe-server role by jloh. Checkout his blog [here](http://blog.jloh.co).
