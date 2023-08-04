# Netology playgroud
# Ansible playbook Clickhouse & Vector installation on Centos7

- [Installation](#installation)
- [Prerequisite](#Prerequisite)
- [Configure](#Configure)
- [Install](#Install)
- [Process](#Process)

## Installation

This ansible playbook supports the following,

- Can be deployed on baremetal and VMs(AWS EC2, YC, Azure etc)
- Supports Centos7

### Prerequisite

- **Ansible 2.9+**

### Configure

Refer the file `inventories/prod.yml` to change:
- VM public IP addresses,
- User if it is not default **Centos**,
- Specify the path to the private ssh key

### Install


    # Deploy with ansible playbook - run the playbook as root:
    ansible-playbook -i inventories/prod.yml site.yml

### Process

Clickhouse:
- Gettting clickhouse distributive (noarch / x86_64)
- Installing clickhouse packages
- Creating clickhouse database 'logs'

Vector:
- Getting vector distributive
- Preparing the configuration
- Configuring vector using template
- Configuring vector service


