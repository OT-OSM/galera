Galera
=========
Galera role to install and configure a Galera Cluster with Mysql 5.7 on Debian and Redhat servers.

Requirements
------------
Define Cluster name for each host using variable <b>'node_name'</b> & <b> 'server _id' </b> in inventory file of Ansible

Role Variables
--------------
We are using below mention default variables in this role.

|**Variables**| **Default Values**|**Description**|
|----------|---------|---------------|
| galera_cluster_name | galera_cluster | Cluster name |
| master_slave_replication | false | Set true if read replica required |
| mysql_root_password | r@@t123 | Password for MariaDb root account |

We are using below mention specific variables to create read replica in this role.

|**Variables**| **Default Values**|**Description**|
|----------|---------|---------------|
| ansible_hostfile_master_groupname | master | Group name in inventory file for master galera cluster|
| ansible_hostfile_slave_groupname | slave | Group name in inventory file for read replica |
| replica_user | replicant | Username of replication |
| replica_pass | Rep@1234 | Password of replication |
| slave_user | slave | Username of slave user |
| slave_passwd | Slave@123 | Password of slave user |
| master_ip | 192.168.1.198 | IP-address of master server |
| slave_ip | 192.168.1.210 | IP-address of slave server |

We are using below mention variables to setup mysql and galera in this role.

|**Variables**| **Default Values**|**Description**|
|----------|---------|---------------|
| mysql_url | http://repo.mysql.com/apt/ubuntu | Mysql url for Ubuntu server |
| mysql_repo | deb {{ mysql_url }} {{ ansible_lsb.codename }} mysql-5.7 | Mysql repo location for Debian server |
| mysql_wsrep_url | http://releases.galeracluster.com/mysql-wsrep-5.7/ubuntu | Mysql url for Ubuntu server |
| mysql_wsrep_repo | deb {{ mysql_wsrep_url }} {{ ansible_lsb.codename }} main | Mysql repo location for Ubuntu server |
| galera_url | http://releases.galeracluster.com/galera-3/ubuntu | Galera url for Ubuntu server |
| galera_repo | deb {{ galera_url }} {{ ansible_lsb.codename }} main | Galera repo location for Ubuntu server |
| mysql_rpm | http://repo.mysql.com/mysql57-community-release-el7-9.noarch.rpm | Mysql rpm for Centos server |
| mysql_python | http://mirror.centos.org/centos/7/os/x86_64/Packages/MySQL-python-1.2.5-1.el7.x86_64.rpm | Mysql python url for Centos server |

Inventory
----------
An inventory should look like this for galera cluster:-
```ini
[galerahost]                 
192.168.1.198    ansible_user=ubuntu    node_name=Node1
192.168.2.208    ansible_user=centos    node_name=Node2
192.168.3.201    ansible_user=opstree   node_name=Node3                   
```
An inventory should look like this for galera cluster with read replica:-
```ini
[master]                 
192.168.1.198    ansible_user=ubuntu    node_name=Node1  server_id=2
192.168.2.208    ansible_user=centos    node_name=Node2  server_id=2
192.168.3.201    ansible_user=opstree   node_name=Node3  server_id=3
[slave]
192.168.1.210    ansible_user=ubuntu    server_id=4       
```
## Example Playbook
Here is an example playbook:-
```yml
---
- hosts: galerahost
  roles:
    - role: osm_galera
      become: yes
```
Usage
-----
For using this role you have to execute playbook only
```shell
ansible-playbook -i hosts site.yml
```
## Author
**[Abhishek Vishwakarma](mailto:abhishek.vishwakarma@opstree.com)*
