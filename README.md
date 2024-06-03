[![CI](https://github.com/de-it-krachten/ansible-role-samba/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-samba/actions?query=workflow%3ACI)


# ansible-role-samba

Install/configure samba



## Dependencies

#### Roles
None

#### Collections
None

## Platforms

Supported platforms

- Red Hat Enterprise Linux 7<sup>1</sup>
- Red Hat Enterprise Linux 8<sup>1</sup>
- Red Hat Enterprise Linux 9<sup>1</sup>
- CentOS 7
- RockyLinux 8
- RockyLinux 9
- OracleLinux 8
- OracleLinux 9
- AlmaLinux 8
- AlmaLinux 9
- SUSE Linux Enterprise 15<sup>1</sup>
- openSUSE Leap 15
- Debian 11 (Bullseye)
- Debian 12 (Bookworm)
- Ubuntu 20.04 LTS
- Ubuntu 22.04 LTS
- Ubuntu 24.04 LTS
- Fedora 39
- Fedora 40

Note:
<sup>1</sup> : no automated testing is performed on these platforms

## Role Variables
### defaults/main.yml
<pre><code>
# Packages required to install samba
samba_packages:
  - samba
  - cifs-utils

# Should this server function within ActiveDirectory
samba_ad: true

# Should this server support legacy Netbios
samba_netbios: false

# Configuration template
samba_conf: samba.conf.j2

# dfree script
samba_dfree_script: /usr/local/bin/dfree

# List of shares
samba_shares: []

# Should the samba users be defined as OS users
samba_create_users: false

# List of local samba users
samba_users: []

# List of samba scripts
samba_scripts: []

# should this role manage firewall rules
samba_manage_firewall: true

# samba firewall ports
samba_firewall_ports_netbios:
  - { port: 137, proto: udp }
  - { port: 138, proto: udp }
  - { port: 139, proto: tcp }
samba_firewall_ports_ad:
  - { port: 445, proto: tcp }
  - { port: 445, proto: udp }
</pre></code>

### defaults/family-Debian.yml
<pre><code>
# samba service
samba_services:
  - smbd
</pre></code>

### defaults/family-RedHat.yml
<pre><code>
# samba service
samba_services:
  - smb
</pre></code>

### defaults/family-Suse.yml
<pre><code>
# samba service
samba_services:
  - smb
</pre></code>




## Example Playbook
### molecule/default/converge.yml
<pre><code>
- name: sample playbook for role 'samba'
  hosts: all
  become: 'yes'
  vars:
    samba_ad: true
    samba_netbios: true
    samba_create_users: true
    samba_users:
      - name: user1
        password: password1
      - name: user2
        password: password2
    samba_shares:
      - name: homes
        settings:
          comment: Home Directories
          browseable: 'yes'
          read__only: 'no'
          create__mask: '0700'
          directory__mask: '0700'
          valid__users: '%S'
          writeable: 'yes'
          public: 'yes'
          dfree__command: /usr/local/bin/dfree
      - name: private
        settings:
          comment: private directories
          browseable: 'no'
          public: 'no'
          dfree__command: /usr/local/bin/dfree
  tasks:
    - name: Include role 'samba'
      ansible.builtin.include_role:
        name: samba
</pre></code>
