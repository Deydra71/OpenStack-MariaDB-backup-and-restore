### Warning: This project is still flagges as "Work in progress", keep that in mind, please.
# OpenStack-MariaDB-backup-and-restore
The Ansible project for creating backups for the core OpenStack services form the MariaDB database as well as restoring them. This project is exclusively set up for the needs of the BUTCA (Brno University of Technology Cyber Arena).

## Use case
This project is intended to be run from the AWX platform, so the inventory and vault credentials are configured there, with few modification, it can be run from the CLI as well.

In the file 'globals.yaml' are defined databases, which can be backuped (restored respectively). Currently, the possible databases are:
 * cinder
 * glance
 * heat
 * keystone
 * neutron
 * nova
 * nova_api
 * nova_cell0
 * placement

If the MariaDB databases will differ in the future, the parameter 'mariadb_databases' can be changed accordingly. Based on the values in the 'mariadb_databases' parameter, the extra-vars can be executed along with the backup or restore playbook.

### The extra vars for the backup.yaml playbook
#### In the AWX environment:
Edit the variables box, e.g.:
```
---
selected_databases:
  - cinder
  - keystone
  - neutron
```

or, if all databases should be backuped:
```
---
selected_databases: 
  - all
```

#### In the CLI:

``` ansible-playbook <path_to_the_backup.yaml> -e '{"selected_databases": ["cinder", "keystone", "neutron"]}' ```

### The extra vars for the restore.yaml playbook
#### In the AWX environment:

Edit the variables box, e.g.:
```
---
selected_backups:
  - cinder
  - keystone
  - neutron
timestamp:
  -  2023-03-25_14:53:50
```

or, if all databases should be restored:
```
---
selected_backups:
  - all
timestamp:
  -  2023-03-25_14:53:50
```

Both 'selected_backups' and 'timestamp' vars are mandatory for successful playbook run!

The 'timestamp' value can be found in the '/tmp' directory as it is the default directory on the host machine for storing the backup files.

#### In the CLI:

``` ansible-playbook <path_to_the_backup.yaml> -e '{"selected_backups": ["all"], "timestamp": ["2023-03-25_14:53:50"]}' ```

For both backup and restore, you can pass extra vars as a file in YAML format and pass it as, e.g.:
ansible-playbook <path_to_the_backup.yaml> -e @vars_file.yml

## TODO: Requirements
## TODO: 
* Copy the backup files to the storage server
* Finnish the testing setup
* Add the playbook, which would backup only the images used for the BUTCA purposes.
