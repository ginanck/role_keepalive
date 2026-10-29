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
