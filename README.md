## keepalived

[![CI](https://github.com/Oefenweb/ansible-haproxy/workflows/CI/badge.svg)](https://github.com/Oefenweb/ansible-keepalived/actions?query=workflow%3ACI)
[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-keepalived-blue.svg)](https://galaxy.ansible.com/Oefenweb/keepalived)

Set up the latest or a specific version of [Keepalived](http://www.keepalived.org/) in Debian-like systems.

#### Requirements

* `git` (will be installed)
* `build-essential` (will be installed)
* `automake` (will be installed)
* `libtool` (will be installed)
* `pkg-config` (will be installed)
* `libssl-dev` (will be installed)

when using `keepalived_install_method: native`

* `keepalived` (will be installed)
* `procps` (will be installed)

#### Variables

* `keepalived_git_repo`: [default: `https://github.com/acassen/keepalived.git`]: Keepalived git repo
* `keepalived_version`: [default: `v2.2.2`]: Keepalived version to install

* `keepalived_install_method`: [default `git`]: The way to install Keepalived (e.g. `native` (from Ubuntu repo), `git`)
* `keepalived_install`: [default: `[]`]: Additional packages to install (e.g. `['libnl-3-dev', 'libnl-genl-3-dev', 'libnl-route-3-dev', 'libnfnetlink-dev']`)
* `keepalived_configure_options`: [default: `[]`]: Options to pass to `./configure`

* `keepalived_options`: [default: `[]`]: Options to pass to the `keepalived`
* `keepalived_options.{n}.name`: [required]: Option name (e.g. `log-facility`)
* `keepalived_options.{n}.value`: [optional]: Option value (e.g. `7`)

* `keepalived_ip_nonlocal_bind`: [default: `1`]: Allow to bind to IP addresses that are nonlocal, meaning that they're not assigned to a device on the local system

* `keepalived_create_keepalived_script_user`: [default: `false`]: Whether or not to create the `keepalived_script` user, see `keepalived_global_defs_script_user`

* `keepalived_global_defs_notification_email`: [default: `['root@localhost.localdomain']`]: Email addresses to send alerts to
* `keepalived_global_defs_notification_email_from`: [default: `'root@localhost.localdomain'`]: From address that will be in header
* `keepalived_global_defs_smtp_server`: [default: `'127.0.0.1'`]: SMTP server IP address
* `keepalived_global_defs_smtp_connect_timeout`: [default: `30`]: SMTP server connect timeout in seconds
* `keepalived_global_defs_script_user`: [optional]: Specify the default user / group to run scripts under. If group is not specified, the group of the user is used. If this option is not specified, the user defaults to `keepalived_script`. If that user exists, otherwise `root` (since `1.3.0`, e.g. `'nobody nogroup'`, )
* `keepalived_global_defs_enable_script_security`: [optional]: Don't run scripts configured to be run as `root` if any part of the path is writable by a `non-root` user (since `1.3.0`, e.g. `true`)
* `keepalived_global_defs_raw`: [optional]: An optional list of raw parameters to add to the `global_defs` config section

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
* `keepalived_vrrp_scripts.key.weight`: [optional]: The check weight to adjust the priority
* `keepalived_vrrp_scripts.key.interval`: [optional]: The check interval in seconds
* `keepalived_vrrp_scripts.key.timeout`: [optional]: Seconds after which script is considered to have failed
* `keepalived_vrrp_scripts.key.rise`: [optional]: Required number of successes for OK transition
* `keepalived_vrrp_scripts.key.fall`: [optional]: Required number of successes for KO transition

* `keepalived_vrrp_track_processes`: [default: `{}`]: VRRP process declarations
* `keepalived_vrrp_track_processes.key`: The name of the VRRP script
* `keepalived_vrrp_track_processes.key.process`: The process to monitor (with optional parameters)
* `keepalived_vrrp_track_processes.key.param_match`: [optional]: If matching parameters, this specifies a `partial` match (i.e. the first n parameters match exactly), or an `initial` match, i.e. the last parameter may be longer that the parameter configured
* `keepalived_vrrp_track_processes.key.weight`: [optional]: The check weight to adjust the priority
* `keepalived_vrrp_track_processes.key.quorum`: [optional]: Minimum number of processes for success
* `keepalived_vrrp_track_processes.key.quorum_max`: [optional]: Maximum number of processes for success
* `keepalived_vrrp_track_processes.key.fork_delay`: [optional]: Time to delay after process quorum gained after fork before consider process up
* `keepalived_vrrp_track_processes.key.terminate_delay`: [optional]: Time to delay after process quorum lost before consider process down
* `keepalived_vrrp_track_processes.key.delay`: [optional]: This sets `fork_delay` and `terminate_delay`
* `keepalived_vrrp_track_processes.key.full_command`: [optional]: Normally process string is matched against the process name, as shown on the `Name`: line in `/proc/PID/status`, unless parameters are specified

* `keepalived_vrrp_instances`: [default: `{}`]: VRRP instance declarations
* `keepalived_vrrp_instances.key`: The name of the VRRP instance
* `keepalived_vrrp_instances.key.interface`: Interface bound by VRRP
* `keepalived_vrrp_instances.key.state`: Start-up default state (`MASTER|BACKUP`). As soon as the other machine(s) come up, an election will be held and the machine with the highest `priority` will become `MASTER`
* `keepalived_vrrp_instances.key.priority`: For electing `MASTER` highest priority (`0...255`) wins
* `keepalived_vrrp_instances.key.virtual_router_id`: Arbitrary unique number (`0...255`) used to differentiate multiple instances of VRRPD running on the same NIC (and hence same socket)
* `keepalived_vrrp_instances.key.advert_int`: [optional]: The advert interval in seconds
* `keepalived_vrrp_instances.key.smtp_alert`: [optional]: Whether or not to send email notifications during state transitioning-
* `keepalived_vrrp_instances.key.authentication`: [optional]: Authentication block
* `keepalived_vrrp_instances.key.authentication.auth_type`: Simple password or IPSEC AH (`PASS|AH`)
* `keepalived_vrrp_instances.key.authentication.auth_pass`: Password string (up to 8 characters)
* `keepalived_vrrp_instances.key.virtual_ipaddresses`: VRRP IP address block
* `keepalived_vrrp_instances.key.virtual_ipaddresses_excluded`: IP address block, which is not included in the VRRP packet itself, in order to support more than 20 ips
* `keepalived_vrrp_instances.key.virtual_routes:`: [optional]: List of virtual route added when the VIP is added
* `keepalived_vrrp_instances.key.nopreempt`: [optional]: VRRP will normally preempt a lower priority machine when a higher priority machine comes online. This option allows the lower priority machine to maintain the master role, even when a higher priority machine comes back online. **NOTE:** For this to work, the initial state of this entry must be `BACKUP`
* `keepalived_vrrp_instances.key.preempt_delay`: [optional]: Seconds after startup until preemption (if not disabled by `nopreempt`). Range: 0 (default) to 1000 **NOTE:** For this to work, the initial state of this entry must be BACKUP
* `keepalived_vrrp_instances.key.track_interfaces`: [optional]: Interface states we monitor
* `keepalived_vrrp_instances.key.track_scripts`: [optional]: Scripts state we monitor
* `keepalived_vrrp_instances.key.track_processes`: [optional]: Processes state we monitor

* `keepalived_vrrp_instances.key.notify`: [optional]: Scripts that is invoked when a server changes state
* `keepalived_vrrp_instances.key.notify_user`: [optional]: Specify the user / group to run this script under (since `1.3.0`, e.g. `'nobody nogroup'`)
* `keepalived_vrrp_instances.key.notify_backup`: [optional]: Scripts that is invoked when a server changes state (to `BACKUP`)
* `keepalived_vrrp_instances.key.notify_backup_user`: [optional]: Specify the user / group to run this script under (since `1.3.0`)
* `keepalived_vrrp_instances.key.notify_fault`: [optional]: Scripts that is invoked when a server changes state (to `FAULT`)
* `keepalived_vrrp_instances.key.notify_fault_user`: [optional]: Specify the user / group to run this script under (since `1.3.0`)
* `keepalived_vrrp_instances.key.notify_master`: [optional]: Scripts that is invoked when a server changes state (to `MASTER`)
* `keepalived_vrrp_instances.key.notify_master_user`: [optional]: Specify the user / group to run this script under (since `1.3.0`)
* `keepalived_vrrp_instances.key.unicast_peer`: [optional]: List of IP addresses if you want to override participating unicast peers (if you don't want to use multicast)
* `keepalived_vrrp_instances.key.unicast_src_ip`: [optional]: IP source address to use for multicast or unicast vrrp packets
* `keepalived_vrrp_instances.key.vmac_xmit_base`: [default: `false`]: Forces VRRP to use the physical interface MAC address as source when it sends its own packets.
* `keepalived_vrrp_instances.key.raw_options`: [optional]: An optional list of raw parameters to add to the vrrp instance

* `keepalived_vrrp_sync_groups`: [default: `{}`]: VRRP group instance declarations
* `keepalived_vrrp_sync_groups.key`: The name of the VRRP group instance
* `keepalived_vrrp_sync_groups.key.instances:`: List of vrrp instances for this group
* `keepalived_vrrp_sync_groups.notify_master`: [optional]: Scripts that is invoked when VRRP group changes state (to `MASTER`)
* `keepalived_vrrp_sync_groups.key.notify_backup`: [optional]: Scripts that is invoked when VRRP group changes state (to `BACKUP`)
* `keepalived_vrrp_sync_groups.key.notify_fault`: [optional]: Scripts that is invoked when VRRP group changes state (to `FAULT`)

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
        script: '/usr/bin/pgrep haproxy'
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

        track_interfaces:
          - eth0 weight 10

        track_scripts:
          - chk_haproxy

        raw_options:
          - dont_track_primary
```

#### License

MIT

#### Author Information

Mischa ter Smitten

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Oefenweb/ansible-keepalived/issues)!
