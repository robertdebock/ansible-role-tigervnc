# [tigervnc](#tigervnc)

Install and configure tigervnc on your system.

|Travis|GitHub|Quality|Downloads|Version|
|------|------|-------|---------|-------|
|[![travis](https://travis-ci.com/robertdebock/ansible-role-tigervnc.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-tigervnc)|[![github](https://github.com/robertdebock/ansible-role-tigervnc/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-tigervnc/actions)|[![quality](https://img.shields.io/ansible/quality/46981)](https://galaxy.ansible.com/robertdebock/tigervnc)|[![downloads](https://img.shields.io/ansible/role/d/46981)](https://galaxy.ansible.com/robertdebock/tigervnc)|[![Version](https://img.shields.io/github/release/robertdebock/ansible-role-tigervnc.svg)](https://github.com/robertdebock/ansible-role-tigervnc/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from `molecule/resources/converge.yml` and is tested on each push, pull request and release.
```yaml
---
- name: converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: robertdebock.tigervnc
```

The machine may need to be prepared using `molecule/resources/prepare.yml`:
```yaml
---
- name: prepare
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.core_dependencies
    - role: robertdebock.gnome
    - role: robertdebock.users
      users_group_list:
        - name: vncgroup
      users_user_list:
        - name: vncuser
          sudo_options: "ALL=(ALL) NOPASSWD: ALL"
          group: vncgroup
```

For verification `molecule/resources/verify.yml` run after the role has been applied.
```yaml
---
- name: Verify
  hosts: all
  become: yes
  gather_facts: yes

  tasks:
    - name: check if connection still works
      ping:

    - name: test port 5901
      wait_for:
        port: 5901

    - name: show testing instruction
      debug:
        msg: "Please run: vncviewer {{ ansible_default_ipv4.address|default(ansible_all_ipv4_addresses[0]) }}:5901"
```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

These variables are set in `defaults/main.yml`:
```yaml
---
# defaults file for tigervnc

# The tigervnc-server runs under a specific user and group. This user is
# created in `molecule/default/prepare.yml`. You can pick an existing
# user or create one using
# [ansible-role-users](https://galaxy.ansible.com/robertdebock/users)
tigervnc_username: vncuser
tigervnc_groupname: vncgroup

# Connecting to tigervnc-server required a password.
tigervnc_password: vncpass
# Use existing user's paswword
tigervnc_user_exists: no

# Desktop session xstartup should connect to e.g. gnome-session, mate-session
tigervnc_desktop_session: gnome-session
```

## [Requirements](#requirements)

- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the current, previous and next release of Ansible.)

The following roles can be installed to ensure all requirements are met, using `ansible-galaxy install -r requirements.yml`:

```yaml
---
- robertdebock.bootstrap
- robertdebock.core_dependencies
- robertdebock.gnome
- robertdebock.users

```

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/tigervnc.png "Dependency")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/robertdebock):

|container|tags|
|---------|----|
|el|7|
|debian|buster, bullseye|
|fedora|31, 32|

The minimum version of Ansible required is 2.8 but tests have been done to:

- The previous version, on version lower.
- The current version.
- The development version.



## [Testing](#testing)

[Unit tests](https://travis-ci.com/robertdebock/ansible-role-tigervnc) are done on every commit, pull request, release and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-tigervnc/issues)

Testing is done using [Tox](https://tox.readthedocs.io/en/latest/) and [Molecule](https://github.com/ansible/molecule):

[Tox](https://tox.readthedocs.io/en/latest/) tests multiple ansible versions.
[Molecule](https://github.com/ansible/molecule) tests multiple distributions.

To test using the defaults (any installed ansible version, namespace: `robertdebock`, image: `fedora`, tag: `latest`):

```
molecule test

# Or select a specific image:
image=ubuntu molecule test
# Or select a specific image and a specific tag:
image="debian" tag="stable" tox
```

Or you can test multiple versions of Ansible, and select images:
Tox allows multiple versions of Ansible to be tested. To run the default (namespace: `robertdebock`, image: `fedora`, tag: `latest`) tests:

```
tox

# To run CentOS (namespace: `robertdebock`, tag: `latest`)
image="centos" tox
# Or customize more:
image="debian" tag="stable" tox
```

## [License](#license)

Apache-2.0

## [Contributors](#contributors)

I'd like to thank everybody that made contributions to this repository. It motivates me, improves the code and is just fun to collaborate.

- [jellevandehaterd](https://github.com/jellevandehaterd)
- [aindenko](https://github.com/aindenko)

## [Author Information](#author-information)

[Robert de Bock](https://robertdebock.nl/)

Please consider [sponsoring me](https://github.com/sponsors/robertdebock).
