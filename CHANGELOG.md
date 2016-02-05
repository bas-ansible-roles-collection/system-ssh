# System SSH (`system-ssh`) - Change log

All notable changes to this role will be documented in this file.
This role adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html).

Note: Developers - make sure to set the `BARC_role_version` variable when releasing new versions of this role.

## [Unreleased][unreleased]

### Added

* Missing 'BARC_INSTALL' tag
* Local facts to record this role has been applied to a system and its version, plus supporting documentation sections

### Fixed

* Change log formatting
* README formatting and typos
* Minor corrections to other files for formatting and typos
* Testing role dependencies should always use latest versions

### Changed

* Installing the EPEL repository to install Fail2Ban on CentOS is no longer tagged as 'CONFIGURE_SYSTEM' in line with
BARC policy

## 0.1.0 - 30/11/2015

### Added

* Initial version - with support for setting selected options in SSH Daemon configuration file and protecting SSH 
logins with Fail2Ban
