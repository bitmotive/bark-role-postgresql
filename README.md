BARK: POSTGRESQL
=========

Install PostgreSQL with sane defaults.

Requirements
------------

All roles in the Bitmotive Ansible Role Kit are configuration driven.

Customize this role by providing environment variables in either your
shell environment, host specific in group_vars/, or at the CLI when
prompted at runtime. 

Role Variables
--------------

**POSTGRESQL_PORT**:

The port PostgreSQL should bind upon when launched.

**POSTGRESQL_PUBLIC_INTERFACE**:

Set to "YES" for PostgreSQL to listen on public network interfaces.

**POSTGRESQL_ROOT_PASSWORD**:

The root password for the PostgreSQL installation.

**POSTGRESQL_DEFAULT_DB**:

If set, a default database will be created with this name.

**POSTGRESQL_DEFAULT_USER**:

If set, a default user will be created and able to access
PostgreSQL on any interface.

**POSTGRESQL_DEFAULT_USER_PASSWORD**:

Set the password for the default PostgreSQL user.

Dependencies
------------

Currently dependent on Debian/Ubuntu due to reliance on `apt` and filepath 
assumptions. 

In addition, if you connect to a remote host as a user other than root and 
attempt to use `become: yes` along with `become_user: postgres` you are
likely to encounter known issues with Ansible's privilege escalation
in regard to temp files:

https://docs.ansible.com/ansible-core/2.15/playbook_guide/playbooks_privilege_escalation.html#risks-of-becoming-an-unprivileged-user

One solution for this is to place lines such as the following in your ansible.cfg: 
```
remote_tmp = /tmp/ansible-remote
local_tmp = /tmp/ansible-local
allow_world_readable_tmpfiles = True
remote_tmp_dir_mode = 0777
```

In addition, the following included task may be needed to set the permissions for the temp directory: 

```
- name: Ensure ansible-remote directory exists with correct permissions
  file:
    path: /tmp/ansible-remote
    state: directory
    mode: '0777'
  become: yes 
```

If the above task runs, be sure to also remove it afterward:

```
- name: Remove ansible-remote directory
  file:
    path: /tmp/ansible-remote
    state: absent
  become: yes
```


License
-------

MIT

Author Information
------------------

A Bitmotive Project: Build. Incubate. Train.
https://bitmotive.com 
