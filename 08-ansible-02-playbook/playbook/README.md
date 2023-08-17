# Netology playgroud
# Ansible playbook Clickhouse & Vector installation on Centos7

This Ansible playbook is designed to automate the installation of ClickHouse and Vector on specified hosts. It streamlines the setup process for ClickHouse database and Vector log collector.

- [Structure](#Structure)
- [Parameters](#Parameters)
- [Tags](#Tags)
- [Installation](#installation)
- [Prerequisite](#Prerequisite)
- [Configure](#Configure)
- [Install](#Install)
- [Process](#Process)

## Playbook Structure

The playbook consists of two main sections: one for installing ClickHouse and another for installing Vector.

## Parameters

This playbook includes the following parameters that can be configured:

- clickhouse_version: Specify the version of ClickHouse to install.
- clickhouse_packages: Choose ClickHouse distribution packages to download.
- vector_config_dir: Set the directory where Vector's configuration will be stored.
Tags

## Tags

To execute specific sections of the playbook, you can utilize the following tags:

- clickhouse: Run tasks related to ClickHouse installation.
- vector: Run tasks related to Vector installation.

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


