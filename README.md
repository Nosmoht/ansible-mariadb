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

# Usage
```yaml
- hosts: servers
  roles:
  - role: mariadb
```

# Author
[Thomas Krahn](mailto:ntbc@gmx.net)

