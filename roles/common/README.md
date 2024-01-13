# Common Role

## Description
The common role is a foundational role designed to ensure that base level configurations and packages are consistent across all hosts. This role takes care of basic system settings such as:

- Setting the hostname
- Installation of base and host specific packages
- SSH configuration
- Active Directory join and configuration of SSSD
- Sudoers configuration

## Role Variables

This role uses several variables, some of which are defined in the `group_vars` of the inventory. Others can be defined in the `host_vars` for an individual host configuration. You can modify these variables to customize the behavior of this role.

**common_base_packages**: (defined in `group_vars/all.yaml`)

A list of packages that will be installed on all hosts. 

Example:
```
common_base_packages:
  - git
  - jq
  - unzip
  - make
```

**host_packages**: (defined in `host_vars/<hostname>.yaml`)

A list of additional packages that are installed on a specific host. This is defined in the host_vars directory under the hostname.

Example:
```
host_packages:
  - 'nginx'
  - 'vim'
```

**common_sudoers**: (defined in `group_vars/all.yaml`)

A list of sudo rules that will be applied to all hosts. Each rule should specify the user or group name, command, and the runas user.

Example:
```
common_sudoers:
  - name: 'example-user'
    group: 'example-group'
    command: 'ALL'
    runas: 'ALL'
```

**host_sudoers**: (defined in `host_vars/<hostname>.yaml`)

A list of additional sudo rules that are specific to a host. This is defined in the host_vars directory under the hostname.

Example:
```
host_sudoers:
  - name: 'extra-user'
    group: 'extra-group'
    command: 'ALL'
    runas: 'ALL'
```

**common_ad_groups**: (defined in `vars/main.yaml`)

A list of common AD groups that are added to the nodes sssd configuration to control who can log in.

Example:
```
common_ad_groups:
  - "group_name"
```

**allowed_ad_groups**: (defined in `inventories/*/<group>.yaml`)

A list of AD groups that are added to the nodes sssd configuration to control who can log in.

Example:
```
allowed_ad_groups:
  - "group_name"
```