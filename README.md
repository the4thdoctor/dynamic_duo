# dynamic_duo
Repository with ansible configuration for ansible and pgbackrest

The playbooks are written using the ansible version 2.5.

Each repository's branch covers a specific step for the installation.

The master branch is the sum of the other branches.

In the ``inventory`` directory there is the file ``hosts.example`` which should be used as template for the working hosts file.

In order to make the playbook work you should copy ``hosts.example`` into ``hosts`` and edit the file for your installation.

The setup playbook should be executed specifying the vault password where the postgres super user password is encrypted.

e.g.

``ansible-playbook --vault-id default@prompt --extra-vars "no_dns=True"  setup.yml``

Each branch have a special role rollback which can rollback the changes applied by the previous roles,  useful to restore the machines to a previous state.

As this is potentially destructive role by default all the tasks are skipped.

The actions from the roles can be rolled back individually passing the appropriate variable set to true or entirely using the wildcard variable ``rbk_all``.

    #rolback the apt configuration leaving the hosts untouched
    ansible-playbook rollback.yml --extra-vars="rbk_apt=True"

* **rbk_hosts=True** removes the hosts configuration
* **rbk_apt=True** removes the apt configuration
* **rbk_ssh=True** removes the ssh configuration
* **rbk_pgsql=True** removes the postgresql clusters and configuration
* **rbk_pgbackrest=True** removes the pgbackrest backup repository
* **rbk_all=True** removes all the changes applied


## Tutorials

* [Branch 01_install](http://www.pgdba.org/post/2018/06/modern_times/)
* [Branch 02_ssh_config](http://www.pgdba.org/post/2018/07/keep_talking/)
* [Branch 03_pgsql_config](http://www.pgdba.org/post/2018/08/mechanical_elephant/)
