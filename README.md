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

* BPF module not loading with sysmon 1.0.2. From /var/log/sysmon/sysmon.log
```
Sep  1 17:12:35 HOST sysmon[62926]: 1628: (15) if r1 == 0x0 goto pc+6
Sep  1 17:12:35 HOST sysmon[62926]:  R0=inv(id=0) R1=inv(id=0) R2=inv1 R5=inv0 R6=inv(id=0,umax_value=4094,var_off=(0x0; 0xfff)) R7=inv(id=0,umax_value=4096,var_off=(0x0; 0x1fff)) R8=map_value(id=0,off=4096,ks=4,vs=8192,imm=0) R9=map_value(id=0,off=128,ks=4,vs=65512,imm=0) R10=fp0 fp-80=ctx fp-88=map_value fp-96=map_value fp-104=map_value fp-112=map_value fp-120=map_value
Sep  1 17:12:35 HOST sysmon[62926]: 1629: (79) r1 = *(u64 *)(r10 -128)
Sep  1 17:12:35 HOST sysmon[62926]: 1630: (a7) r1 ^= 4095
Sep  1 17:12:35 HOST sysmon[62926]: 1631: (79) r2 = *(u64 *)(r10 -88)
Sep  1 17:12:35 HOST sysmon[62926]: 1632: (0f) r2 += r1
Sep  1 17:12:35 HOST sysmon[62926]: math between map_value pointer and register with unbounded min value is not allowed
Sep  1 17:12:35 HOST sysmon[62926]: libbpf: -- END LOG --
Sep  1 17:12:35 HOST sysmon[62926]: libbpf: failed to load program 'sysmon/ProcCreate/exit'
Sep  1 17:12:35 HOST sysmon[62926]: libbpf: failed to load object './/sysmonEBPFkern4.15.o'
Sep  1 17:12:35 HOST sysmon[62926]: ERROR: failed to load prog: 'Invalid argument'
Sep  1 17:12:35 HOST sysmon[62926]: Telemetry failed to start: eBPF object could not be loaded
```
Known pending issue https://github.com/Sysinternals/SysmonForLinux/issues/65, https://github.com/Sysinternals/SysmonForLinux/issues/66
Temporary workaround is to downgrade to 1.0.0. You will need to pin version to avoid upgrading it.
```
apt install sysmonforlinux=1.0.0
systemctl restart sysmon
cat /etc/apt/preferences.d/sysmonforlinux
Package: sysmonforlinux
Pin: Version 1.0.0
Pin-Priority: 900
```

* Cannot allocate memory
```
Sep  1 18:16:41 HOST sysmon[58907]: Configuration file validated.
Sep  1 18:16:41 HOST sysmon[58907]: Found Kernel version: 4.15
Sep  1 18:16:41 HOST sysmon[58907]: Using EBPF object: .//sysmonEBPFkern4.15.o
Sep  1 18:16:47 HOST sysmon[58907]: Discovering offsets...done
Sep  1 18:16:47 HOST sysmon[58907]: libbpf: failed to mmap perf buffer on cpu #1: Cannot allocate memory
Sep  1 18:16:47 HOST sysmon[58907]: ERROR: failed to setup perf_buffer: -12
Sep  1 18:16:47 HOST sysmon[58907]: Telemetry failed to start: perf ring buffer could not be started
```
Restarting daemon seems to solve issue sometimes.


## License

BSD 2-clause
