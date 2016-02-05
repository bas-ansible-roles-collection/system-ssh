# System SSH (`system-ssh`)

Master:
[![Build Status](https://semaphoreci.com/api/v1/bas-ansible-roles-collection/system-ssh/branches/master/badge.svg)](https://semaphoreci.com/bas-ansible-roles-collection/system-ssh)

Develop:
[![Build Status](https://semaphoreci.com/api/v1/bas-ansible-roles-collection/system-ssh/branches/develop/badge.svg)](https://semaphoreci.com/bas-ansible-roles-collection/system-develop)

Configures SSH daemon settings and fail2ban protection for a machine

**Part of the BAS Ansible Role Collection (BARC)**

## Overview

* Optionally, configures SSH to disallow root logins, this option is *enabled* by default
* Optionally, configures SSH to disallow password based logins, this option is *enabled* by default
* Optionally, installs and configures the `fail2ban` utility to protect SSH logins, this option is *enabled* by default

## Quality Assurance

This role uses manual and automated testing to ensure the features offered by this role work as advertised. 
See `tests/README.md` for more information.

## Dependencies

* [**bas-ansible-roles-collection.system-firewall**](https://galaxy.ansible.com/bas-ansible-roles-collection/system-firewall/)
  * Minimum version: *0.2.0*

### Pinning dependencies

All dependencies of this role will use the latest role version. In many cases this will be unsuitable and so versions
should be 'pinned' to a specific version to ensure breaking changes for example are introduced. This issue becomes
increasingly important the longer a project is not updated.

A method developed for BAS projects uses a `roles.yml` file to specify all roles, including dependencies of roles, 
and their version. This does require you to resolve dependencies for roles manually, however dependencies are listed in
each role's `meta/main.yml` and `README.md` file.

Note: BAS projects **SHOULD** always pin role versions, except in exceptional circumstances.

### Avoiding dependencies

All dependencies of this role are tagged as *BARC_ROLE_DEPENDENCY*. Skipping this tag when executing a playbook that
includes this role will prevent role dependencies from being installed.

E.g.

```shell
$ ansible-playbook playbook.yml --skip-tags "BARC_ROLE_DEPENDENCY"
```

Note: This 'dependency' tag is shared across all BARC roles, and skipping it will therefore exclude all dependencies,
from all BARC roles. There is currently no supported method to exclude only this roles dependencies.

Note: Using this role without its dependencies is **NOT** supported, and may lead to incomplete results or errors.

## Requirements

* None

## Limitations

* The SSH daemon cannot be restarted using the service module on Ubuntu

Separate handlers are needed because of a bug in Ansible where the _service_ module cannot be used to restart SSH on 
Ubuntu.

Once this is fixed changing this role is relatively trivial and will remove the need for separate handlers and tasks.

*This limitation is **NOT** considered to be significant. A workaround is in place mitigate this limitation, pending a*
*permanent resolution by Ansible. Pull requests addressing this limitation will be considered.*

See [#1298](https://github.com/ansible/ansible-modules-core/issues/1298) for further information of this upstream bug.

See [BARC-85](https://jira.ceh.ac.uk/browse/BARC-85) for further information of this limitation in this role.

## Usage

### BARC manifest

By default, BARC roles will record that they have been applied to a system. This is recorded using a set of 
[Ansible local facts](http://docs.ansible.com/ansible/playbooks_variables.html#local-facts-facts-d), specifically:

* `ansible_local.barc-system-ssh.general.role_applied` - to indicate that this role has been applied
* `ansible_local.barc-system-ssh.general.role_version` - to indicate the applied version of this role

Note: You **SHOULD** use this feature to determine whether this role has been applied to a system.

If you do not want these facts to be set by this role, you **MUST** skip the **BARC_SET_MANIFEST** tag. No support is 
offered in this case, as other roles or use-cases may rely on this feature. Therefore you **SHOULD NOT** disable this
feature.

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

- name: configure SSH
  hosts: all
  become: yes
  vars: []
  roles:
    - bas-ansible-roles-collection.system-ssh
```

### Tags

BARC roles use standardised tags to control which aspects of an environment are changed by roles. Where relevant, tags
will be applied at a role, or task(s) level, as indicated below.

This role uses the following tags, for various tasks:

* [**BARC_CONFIGURE**](https://antarctica.hackpad.com/BARC-Standardised-Tags-AviQxxiBa3y#:h=BARC_CONFIGURE)
* [**BARC_CONFIGURE_SECURITY**](https://antarctica.hackpad.com/BARC-Standardised-Tags-AviQxxiBa3y#:h=BARC_CONFIGURE_SECURITY)
* [**BARC_CONFIGURE_PACKAGES**](https://antarctica.hackpad.com/BARC-Standardised-Tags-AviQxxiBa3y#:h=BARC_CONFIGURE_PACKAGE)
* [**BARC_INSTALL**](https://antarctica.hackpad.com/BARC-Standardised-Tags-AviQxxiBa3y#:h=BARC_INSTALL)
* [**BARC_INSTALL_PACKAGES**](https://antarctica.hackpad.com/BARC-Standardised-Tags-AviQxxiBa3y#:h=BARC_INSTALL_PACKAGE)
* [**BARC_SET_MANIFEST**](https://antarctica.hackpad.com/BARC-Standardised-Tags-AviQxxiBa3y#:h=BARC_SET_MANIFEST)

### Variables

#### *BARC_role_name*

* **MUST NOT** be specified
* Specifies the name of this role within the BAS Ansible Roles Collection (BARC) used for setting local facts
* See the *BARC roles manifest* section for more information
* Example: system-ssh

#### *BARC_role_version*

* **MUST NOT** be specified
* Specifies the name of this role within the BAS Ansible Roles Collection (BARC) used for setting local facts
* See the *BARC roles manifest* section for more information
* Example: 2.0.0

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

### Source code

All changes should be committed, via pull request, to the canonical repository, which for this project is:

`ssh://git@stash.ceh.ac.uk:7999/barc/system-ssh.git`

A mirror of this repository is maintained on GitHub. Changes are automatically pushed from the canonical repository to
this mirror, in a one-way process.

`git@github.com:bas-ansible-roles-collection/system-ssh.git`

Note: The canonical repository is only accessible within the NERC firewall. External collaborators, please make pull 
requests against the mirrored GitHub repository and these will be merged as appropriate.

### Contributing policy

This project welcomes contributions, see `CONTRIBUTING.md` for our general policy.

The [Git flow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow/) 
workflow is used to manage the development of this project:

* Discrete changes should be made within feature branches, created from and merged back into develop 
(where small changes may be made directly)
* When ready to release a set of features/changes, create a release branch from develop, update documentation as 
required and merge into master with a tagged, semantic version (e.g. v1.2.3)
* After each release, the master branch should be merged with develop to restart the process
* High impact bugs can be addressed in hotfix branches, created from and merged into master (then develop) directly

### Release procedure

See [here](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Release-procedures) for general 
release procedures for BARC roles.

## Licence

Copyright 2015 NERC BAS.

Unless stated otherwise, all documentation is licensed under the Open Government License - version 3. All code is
licensed under the MIT license.

Copies of these licenses are included within this role.
