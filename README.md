## keepalived

[![Build Status](https://travis-ci.org/Oefenweb/ansible-keepalived.svg?branch=master)](https://travis-ci.org/Oefenweb/ansible-keepalived) [![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-keepalived-blue.svg)](https://galaxy.ansible.com/tersmitten/keepalived)

Set up the latest or a specific version of [Keepalived](http://www.keepalived.org/) in Ubuntu systems.

#### Requirements

- `git-core` (will be installed)
- `build-essential` (will be installed)
- `automake` (will be installed)
- `pkg-config` (will be installed)
- `libssl-dev` (will be installed)

#### Variables

* `keepalived_version`: [default: `v1.3.2`]: Keepalived version to install

* `keepalived_install`: [default: `[]`]: Additional packages to install (e.g. `['libnl-3-dev', 'libnl-genl-3-dev', 'libnl-route-3-dev', 'libnfnetlink-dev']`)
* `keepalived_configure_options`: [default: `[]`]: Options to pass to `./configure`

* `keepalived_options`: [default: `[]`]: Options to pass to the `keepalived`
* `keepalived_options.{n}.name`: [required]: Option name (e.g. `log-facility`)
* `keepalived_options.{n}.value`: [optional]: Option value (e.g. `7`)

* `keepalived_ip_nonlocal_bind`: [default: `1`]: Allow to bind to IP addresses that are nonlocal, meaning that they're not assigned to a device on the local system

* `keepalived_global_defs_notification_email`: [default: `['root@localhost.localdomain']`]: Email addresses to send alerts to
* `keepalived_global_defs_notification_email_from`: [default: `'root@localhost.localdomain'`]: From address that will be in header
* `keepalived_global_defs_smtp_server`: [default: `'127.0.0.1'`]: SMTP server IP address
* `keepalived_global_defs_smtp_connect_timeout`: [default: `30`]: SMTP server connect timeout in seconds
* `keepalived_global_defs_script_user`: [optional]: Specify the default user / group to run scripts under (e.g. `'nobody nogroup'`). If group is not specified, the group of the user is used. If this option is not specified, the user defaults to `keepalived_script`. If that user exists, otherwise `root`
* `keepalived_global_defs_enable_script_security`: [optional]: Don't run scripts configured to be run as `root` if any part of the path is writable by a `non-root` user

* `keepalived_vrrp_script_map`: [default: `{}`]: Script declarations
* `keepalived_vrrp_script_map.key`: [required]: The identifier of the file (e.g. `check-haproxy`)
* `keepalived_vrrp_script_map.key.src`: [required]: The local path of the file to copy, can be absolute or relative (e.g. `../../../files/keepalived/usr/local/bin/check-haproxy`)
* `keepalived_vrrp_script_map.key.dest`: [required]: The remote path of the file to copy (e.g. `/usr/local/bin/check-haproxy`)
* `keepalived_vrrp_script_map.key.owner`: [optional, default `root`]: The name of the user that should own the file
* `keepalived_vrrp_script_map.key.group`: [optional, default `root`]:The name of the group that should own the file
* `keepalived_vrrp_script_map.key.mode`: [optional, default `0750`]: The mode of the file

* `keepalived_vrrp_scripts`: [default: `{}`]: VRRP script declarations
* `keepalived_vrrp_scripts.key`: The name of the VRRP script
* `keepalived_vrrp_scripts.key.script`: The script to run periodically
* `keepalived_vrrp_scripts.key.weight`: The check weight to adjust the priority (optional)
* `keepalived_vrrp_scripts.key.interval`: The check interval in seconds (optional)

* `keepalived_vrrp_instances`: [default: `{}`]: VRRP instance declarations
* `keepalived_vrrp_instances.key`: The name of the VRRP instance
* `keepalived_vrrp_instances.key.interface`: Interface bound by VRRP
* `keepalived_vrrp_instances.key.state`: Start-up default state (`MASTER|BACKUP`). As soon as the other machine(s) come up, an election will be held and the machine with the highest `priority` will become `MASTER`
* `keepalived_vrrp_instances.key.priority`: For electing `MASTER` highest priority (`0...255`) wins
* `keepalived_vrrp_instances.key.virtual_router_id`: Arbitrary unique number (`0...255`) used to differentiate multiple instances of VRRPD running on the same NIC (and hence same socket)
* `keepalived_vrrp_instances.key.advert_int`: The advert interval in seconds (optional)
* `keepalived_vrrp_instances.key.smtp_alert`: Whether or not to send email notifications during state transitioning (optional)
* `keepalived_vrrp_instances.key.authentication`: Authentication block
* `keepalived_vrrp_instances.key.authentication.auth_type`: Simple password or IPSEC AH (`PASS|AH`)
* `keepalived_vrrp_instances.key.authentication.auth_pass`: Password string (up to 8 characters)
* `keepalived_vrrp_instances.key.virtual_ipaddresses`: VRRP IP address block
* `keepalived_vrrp_instances.key.nopreempt`: VRRP will normally preempt a lower priority machine when a higher priority machine comes online. This option allows the lower priority machine to maintain the master role, even when a higher priority machine comes back online. **NOTE:** For this to work, the initial state of this entry must be `BACKUP`
* `keepalived_vrrp_instances.key.preempt_delay`: Seconds after startup until preemption (if not disabled by `nopreempt`). Range: 0 (default) to 1000 **NOTE:** For this to work, the initial state of this entry must be BACKUP
* `keepalived_vrrp_instances.key.track_scripts`: Scripts state we monitor

* `keepalived_vrrp_instances.key.notify`: Scripts that is invoked when a server changes state
* `keepalived_vrrp_instances.key.notify_backup`: Scripts that is invoked when a server changes state (to `BACKUP`)
* `keepalived_vrrp_instances.key.notify_fault`: Scripts that is invoked when a server changes state (to `FAULT`)
* `keepalived_vrrp_instances.key.notify_master`: Scripts that is invoked when a server changes state (to `MASTER`)

#### Dependencies

None

#### Example (HAProxy)

```yaml
---
- hosts: all
  roles:
    - keepalived
  vars:
    keepalived_options:
      - name: log-detail
    keepalived_vrrp_scripts:
      chk_haproxy:
        script: 'killall -0 haproxy'
        weight: 2
        interval: 1

    keepalived_vrrp_instances:
      VI_1:
        interface: eth1
        state: MASTER
        priority: 101
        virtual_router_id: 51

        authentication:
          auth_type: PASS
          auth_pass: '4Apr3C*d'

        virtual_ipaddresses:
          - '10.0.0.10/24 dev eth1 label eth1:1'

        track_scripts:
          - chk_haproxy
```

#### License

MIT

#### Author Information

Mischa ter Smitten

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Oefenweb/ansible-keepalived/issues)!
