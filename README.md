NRPE Configuration
==================

An Ansible role to handle the installation and rollout of the Nagios NRPE Daemon.

This role currently supports:
  - Red Hat Enterprise Linux 7
  - CentOS 7
  - Fedora versions


Requirements
------------
* EPEL needs to be installed, and configured, on Red Hat, Scientific Linux, CentOS and any other RHEL-derivative. Fedora systems should function properly without this requirement.


Role Information
--------------

This role gives you the ability to deploy plugins on a global and per-server basis. This can be done by:
  * putting plugins into [`files/plugins/global`](files/plugins/global) or
  * by creating a folder in `files/plugins/` that is the servers [FQDN](http://en.wikipedia.org/wiki/Fully_qualified_domain_name).

You can find out your servers FQDN by running the [Ansible Setup](http://docs.ansible.com/setup_module.html) module.


OS Specific Role variables
--------------------------
These following variable are Operating System, or at least Operating System
family specific. They are configurable, but change at your own risk!

  * *nagios_nrpe_server_pid*: /var/run/nrpe/nrpe.pid
  * *nagios_nrpe_server_user*: nrpe
  * *nagios_nrpe_server_group*: nrpe
  * *nagios_nrpe_server_repo_redhat*: epel
  * *nagios_nrpe_server_service*: nrpe
  * *nagios_nrpe_server_dir*: /etc/nagios


Role Variables
--------------
  * *nrpe_data*: Filesystem tree (local-only) where extra checks need to reside.
  * *nrpe*: base configuration variable for NRPE. This variable acts as a dictionary (hash) of nrpe configuration options.
  * *nrpe_defaults*: Default options are available in the [`vars/RedHat.yml`](vars/RedHat.yml) file. The defaults are used of no corresponding nrpe[option] variable is set.
  * *nrpe_<configuration_option>*: The override option exists for all nrpe variables. To override the default and the base configuration variable -- for instance, in a specific host_vars folder -- use this configuration option.
  * *nrpe_command*: This variable contains the default, and system-specific, nrpe check options.

Default Role variables
-----------------
```yaml
---
#
# Check for virtualization types. If we're virtualized (mainly testing), then
# don't try to manage or reload the service. Do not change unless you know what
# you are doing!
#
nrpe_manage_service: {}
nrpe_allow_restart: {}

#
# Should we skip the defaults provided by the operating system? This should
# typically be set to false. If you are generating a completely independent
# configuration file, then you should probably set this to try and override
# the nrpe variable below.
#
nrpe_skip_defaults: false

#
# By default, we set the os_supported variable to "no". This way, we can
# override it in the vars defaults files for any OS that we can support with
# this role. This short-circuits the execution of the playbook for not EL-
# derivatives.
#
nrpe_os_supported: no

#
# By default, inherit from the Red Hat nrpe_defaults variables.
#
nrpe: {}
nrpe_command: {}
```

```yaml
$> cat group_vars/all
nrpe:
  server_address: 127.0.0.1
  bind_address: 127.0.0.1
  allowed_hosts:
    - nagios-server.example.com
    - 127.0.0.1
  port: 5666
  dont_blame_nrpe: 0
  log_facility: daemon

nrpe_command:
  default:
    oracle_tnsping:
      script: check_oracle_health
      option: --mode tnsping
    oracle_connection-time:
      script: check_oracle_health
      option: --mode connection-time
  'hostname.domain':
    check_uptime:
      script: check_uptime
      option: -u days -w 90 -c 120
```

```yaml
$> cat host_vars/host.example.com
---
#
# NRPE runs on port 5667 on this host, so this variable overrides nrpe['port']
# in the group_vars/all file.
#
nrpe_port: 5667
```

Dependencies
------------
none

Example Playbook
----------------

```yaml
- hosts: servers
  roles:
     - ISU-Ansible.nrpe
  vars:
    nrpe:
      allowed_hosts:
        - 192.168.0.1
        - 127.0.0.1
    nrpe_command:
      default:
        oracle_tnsping:
          script: check_oracle_health
          option: --mode tnsping
        oracle_connection-time:
          script: check_oracle_health
          option: --mode connection-time
      'hostname.domain':
        check_uptime:
          script: check_uptime
          option: -u days -w 90 -c 120
```

License
-------
MIT

Author Information
------------------
* Barry Britt

This role was adapted from the nagios-nrpe-server role by jloh. Checkout his blog [here](http://blog.jloh.co).
