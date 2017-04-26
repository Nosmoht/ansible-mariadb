mariadb
=========
- [Introduction](#introduction)
- [Requirements](#requirements)
- [Variables](#variables)
- [Usage](#usage)

# Introduction
Ansible role to setup and configure MariaDB or MariaDB Galera Cluster.
Database can be ensured by providing they description via __mariadb_dbs__.
Database users can be ensured by providing they description via __mariadb_users__.

Supported distributions
- CentOS 7
- RedHat 7
- Ubuntu Xenial (untested)

# Requirements
- Ansible >= 2.0
- EPEL repository must be available on RHEL based systems.

# Variables

## Standard variables

| Name | Description | Default |
|:-----|:------------|:--------|
| mariadb_config_file_path | Path to configuration file | /etc/my.cnf.d |
| mariadb_config_file_name | Name of configuration file | server.cnf |
| mariadb_package_name | Package name | _Depends on Linux distribution_ |
| mariadb_package_state | Package state | installed |
| mariadb_service_name | Service name | mariadb |
| mariadb_service_state | Service state | running |
| mariadb_service_enabled | Service enabled | true |
| mariadb_install_python_package | Boolean to define if Python packages should be installed | False |
| mariadb_python_package_name | Name of package to install for Python support | _Depends on Linux distribution_ |
| mariadb_dbs | List of dictionaries of databases to ensure | [] |
| mariadb_users | List of dictionaries of database users to ensure | [] |

## Server System Variables
Following parameters can be set.

For a complete list of possible config parameters see: https://mariadb.com/kb/en/mariadb/server-system-variables/


| Name | Description | Default |
|:-----|:------------|:--------|
| mariadb_bind_address | IP address to bind service on | 127.0.0.1 |
| mariadb_default_storage_engine | Default Storage Engine to use | innodb |
| mariadb_collation_server | Default collation used by the server | utf8_general_ci |
| mariadb_character_set_server | Default character set used by the server | utf8 |
| mariadb_init_connect: | String containing one or more SQL statements, separated by semicolons, that will be executed by the server for each client connecting | 'SET NAMES utf8' |

## Galera Cluster System variables
Following parameters can be set.

For a complete list of possible parameters see: https://mariadb.com/kb/en/mariadb/galera-cluster-system-variables/

| Name | Description | Default |
|:-----|:------------|:--------|
| mariadb_cluster | Boolean to define if a Galera cluster should be deployed | __True__ if mariadb_cluster_nodes is not an empty list |
| mariadb_cluster_nodes | List of IP addresses or hostnames of all cluster nodes | [] |
| mariadb_cluster_wsrep_on | String to define if WSREP is enabled | 'ON' |
| mariadb_cluster_wsrep_provider | String to defined WSREP provider | /usr/lib64/galera/libgalera_smm.so |
| mariadb_cluster_wsrep_cluster_address | The addresses of cluster nodes to connect to when starting up | 'gcomm://{{ mariadb_cluster_nodes &#124; join(",") }}' |
| mariadb_cluster_wsrep_cluster_name | The name of the cluster | mariadb-cluster |
| mariadb_cluster_wsrep_node_address | Specifies the node's network address, in the format ip address[:port]| '{{ mariadb_bind_address }}' |
| mariadb_cluster_wsrep_node_name | Name of this node | '{{ ansible_nodename }}' |
| mariadb_cluster_wsrep_sst_method | Method used for taking the state snapshot transfer | rsync
| mariadb_cluster_binlog_format | ? | row |
| mariadb_cluster_default_storage_engine | | InnoDB |
| mariadb_cluster_innodb_autoinc_lock_mode | Locking mode used for generating auto-increment values. 0 is the traditional lock mode, 1 the consecutive, and 2 the interleaved. See AUTO_INCREMENT handling in XtraDB/InnoDB for more on the lock modes. In order to use Galera, the mode needs to be set to 2. | 2 |

# Usage
Install and start MariaDB using defaults
```yaml
- hosts: servers
  roles:
  - role: mariadb
```

Install also with Python support.
```yaml
- hosts: servers
  roles:
  - role: mariadb
    mariadb_install_python_package: True
```

Install and setup a database called test using utf8 encoding
```yaml
- hosts: servers
  roles:
  - role: mariadb
    mariadb_dbs:
    - name: test
      encoding: utf8
```

Install and import a database test from SQL file.
Create a user test with permissions to access the test database from the database host.
```yaml
- hosts: servers
  roles:
  - role: mariadb
    mariadb_dbs:
    - name: test
      state: import
      src: /tmp/test.sql
    mariadb_users:
    - name: test
      password: topsecret
      host: 127.0.0.1
      priv: '*.*:ALL'
```

Setup a Galera Cluster
```yaml
- hosts: galera-server
  roles:
  - role: mariadb
    mariadb_cluster_nodes: '{{ groups["galera-server"] }}'
    mariadb_bind_address: '{{ ansible_default_ipv4.address }}'
```

# Author
[Thomas Krahn](mailto:ntbc@gmx.net)
