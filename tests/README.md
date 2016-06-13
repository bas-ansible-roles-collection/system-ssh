# System SSH (`system-ssh`) - Testing

## Overview

To ensure this role works correctly, tests **MUST** be written for any role changes. Roles must pass their tests before
new versions are released.

These tests, and their different configurations, aim to cover the most frequent, and not all, ways a role is used using
multiple environments.

* General information on how BARC roles are tested is available in the
[BARC Overview and Polices Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Testing)
* The structure and implementation of these tests is described in more detail in the
[Pristine - BARC Flavour documentation](https://paper.dropbox.com/doc/BAS-Base-Project-Pristine-BARC-Flavour-07j1BJt65gs2crIhMgWhw#:h=Role-testing)
* Details on the testing environments these tests are ran within are described in more detail in the [Pristine - BARC Flavour documentation](https://paper.dropbox.com/doc/BAS-Base-Project-Pristine-BARC-Flavour-07j1BJt65gs2crIhMgWhw#:h=Role-testing)

The *Testing* environment tests a single scenario:

* Prevents root and password based logins and enabled `fail2ban` to protect against brute forcing SSH

The *Local Testing* environment tests multiple scenarios:

1. `test-basic` - Prevents root and password based logins and enables `fail2ban` to protect against brute forcing SSH
2. `ban-test` - Used for functional tests to verify prohibited SSH settings are correctly applied and `fail2ban`
will block repeatedly invalid connections

**Note:** *Local Testing* environments test scenarios on all operating systems this role supports. Services providing
*Testing* environments may limit which operating system are available (e.g. SemaphoreCI only supports Ubuntu Trusty).

## Setup

To bring up a local testing environment:

1. Ensure you meet all the
[requirements](https://paper.dropbox.com/doc/BAS-Base-Project-Pristine-Base-Flavour-Usage-ZdMdHHzf8xB4HjxcNuDXa#:h=Environment---local-testing)
to bring up a local testing environment
2. `$ cd system-ssh/tests/provisioning/site-testing-local`
3. `$ vagrant up`
4. `$ cd ..`
5. `$ ansible-playbook site-testing-local.yml`

To bring up the testing environment:

1. Ensure you meet all the [requirements](https://paper.dropbox.com/doc/BAS-Base-Project-Pristine-Base-Flavour-Usage-ZdMdHHzf8xB4HjxcNuDXa#:h=Environment---testing)
to bring up a testing environment
2. Ensure you have performed all the [setup tasks](https://paper.dropbox.com/doc/BAS-Base-Project-Pristine-Base-Flavour-Usage-ZdMdHHzf8xB4HjxcNuDXa#:h=Environment---testing)
for the testing environment

Note: This role uses *SeamphoreCI* as its Continuous Integration service.

## Usage

To run tests using a local testing environment:

1. `$ cd system-ssh/tests/provisioning`
2. `$ ansible-playbook app-test-local.yml`

To run tests using a remote testing environment:

* No action is needed other than committing changes to the role repository, tests will then run automatically

Results for these tests can be found [here](https://semaphoreci.com/bas-ansible-roles-collection/system-ssh).
