Role Name
=========

A brief description of the role goes here.

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).

## Security and behaviour

- `keepalive_auth_pass` has no default. Supply it from the vault; the role asserts it before
  rendering and marks the configuration tasks `no_log`.
- Instances must use `auth_type: PASS` (or `AH`); any other value fails the pre-flight check.
- The check script uses a negative weight (`-20` by default), so a failing check lowers the
  node priority and the virtual address moves to a healthy peer.
- The configuration file is installed with mode `0640`; the service is enabled and started, so
  it survives a reboot.
- Debian and RedHat families are supported through the generic package module; the pre-flight
  check rejects other families with a message that names what was detected.

## Operational options

- `keepalive__global_defs` sets the router id, script security and optional SMTP alerts.
- `keepalive__advert_int`, per-instance `advert_int`, `preempt`, `use_vmac`, `track_interface`,
  `keepalive__track_interface` and `keepalive__sync_group` control timing and tracking.
- `keepalive__notify_scripts` maps `master`, `backup` and `fault` to hook scripts.
- `keepalive__scripts` distributes health check scripts to `/etc/keepalived` with mode `0750`.
- `keepalive__manage_sysctl` persists `net.ipv4.ip_nonlocal_bind`;
  `keepalive__manage_firewall` allows VRRP through firewalld or ufw.
- `keepalive__validate_config` runs `keepalived -t` on a staged file before it replaces the live
  configuration.
- `keepalive__virtual_servers` renders LVS `virtual_server` blocks.

## Molecule scenarios

- `default` and `rockylinux-9` run on the Rocky Linux 9 image; `rockylinux-8` targets Rocky Linux 8.
- Run them with the mitogen strategy disabled, otherwise Ansible hangs against the local Docker socket:

  ```sh
  env -u ANSIBLE_STRATEGY -u ANSIBLE_STRATEGY_PLUGINS molecule test
  env -u ANSIBLE_STRATEGY -u ANSIBLE_STRATEGY_PLUGINS molecule test -s rockylinux-8
  ```

- The Rocky Linux 8 image ships Python 3.6 only; ansible-core 2.21 cannot execute modules on a
  3.6 target, so the `rockylinux-8` scenario needs an image with Python 3.8 or newer.
- The role dependencies are taken from the checked-out `../role_base` sibling, so local changes to
  the base role are exercised by the scenarios.
