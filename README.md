# dinamic_duo
Repository with ansible configuration for ansible and pgbackrest

The playbooks are written against the ansible version 2.5.

Each repository's branch covers a specific step for the installation.

The master branch is the sum of the other branches.

In the ``inventory`` directory there is the file ``hosts.example`` which should be used as template for the working hosts file.

In order to make the playbook work you should copy ``hosts.example`` into ``hosts`` and edit the file for your installation.

Each branch have a special role rollback which can rollback the changes applied by the previous roles,  useful to restore the machines to a previous state.

As this is potentially destructive role by default all the tasks are skipped.

The specific roles can be rolled back individually passing the appropriate variable set to true or entirely using the wildcard variable ``rbk_all``.

    #rolls back the apt configuration leaving the hosts untouched
    ansible-playbook rollback.yml --extra-vars="rbk_apt=True"

## Branch 01_install

This branch installs a base system with postgresql, pgbackrest and pythonpg.

The playbook setup.yml executes two roles, ``hosts`` and ``apt``

### Role hosts

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


### Role apt
The role apt is used to install the required packages listed in the ``apt`` group_vars file.

The role setup the pgdg repository importing the signing key. The lsb_codename is dynamically generated.

The role manages automatically the special case of devuan ascii which maps to debian stretch in the pgdg repository.

The installed packages are postgresql-10, postgresql-client-10 postgresql-contrib-10, pgbackrest and python-psycopg2.
