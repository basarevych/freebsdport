# freebsdport

Ansible module that configures, installs, updates and deinstalls FreeBSD ports in a canonical way.

# Installation

Create **library** directory at the same level as your **roles** directory. Copy **freebsdport.py** to **library/**

# Details

Look inside [freebsdport.py](freebsdport.py) for more information.

# Examples

```
# Set all Ansible port options
- freebsdport:
    name: sysutils/ansible
    options: DOCS=off EXAMPLES=on NETADDR=on
    state: configured
# Enable DOCS and NETADDR, set EXAMPLES to default value and install Ansible
- freebsdport:
    name: sysutils/ansible
    enable: DOCS NETADDR
    reset: EXAMPLES
    state: present
# Update Ansible with dependencies after refreshing the ports tree
- freebsdport:
    name: sysutils/ansible
    state: latest
    refresh_tree: yes
# Reinstall all of the ports (or use the name parameter)
- freebsdport:
    state: reinstalled
# Deinstall Ansible port
- freebsdport:
    name: sysutils/ansible
    state: absent
```

# Description

### Notes:
* This module might install (automatically) or refresh (when requested)
  the port tree using **portsnap** utility.
* With addition to **make**, **pkg** utility is used which might be
  automatically installed if not present on target host.
* Port options are stored in **/etc/make.ports.conf** file on target host.
  Instructions to include this file are automatically added to
  **/etc/make.conf** file.
* Do not run **make config** manually on target hosts as this configuration
  will be removed on next reinstall.
* If port fails to build you can find build log in /var/log/freebsdport
  directory.

### Options:
```
  name:
    description:
      - Port name with category, e.g. C(sysutils/ansible) to work with
        Ansible port on the remote host.
      - Required for states C(configured), C(present) and C(absent). Ignored
        when no C(state) provided.
    required: false
    default: null
    
  state:
    description:
      - Whether to install (C(present) or C(latest)), force reinstall
        (C(reinstalled)), remove (C(absent)), or just modify options
        (C(configured)).
    choices: [ configured, present, latest, reinstalled, absent ]
    required: false
    default: null
    
  options:
    description:
      - Space separated list of all the port's options you wish to define.
        Each item is in the form of OPTION_NAME=state, where state is either
        C(on) or C(off), e.g. options="FOO=on BAR=on BAZ=off".
      - If this parameter is specified, options not listed will be reset to
        port's default values. Use parameters C(enable), C(disable) or C(reset)
        instead if you only want to modify some specific options without
        resetting all the others.
      - If C(state) is C(present) or C(latest) and new options differ from
        the ones saved the port will be reinstalled with the new options.
        Ignored when no C(name) provided.
    required: false
    default: null
    
  enable:
    description:
      - Space separated list of option names that will be enabled,
        e.g. enable="FOO BAR".
      - This parameter will only change options specified and leave all the
        other options unmodified.
      - If C(state) is C(present) or C(latest) and new options differ from
        saved ones the port will be reinstalled with the new options.
        Ignored when no C(name) provided.
    required: false
    default: null
    
  disable:
    description:
      - Space separated list of options names that will be disabled,
        e.g. disable="FOO BAR".
      - This parameter will only change options specified and leave all the
        other options unmodified.
      - If C(state) is C(present) or C(latest) and new options differ from
        saved ones the port will be reinstalled with the new options.
        Ignored when no C(name) provided.
    required: false
    default: null
    
  reset:
    description:
      - Space separated list of options names that will be reset to their
        default, e.g. reset="FOO BAR".
      - This parameter will only change options specified and leave all the
        other options unmodified.
      - If C(state) is C(present) or C(latest) and new options differ from
        saved ones the port will be reinstalled with the new options.
        Ignored when no C(name) provided.
    required: false
    default: null
    
  refresh_tree:
    description:
      - Refresh ports tree using C(portsnap) before doing anything else.
    required: false
    default: false
    
  cron:
    description:
      - Whether to use C(cron) or C(fetch) command of C(portsnap) when
        upgrading ports tree. Used with C(refresh_tree) is enabled.
    required: false
    default: false
    
  include_deps:
    description:
      - When C(state) is C(latest) or C(reinstalled) and C(name) is given this
        option will also upgrade/reinstall dependencies of given port.
    required: false
    default: true
    
  ignore_vulnerabilities:
    description:
      - Install port even if it has known vulnerabilities.
    required: false
    default: false
    
  ports_dir:
    description:
      - Ports directory on target host
    required: false
    default: /usr/ports
    
  conf_file:
    description:
      - Port options file on target host
    required: false
    default: /etc/make.ports.conf
```
