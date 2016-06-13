# System SSH (`system-ssh`)

Master: [![Build Status](https://semaphoreci.com/api/v1/bas-ansible-roles-collection/system-ssh/branches/master/badge.svg)](https://semaphoreci.com/bas-ansible-roles-collection/system-ssh)
Develop: [![Build Status](https://semaphoreci.com/api/v1/bas-ansible-roles-collection/system-ssh/branches/develop/badge.svg)](https://semaphoreci.com/bas-ansible-roles-collection/system-ssh)

Configures SSH daemon settings and fail2ban protection for a machine.

**Part of the BAS Ansible Role Collection (BARC)**

**This role uses version 0.3.1 of the BARC flavour of the BAS Base Project - Pristine**.

## Overview

* Optionally, configures SSH to disallow root logins, this option is *enabled* by default
* Optionally, configures SSH to disallow password based logins, this option is *enabled* by default
* Optionally, installs and configures the `fail2ban` utility to protect SSH logins, this option is *enabled* by default

## Quality Assurance

This role uses manual and automated testing to ensure its features work as advertised.
See [here](tests/README.md) for more information.

## Ansible compatibility

* this role supports Ansible 1.8 or higher in the 1.x series
* this role supports Ansible 2.x

**Note:** Support for Ansible 1.x is deprecated by this role, future versions will support Ansible 2.x only.

More information on Ansible compatibility is available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Ansible-compatbility).

## Dependencies

* [**bas-ansible-roles-collection.system-firewall**](https://galaxy.ansible.com/bas-ansible-roles-collection/system-firewall/)
  * Minimum version: *0.2.0*

More information on role dependencies is available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Role-dependencies)

**Note:** Using this role without its dependencies is **NOT** supported, and may lead to incomplete results or errors.

## Requirements

* none

More information on role requirements is available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Role-requirements)

## Limitations

* The SSH daemon cannot be restarted using the service module on Ubuntu

Separate handlers are needed because of a bug in Ansible where the _service_ module cannot be used to restart SSH on 
Ubuntu.

Once this is fixed changing this role is relatively trivial and will remove the need for separate handlers and tasks.

*This limitation is **NOT** considered to be significant. A workaround is in place mitigate this limitation, pending a*
*permanent resolution by Ansible. Pull requests addressing this limitation will be considered.*

See [#1298](https://github.com/ansible/ansible-modules-core/issues/1298) for further information of this upstream bug.

See [BARC-85](https://jira.ceh.ac.uk/browse/BARC-85) for further information of this limitation in this role.

More information on role limitations is available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Role-limitations)

## Usage

### BARC Manifest

By default, BARC roles will record that they have been applied to a system, this is termed the BAS Manifest.
The specific local facts set for this role are:

* `ansible_local.barc_system_ssh.general.role_applied` - (boolean) records whether a role has been applied
* `ansible_local.barc_system_ssh.general.role_version` - (string) records the version of the role that was applied

**Note:** You **SHOULD** use this feature to determine whether this role has been applied to a system.
If you do not want these facts to be set by this role, you **MUST** skip the **BARC_SET_MANIFEST** tag.

More information is available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Role-Manifest)

### SSH Daemon configuration

The SSH Daemon is the *server* process for SSH which listens for and handles SSH connections, as opposed to the 
*client* process used by end users to connect to a host running the *server* process.

This Daemon supports a range of options, some of which control allowed authentication methods and users/groups. This 
role supports configuring a limited subset of this options, with an aim to improving security when using SSH by 
limiting privileges and reducing the ability to 'brute force' an SSH login.

### Fail2Ban

In addition to configuring options in the SSH Daemon, the `fail2ban` utility can be used to monitor SSH logins and 
where multiple failed logins are found from the same source, trigger a temporary firewall rule to block the source.

The effect of this process reduces the ability to 'brute force' an SSH login.

The `fail2ban` utility can be used to perform other actions for other applications (such as web-servers) which can be
configured outside of this role.

### Typical playbook

```yaml
---

- name: ...
  hosts: all
  become: yes
  vars: []
  roles:
    - bas-ansible-roles-collection.system-ssh
```

### Tags

BARC roles use standardised tags to control which aspects of an environment are changed by roles.
Tasks in this role will 'tag' themselves with these tags as appropriate, typically in `tasks/main.yml`.

More information is available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Appendix-B---BARC-Standardised)

### Variables

#### *system_ssh_barc_role_name*

* **MUST NOT** be specified
* specifies the name of this role within the BAS Ansible Roles Collection (BARC) used for setting local facts
* see the *BARC roles manifest* section for more information
* example: `system_ssh`

#### *system_ssh_barc_role_version*

* **MUST NOT** be specified
* specifies the name of this role within the BAS Ansible Roles Collection (BARC) used for setting local facts
* see the *BARC roles manifest* section for more information
* example: `2.0.0`

#### *system_ssh_config_permit_root_login*

* **MAY** be specified
* Specifies whether the root user can login using SSH
* Values **MUST** use one of these options, as determined by the `SSH` Daemon configuration file:
  * `yes`
  * `no`
* Values **MUST** be quoted to prevent Ansible coercing values to True/False which is invalid for this variable
* Where not specified, a value of `no` will be assumed
* Default: `no`

#### *system_ssh_config_permit_password_login*

* **MAY** be specified
* Specifies whether users can login using password authentication, as opposed to private/public key or other types
* Values **MUST** use one of these options, as determined by the `SSH` Daemon configuration file:
  * `yes`
  * `no`
* Values **MUST** be quoted to prevent Ansible coercing values to True/False which is invalid for this variable
* Where not specified, a value of `no` will be assumed
* Default: `no`

#### *system_ssh_use_fail2ban*

* **MAY** be specified
* Specifies whether `fail2ban` should be installed and configured to protect against brute forced SSH logins
* This variable is used as a 'feature flag' for whether tasks related to `fail2ban` will be applied
* Values **MUST** use one of these options, as determined by Ansible:
  * `true`
  * `false`
* Values **SHOULD NOT** be quoted to prevent Ansible coercing values to a string
* Where not specified, a value of `true` will be assumed
* Default: `true`

## Developing

### Issue tracking

Issues, bugs, improvements, questions, suggestions and other tasks related to this package are managed through the
[BAS Ansible Roles Collection](https://jira.ceh.ac.uk/projects/BARC) (BARC) project on Jira.

This service is currently only available to BAS or NERC staff, although external collaborators can be added on request.
See our contributing policy for more information.

More information is also available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Issue-Tracking)

### Source code

All changes should be committed, via pull request, to the canonical repository:

`ssh://git@stash.ceh.ac.uk:7999/barc/system-ssh.git`

A read-only mirror of this repository is maintained on GitHub:

`git@github.com:bas-ansible-roles-collection/system-ssh.git`

More information is available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Source-Code)

### Contributing policy

This role welcomes contributions, see `CONTRIBUTING.md` for our general policy.

### Release procedure

The general release procedure for BARC roles is available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Release-procedures)

## Licence

Copyright 2016 NERC BAS.

Unless stated otherwise, all documentation is licensed under the Open Government License - version 3.
All code is licensed under the MIT license.

Copies of these licenses are included within this role.
