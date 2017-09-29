unixodbc-cfg
============
[![Build Status](https://travis-ci.org/CyVerse-Ansible/ansible-unixodbc-cfg.svg?branch=master)](https://travis-ci.org/CyVerse-Ansible/ansible-unixodbc-cfg)
[![Ansible Galaxy](https://img.shields.io/ansible/role/20679.svg)](https://galaxy.ansible.com/CyVerse-Ansible/unixodbc-cfg/)

This role manages the configuration files for unixODBC. At the moment, it can configure user `.odbc.ini` files, but in the future it will be able to manage system `odbc.ini` and `odbcinst.ini` files.


Requirements
------------

None


Role Variables
--------------

Here are the role variables. None of them are required.

Variable                    | Default                   | Comment
--------------------------- | --------------------------| -------
`unixodbc_cfg_odbcini_path` | /home/`unixodbc_cfg_user` | This is directory where `.odbc.ini` file will be place.
`unixodbc_cfg_sources`      | {}                        | A dictionary defining the data sources. _See below_.
`unixodbc_cfg_user`         | `ansible_user`            | The `.odbc.ini` will be generated for the given user.

Each item in the `unixodbc_cfg_sources` dictionary has a key that is the name of the source with the value being a map with the following fields.

Field               | Required | Default | Comment
------------------- | -------- | ------- | -------
`driver_file`       | yes      |         | The driver file name for the data source
`driver_properties` | no       | {}      | A dictionary containing the properties passed on to the data source driver.
`tracefile`         | no       | null    | This is the file where a trace will be written. If this field isn't present, is `null`, or is empty, no trace will be will written.


Dependencies
------------

none


Example Playbook
----------------

    - hosts: servers
      roles:
         - role: CyVerse-Ansible.unixodbc-cfg
           vars:
             unixodbc_cfg_sources:
               postgres:
                 driver_file: /usr/pgsql-9.3/lib/psqlodbc.so
                 driver_properties:
                   CommLog: 0
                   Database: ICAT
                   Debug: 0
                   Ksqo: 0
                   Port: "{{ dbms_port }}"
                   ReadOnly: no
                   Servername: "{{ dbms_host }}"


License
-------

See [license](/license.md).


Author Information
------------------

Tony Edgin  
<tedgin@cyverse.org>  
[CyVerse](https://cyverse.org)
