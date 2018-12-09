# MySQL Ansible playbooks

requires python3+ ansible 2.4+ 

## MySQL install

### Pre install
we assume the MySQL server configuration as followings:
- 32 CPU core
- 256G Memory
- SSD storage with 20000 IOPS in 16K page size

The mysql config template base on these server configuration.

If your server's configuration dose not match it, please tune the variables in template according to your server.
the template file is `roles/mysql/templates/mysql_config.j2`.

and we assume that:
- you have installed mysql binary files in `/usr/local/mysql`.
- running multiple mysql instances on different ports.
- use user/group `mysql:mysql` to start/stop mysql instances
- mysql config file path and format is `/etc/mysql/${port}.cnf`
- mysql data dir is `/data0/mysql`.
- mysql template file is `/data0/mysql/tmp`.
- mysql socket file path and format is `/tmp/mysql_${port}.sock`.
- use root and socket file to login.

You have to make sure these dirs exist in your OS, 
cause the playbook will only `check` these dirs exists but not create them.

If you want to use a different path, please modify vars in `mysql_install.yml` before install.

Plase note this playbook only support MySQL version 5.6+ or 5.7+.

### Install instance

please modify default password in `inventory.ini` before install, and run this command below:
```console
ansible-playbook mysql_install.yml -e 'hosts=mysql1.example.host,mysql2.example.host,mysql3.example.host port=3306 dbname=mydb'
```

## Replication

we assume replication configuration is:
- use gtid
- use semi_sync
- use parallel replication on 5.7

set up master slave replacation, please modify default password in `inventory.ini` before install

run this command below to create a replication between two instances:
```console
ansible-playbook mysql_repl.yml -e 'master=mysql1.example.host slaves=mysql2.example.host,mysql3.example.host port=3306'
```
