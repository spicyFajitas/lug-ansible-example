# lug-ansible-example

## Overview

Example repository of ansible playbooks for @spicyFajita's presentation at the 2024-02-15 Linux Users Group club meeting.

For the purposes of this demonstration, all sensitive passwords/data specific to my environment should have been properly scrubbed.
If this is not the case, then that sucks for me.
If you use this repository directly (forked, copied, etc.) MAKE SURE YOU CHANGE THE ANSIBLE VAULT PASSWORD AND REKEY YOUR ANSIBLE VAULTS!!!

## Installation

```bash
pip install ansible
```

## Usage

To run playbooks, `cd` into the directory of your ansible repo.

To run a playbook against a local machine, run the command `ansible-playbook --connection=local server-playbook.yml -i 127.0.0.1`

To run the playbook on a remote machine, run using the command `ansible-playbook server-xyxplay.yml -i inventory/inventory --vault-password-file location/of/your/vault-password-file.txt`

To limit the playbook run to certain hosts, use the `--limit hostname` flag. You can limit to more than one host by specifying the group name instead of hostname or by putting multiple hosts: `--limit "host1,host2,host3,host4"`

To run a playbook in pretend mode, add the flag `--check`

### Common Commands

```bash
# run a playbook in "pretend" mode 
ansible-playbook server-xyxplay.yml -i inventory/inventory --vault-password-file location/of/your/vault-password-file.txt --check

# run a playbook in "real" mode
ansible-playbook server-xyxplay.yml -i inventory/inventory --vault-password-file location/of/your/vault-password-file.txt --check

# edit an ansible vault
ansible-vault edit path/to/ansible/vault-file.yml
```

## Project Structure Reference

If a more complex structure is needed, this is a good one to follow.

```txt
ansible-playbook-repo/
├── inventories/
│   ├── production/
│   │   └── hosts           # Inventory file for production servers
│   └── staging/
│       └── hosts           # Inventory file for staging servers
|
├── group_vars/
│   ├── all.yml            # Group variables that apply to all hosts (maybe specify variable {{ ssh_keys }} that will be installed on all hosts)
│   ├── production.yml     # Group variables specific to production environment
│   └── staging.yml        # Group variables specific to staging environment
|
├── host_vars/
│   ├── host1.yml         # Host variables specific to host1 (for example, locked down host - only install {{ senior_ssh_keys }})
│   └── host2.yml         # Host variables specific to host2
|
├── roles/
│   ├── common/           # Common role - installs packages, installs SSH keys, etc. on all servers
│   ├── servers/          # Web server role/
│   │   ├── serverA       # specific role for specific server
│   │   ├── serverB       # specific role for specific server
│   │   └── serverC       # specific role for specific server
│   ├── database/         # Database server role - installs/configures database specific things (percona, mysql, etc)
│   └── ...
|
├── site.yml              # Main playbook file
└── ansible.cfg           # Ansible configuration file
```

## Requirements

- Ansible installed on an ansible-controller
- SSH access to any host you want Ansible to be able to configure
