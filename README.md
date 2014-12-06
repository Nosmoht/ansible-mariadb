ansible-role-mariadb
=========

Ansible role to setup and configure MariaDB on RHEL based systems.

Requirements
------------

No requirements yet.

Role Variables
--------------

    mariadb_package_name: mariadb-server
    mariadb_package_state: installed
    mariadb_service_name: mariadb
    mariadb_service_state: running
    mariadb_service_enabled: true
    mariadb_config_file: /etc/my.cnf
    mariadb_config:
    - section: mysqld
      params:
      - name: default-storage-engine
        value: innodb
      - name: innodb_file_per_table
        value: ''
      - name: collation-server
        value: utf8_general_ci
      - name: init-connect
        value: '''SET NAMES utf8'''
      - name: character-set-server
        value: utf8

Dependencies
------------

No dependencies.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: Nosmoht.ansible-role-mariadb }

License
-------

BSD

Author Information
------------------

Name: Thomas Krahn
mail: ntbc@gmx.net

