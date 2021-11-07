[![Actions Status - Master](https://github.com/juju4/ansible-sysmon/workflows/AnsibleCI/badge.svg)](https://github.com/juju4/ansible-sysmon/actions?query=branch%3Amaster)
[![Actions Status - Devel](https://github.com/juju4/ansible-sysmon/workflows/AnsibleCI/badge.svg?branch=devel)](https://github.com/juju4/ansible-sysmon/actions?query=branch%3Adevel)

# sysmon ansible role

Install and configure Sysmon for Linux

* https://github.com/Sysinternals/SysmonForLinux

## Requirements & Dependencies

### Ansible
It was tested on the following versions:
 * 4.3

### Operating systems

Tested on Ubuntu 18.04, 20.04, Centos 7 and 8.

## Example Playbook

Just include this role in your list.
For example

```
- host: myhost
  roles:
    - juju4.sysmon
```

## Variables

N/A

## Continuous integration

```
$ pip install molecule docker
$ molecule test
$ MOLECULE_DISTRO=ubuntu:20.04 molecule test --destroy=never
```


## Troubleshooting & Known issues

* build option requires [monodevelop](https://www.monodevelop.com/download/) which is only available officially up to Ubuntu 18.04 at Oct 2021 but as said in download page, bionic repository is functional on more recent releases.

## License

BSD 2-clause
