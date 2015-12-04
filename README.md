mariadb
=========
- [Introduction](#introduction)
- [Requirements](#requirements)
- [Variables](#variables)
- [Usage](#usage)

# Introduction
Ansible role to setup and configure MariaDB on RHEL based systems.
Database can be ensured by providing they description via __mariadb_dbs__.
Database users can be ensured by providing they description via __mariadb_users__.

# Requirements
- EPEL repository must be available on RHEL based systems.

# Variables
| Name | Description | Default |
|:-----|:------------|:--------|
| mariadb_package_name | Package name | mariadb-server |
| mariadb_package_state | Package state | installed |
| mariadb_service_name | Service name | mariadb |
| mariadb_service_state | Service state | running |
| mariadb_service_enabled | Service enabled | true |
| mariadb_config_file | Configuration file | /etc/my.cnf |
| mariadb_install_python_package | Boolean to define if Python packages should be installed | False |
| mariadb_python_package_name | Name of package to install for Python support | MySQL-python |
| mariadb_dbs | List of dictionaries of databases to ensure | [] |
| mariadb_users | List of dictionaries of database users to ensure | [] |

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

# Author
[Thomas Krahn](mailto:ntbc@gmx.net)
