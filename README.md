# ansible-clamav

Ansible role for install and configure [clamAV](https://www.clamav.net/)

Limitation
----------

1. This role not install `epel` you must first install them yourself
see [additional](README.md#Additional)

Requirements
------------
Tested on RHEL 7, CentOS 7/8

* ansible 2.7 or above
* EPEL repository

Role Variables
--------------
**It is not necessary to use all these variable blocks, you can use only the config options you really need.** 

Available variables are be listed below, along with default values. See [defaults/main.yml](defaults/main.yml).

You can change all variables by group_vars or host_vars

| Variable name | Required* | Description | Default Value |
| --- | --- | --- | --- |
| `clamav_os_packages` | **No** | Clamav package |  | 
| `clamav_daemon_custom_config` | **No** | Custom configuration for clamav | [] | 
| `clamav_daemon_state` | **No** | ClamAV daemon state | started | 
| `clamav_daemon_enabled` | **No** | Should service enable at the boot | true |
| `clamav_freshclam_custom_config` | **No** | Custom configuration for freshclam | [] |
| `clamav_freshclam_daemon_state` | **No** | ClamAV update daemon state | started |
| `clamav_freshclam_daemon_enabled` | **No** | Should service enable at the boot | true | 
| `clamav_managed_log` | **No** | Managed log file for service | false | 
| `clamav_daemon_log_dir` | **No**   | clamav log dir | /var/log | 
| `clamav_freshclam_log_dir` | **No** | freshclam log dir | /var/log | 

****Required*** - if "yes", you need to set this parameter under your conditions.

if `clamav_managed_log` set to true then clamd and freshclam application will write its logs to a file

List of hashes `clamav_freshclam_custom_config` and `clamav_daemon_custom_config` have a two mandatory items
`option` and `value` and optional items `state`

like this:
```
clamav_freshclam_custom_config:
  - option: item1 
    value: value1
  - option: item2
    value: value2
    state: absent
```

All configuration parameters you can see on the [clamAV documentation][1]

Dependencies
------------
none

Additional
-----------
1. for install EPEL you can use this role [geerlingguy.repo-epel][2]

For example:
```yaml
- name: Install Epel
  hosts: 
    - clamav.example.com
  become: yes
  become_method: sudo
  roles:
    - geerlingguy.repo-epel

```
 
Example Playbook
------------

```yaml
- name: Install ClamAV
  hosts: 
    - clamav.example.com
  become: yes
  become_method: sudo
  roles:
    - geerlingguy.repo-epel
    - vitcheus.clamav
  vars:
    clamav_daemon_custom_config:
      - option: SelfCheck
        value: 300
```

[1]: https://www.clamav.net/documents/configuration
[2]: https://github.com/geerlingguy/ansible-role-repo-epel
