# role-debian_base

Ansible role for conditional reboot of Debian/Ubuntu systems.

> Be aware that this role is opinionated and fits my preferences and way of working.
> It may or may not be suitable for your needs.

## Role variables

### Mandatory variables

None.

### Important optional parameters (with defaults)

`reboot_forced: false` - Indicate of must reboot regardles of the existence of the `reboot_required` file.

These variables can be specified in the `hosts.yaml` inventory file or in the `vars`section of an Ansible playbook, like this:

```yaml
- hosts: all
  become: true
  gather_facts: true
  vars:
    reboot_forced: true
  roles:
    - debian-base # reboot is called from `debian-base`
  ...
```

Or in an inventory file:

```yaml
---
all:
  vars:
    reboot_forced: true
    ...
  children:
  ...

## Usage of this role

To use this role directly, include the following section in a `requirements.yml` file in the local `roles` directory:

```yaml
# Include the 'debian-base` role from GitHub
- src: git@github.com:crazyelectron-io/role-reboot.git
  scm: git
  version: master
  name: reboot
```

> Only include the 'top' roles, dependencies - when listed in `meta/main.yml` of the imported role - will be downloaded automatically.

To retrieve roles like this in your project, run `ansible-galaxy install -r roles/requirements.yml`.
Because these roles will not be updated locally when the repository is changed, to refresh an already retrieved role use `ansible-galaxy install -f -r roles/requirements.yml`

## Dependencies

None.

## Suggested project structure

```shell
├── inventories
│   ├── dev
│   │   ├── group_vars/
│   │   └── hosts.yml
│   └── prod
│       ├── group_vars/
│       └── hosts.yml
├── group_vars/
├── host_vars/
├── files/
├── templates/
├── roles
│   ├── local
│   │   ├── local_role1/
│   │   └── local_role2/
│   ├── requirements.yml
│   ├── .gitignore
├── ansible.cfg
├── README.md
├── some_playbook.yml
├── other_playbook.yml
```

Create a `roles/.gitignore` file to exclude the downloaded roles:

```ini
#Ignore everything in roles dir...
/*
# ... but current file...
!.gitignore
# ... external role requirement file
!requirements.yml
# ... and configured custom/local roles
!local*/
```

Add `roles_path = roles` to `ansible.cfg` to make sure that roles are searched and downloaded in our local folder.

### Workflow to deploy from the project

1. Clone the project repository
2. Download the external roles: `ansible-galaxy install -r roles/requirements`
3. Launch your playbook: `ansible-playbook -i inventories/dev some_playbook.yml -u ANSIBLE_USER`
