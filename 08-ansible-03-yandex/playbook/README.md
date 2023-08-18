# Ansible playbook Clickhouse, Vector and Lighthouse + NGINX installation on Centos7

This Ansible playbook is designed to automate the installation of Clickhouse, Vector and Lighthouse + NGINX on three different hosts. It streamlines the setup process for ClickHouse database, Vector log collector and Lighhouse WEB-perfomance tool running on NGINX web server.

- [Structure](#Structure)
- [Parameters](#Parameters)
- [Tags](#Tags)
- [Installation](#installation)
- [Prerequisite](#Prerequisite)
- [Configure](#Configure)
- [Install](#Install)
- [Process](#Process)

## Playbook Structure

The playbook consists of four sections:
1. Installing NGINX on host #1
2. Installing Lighthouse on host #1
3. Installing Vector on host #2
4. Installing Clickhouse on host #3

## Parameters

This playbook includes the following parameters that can be configured:

- clickhouse_version: Specify the version of ClickHouse to install.
- clickhouse_packages: Choose ClickHouse distribution packages to download.
- vector_config_dir: Set the directory where Vector's configuration will be stored.
- lighthouse_vcs: Git source for Lighthouse distribution
- lighthouse_location_dir: Set the directory where Lighhouse is located

## Tags

To execute specific sections of the playbook, you can utilize the following tags:

- clickhouse: Run tasks related to ClickHouse installation.
- vector: Run tasks related to Vector installation.
- nginx: Run tasks related to NGINX installation.
- lighthouse: Run tasks related to Lighthouse installation.

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
- User for nginx

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

NGINX:
- Install EPEL-release
- Install NGINX 
- Configuring NGINX using template

Lighthouse:
- Getting Lighthouse distributive from git
- Preparing the configuration including binding to NGINX
- Configuring Lighthouse using template
