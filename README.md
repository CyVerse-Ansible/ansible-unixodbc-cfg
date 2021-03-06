unixodbc-cfg
============
[![Build Status](https://travis-ci.org/CyVerse-Ansible/ansible-unixodbc-cfg.svg?branch=master)](https://travis-ci.org/CyVerse-Ansible/ansible-unixodbc-cfg)
[![Ansible Galaxy](https://img.shields.io/badge/role-cyverse--ansible.unixodbc--cfg-blue.svg)](https://galaxy.ansible.com/cyverse-ansible/unixodbc-cfg/)

This role manages the configuration files for unixODBC. At the moment, it can
configure user `.odbc.ini` files, but in the future it will be able to manage
system `odbc.ini` and `odbcinst.ini` files.


Requirements
------------

None


Role Variables
--------------

Here are the role variables. None of them are required.

Variable                    | Default                   | Comment
--------------------------- | --------------------------| -------
`unixodbc_cfg_defer`        | false                     | Whether or not to defer execution. _See below_.
`unixodbc_cfg_group`        | `unixodbc_cfg_user`       | The `.odbc.ini` will be generated for this group.
`unixodbc_cfg_odbcini_path` | /home/`unixodbc_cfg_user` | This is directory where `.odbc.ini` file will be place.
`unixodbc_cfg_sources`      | []                        | A list of data source definitions. _See below_.
`unixodbc_cfg_user`         | `ansible_user`            | The `.odbc.ini` will be generated for the given user.

If `unixodbc_cfg_defer` is `true`, the role will make no changes when its
`main.yml` tasks are run. This allows implicit dependency management through
the `meta/main.yml` file to be used when this role is used by another role
through an `import_role` or `include_role` task.

Each item in the `unixodbc_cfg_sources` list is a map with the following fields.

Field               | Required | Default | Comment
------------------- | -------- | ------- | -------
`driver_file`       | yes      |         | The driver file name for the data source
`driver_properties` | no       | {}      | A dictionary containing the properties passed on to the data source driver.
`source_name`       | yes      |         | The name of the source.
`tracefile`         | no       | null    | This is the file where a trace will be written. If this field isn't present, is `null`, or is empty, no trace will be will written.


Dependencies
------------

none


Example Playbook
----------------

Here's an example where the role is run from a play.

    - hosts: servers
      roles:
         - role: cyverse-ansible.unixodbc-cfg
           vars:
             unixodbc_cfg_sources:
               - source_name: postgres
                 driver_file: /usr/pgsql-9.3/lib/psqlodbc.so
                 driver_properties:
                   CommLog: 0
                   Database: ICAT
                   Debug: 0
                   Ksqo: 0
                   Port: "{{ dbms_port }}"
                   ReadOnly: no
                   Servername: "{{ dbms_host }}"

Here's an example where the role's `odbc.yml` tasks are run from an
`include_role` task.

    - include_role:
        name: cyverse-ansible.unixodbc-cfg
        tasks_from: odbc.yml
      vars:
        unixodbc_cfg_odbcini_path: /var/lib/irods
        unixodbc_cfg_user: "{{ service_account_name }}"
        unixodbc_cfg_sources:
          - source_name: postgres
            driver_file: /usr/pgsql-9.3/lib/psqlodbc.so
            driver_properties:
              CommLog: 0
              Database: "{{ db_name }}"
              Debug: 0
              Ksqo: 0
              Port: "{{ dbms_port }}"
              ReadOnly: no
              Servername: "{{ dbms_host }}"


License
-------

See [license](/LICENSE.txt).


Author Information
------------------

Tony Edgin  
<tedgin@cyverse.org>  
[CyVerse](https://cyverse.org)
