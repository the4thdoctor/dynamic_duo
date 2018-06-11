# Branch 02_ssh_config

This branch configures ssh on the postgres os user with login without password.

The branch contains only the role ssh which performs the following actions

* on each server creates the ssh key pairs for the postgresql user
* using the module fetch retrieves the public keys into the local directory keys/
* using the module autorized_keys and looping over the groups dbserver and bckserver sets the public keys on each server
* calls /bin/true  via ssh using the option StrictHostKeyChecking=no. this way the server fingerprint is added to the known_hosts on each server

## Rollback

When the rollback playbook is called with the variable rbk_ssh=True or rbk_all=True the playbook removes the .ssh directory from the postgres user.
