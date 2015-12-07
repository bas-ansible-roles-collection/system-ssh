# System SSH (`system-ssh`) - Change log

All notable changes to this role will be documented in this file.
This role adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html).

## [Unreleased][unreleased]

### Added

* Missing 'BARC_INSTALL' tag

### Fixed

* Change log formatting
### Changed

* Installing the EPEL repository to install Fail2Ban on CentOS is no longer tagged as 'CONFIGURE_SYSTEM' in line with
BARC policy

## 0.1.0 - 30/11/2015

### Added

* Initial version - with support for setting selected options in SSH Daemon configuration file and protecting SSH 
logins with Fail2Ban
