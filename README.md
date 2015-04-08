## keepalived

[![Build Status](https://travis-ci.org/Oefenweb/ansible-keepalived.svg?branch=master)](https://travis-ci.org/Oefenweb/ansible-keepalived) [![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-keepalived-blue.svg)](https://galaxy.ansible.com/list#/roles/3349)

Set up [Keepalived](http://www.keepalived.org/) in Ubuntu systems.

#### Requirements

None

#### Variables

* `keepalived_ip_nonlocal_bind` [default: `1`]: Allow to bind to IP addresses that are nonlocal, meaning that they're not assigned to a device on the local system

* `keepalived_global_defs_notification_email` [default: `['root@localhost.localdomain']`]: Email addresses to send alerts to
* `keepalived_global_defs_notification_email_from` [default: `'root@localhost.localdomain'`]: From address that will be in header
* `keepalived_global_defs_smtp_server` [default: `'127.0.0.1'`]: SMTP server IP address
* `keepalived_global_defs_smtp_connect_timeout` [default: `30`]: SMTP server connect timeout in seconds

* `keepalived_vrrp_scripts` [default: `{}`]: VRRP script declarations
* `keepalived_vrrp_scripts.key`: The name of the VRRP script
* `keepalived_vrrp_scripts.key.script`: The script to run periodically
* `keepalived_vrrp_scripts.key.weight`: The check weight to adjust the priority (optional)
* `keepalived_vrrp_scripts.key.interval`: The check interval in seconds (optional)

* `keepalived_vrrp_instances` [default: `{}`]: VRRP instance declarations
* `keepalived_vrrp_instances.key`: The name of the VRRP instance
* `keepalived_vrrp_instances.key.interface`: Interface bound by VRRP
* `keepalived_vrrp_instances.key.state`: Start-up default state (`MASTER|BACKUP`). As soon as the other machine(s) come up, an election will be held and the machine with the highest `priority` will become `MASTER`
* `keepalived_vrrp_instances.key.priority`: For electing `MASTER` highest priority (`0...255`) wins
* `keepalived_vrrp_instances.key.virtual_router_id`: Arbitrary unique number (`0...255`) used to differentiate multiple instances of VRRPD running on the same NIC (and hence same socket)
* `keepalived_vrrp_instances.key.advert_int`: The advert interval in seconds
* `keepalived_vrrp_instances.key.smtp_alert`: Whether or not to send email notifications during state transitioning
* `keepalived_vrrp_instances.key.authentication`: Authentication block
* `keepalived_vrrp_instances.key.authentication.auth_type`: Simple password or IPSEC AH (`PASS|AH`)
* `keepalived_vrrp_instances.key.authentication.auth_pass`: Password string (up to 8 characters)
* `keepalived_vrrp_instances.key.virtual_ipaddresses`: VRRP IP address block
* `keepalived_vrrp_instances.key.track_scripts`: Scripts state we monitor

## Dependencies

None

#### Example

```yaml
---
- hosts: all
  roles:
  - keepalived
```

#### License

MIT

#### Author Information

Mischa ter Smitten

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Oefenweb/ansible-keepalived/issues)!
