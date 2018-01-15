# freebsdport

Ansible module that configures, installs, updates and deinstalls FreeBSD ports in canonical way.

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
