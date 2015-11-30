mariadb
=========

# Introduction
Ansible role to setup and configure MariaDB on RHEL based systems.

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


# Author
[Thomas Krahn](mailto:ntbc@gmx.net)
