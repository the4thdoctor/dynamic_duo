# Branch 01_install

This branch installs a base system with postgresql, pgbackrest and pythonpg.

The playbook setup.yml executes two roles, ``hosts`` and ``apt``

## Role hosts

The role ``hosts`` is used to setup the file ``/etc/hosts`` with the three database servers if there is no dns configured.

The role consists of a single task which is skipped by default.
If you want to execute the role ensure that in the host file the ip addresses are correctly associated with the host names

e.g.

    [hosts]
    db01 srv_ip=192.168.1.21
    db02 srv_ip=192.168.1.24
    backupsrv srv_ip=192.168.1.22

When running the playbook add the extra var no_dns=True

e.g.

    ansible-playbook setup.yml --extra-vars="no_dns=True"

If your servers are resolving using dns you can skip this role safely.

### Rollback

When rollback.yml is executed with rbk_hosts=True or rbk_all=True, the host file is replaced with a version with the server name pointing to the localhost address 127.0.0.1

## Role apt
The role apt is used to install the required packages listed in the ``apt`` group_vars file.

The role setup the pgdg repository importing the signing key. The ``lsb_codename`` is dynamically generated from the ansible facts.

The role manages automatically the special case of [devuan ascii](https://devuan.org/) which maps to debian stretch in the pgdg repository.

The role apt installs the packages **postgresql-10, postgresql-client-10 postgresql-contrib-10, pgbackrest, python-psycopg2**.


### Rollback

When rollback.yml is executed with rbk_apt=True or rbk_all=True, the postgres packages are uninstalled, the pgdg repository is removed and the apt key deleted.
