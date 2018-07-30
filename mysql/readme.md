# MySQL Ansible playbooks

requires python3+ ansible 2.4+ 

## MySQL install

before install

a. we assume that
- you have installed mysql in /usr/local/mysql
- running multiple MySQL instances on one machine via different ports
- install mysql conf as `/etc/mysql/$port.cnf`
- install mysql data in /data0/mysql
- install mysql template file in /data0/mysql/tmp
- install mysql socket as `/tmp/mysql_$port.sock`, and root login via socket
- use user and group `mysql` to start instances

You have make sure these dirs exist on server, the playbook will check these dirs.
 
b. and we assume the MySQL server configuration as followings.
- 32 CPU core
- 256G Memory
- SSD storage with 20000 IOPS in 16K page size

If your machine dose not match this configuration, you should tune the variables in template according to your server.
template is `roles/mysql/templates/mysql_config.j2`.

Plase note this playbook only support MySQL version 5.6+ or 5.7+.

---
install  mysql instance

please modify default password in `inventory.ini` before install 
```console
ansible-playbook mysql_install.yml -e 'hosts=mysql1.example.host,mysql2.example.host,mysql3.example.host port=3306 dbname=mydb'
```

## Replication

we assume that
- use gtid
- use semi_sync
- use parallel replication on 5.7

set up master slave replacation, please modify default password in `inventory.ini` before install 
```console
ansible-playbook mysql_repl.yml -e 'master=mysql1.example.host slaves=mysql2.example.host,mysql3.example.host port=3306'
```
