Galera
=========
[![Opstree Solutions][opstree_avatar]][opstree_homepage]<br/>[Opstree Solutions][opstree_homepage] 

  [opstree_homepage]: https://opstree.github.io/
  [opstree_avatar]: https://img.cloudposse.com/150x150/https://github.com/opstree.png

An Ansible role to install and configure a Galera Cluster with Mysql 5.7 on Debian and Redhat servers.

Version History
---------------

|**Date**| **Version**| **Description**| **Changed By** |
|----------|---------|---------------|-----------------|
|**April '30** | v.1.0 | Initial Draft | Abhishek Vishwakarma |

Supported OS
------------
  * CentOS:7
  * Ubuntu:bionic
  * Ubuntu:xenial

Dependencies
------------
* Define Cluster name for each host using variable <b>'node_name'</b> & <b> 'server _id' </b> in inventory file of Ansible
* Mysql Password must contain special and numeric characters and having minimum length 8  

Role Variables
--------------
We are using below mention default variables in this role.

|**Variables**| **Default Values**|**Description**|
|----------|---------|---------------|
| galera_cluster_name | galera_cluster | Cluster name |
| master_slave_replication | false | Set true if read replica required |
| mysql_root_password | r@@t123 | Password for Mysql root account |
| add_node | false | Set true to add new node in cluster |

We are using below mention specific variables to create read replica in this role.

|**Variables**| **Default Values**|**Description**|
|----------|---------|---------------|
| ansible_hostfile_master_groupname | master | Group name in inventory file for master galera cluster|
| ansible_hostfile_slave_groupname | slave | Group name in inventory file for read replica |
| replica_user | replicant | Username of replication |
| replica_pass | Rep@1234 | Password of replication |
| slave_user | slave | Username of slave user |
| slave_passwd | Slave@123 | Password of slave user |

We are using below mention variables to setup mysql and galera in this role.

|**Variables**| **Description**|
|----------|---------|
| mysql_wsrep_url | Mysql url for Ubuntu server |
| mysql_wsrep_repo | Mysql repo location for Ubuntu server |
| galera_url | Galera url for Ubuntu server |
| galera_repo | Galera repo location for Ubuntu server |
| mysql_python | Mysql python url for Centos server |
| mysql_rpm | Mysql rpm location for Centos server |

Inventory
----------
An inventory should look like this for galera cluster:-
```ini
[master]                 
192.168.1.198    ansible_user=ubuntu    node_name=Node1
192.168.2.208    ansible_user=centos    node_name=Node2
192.168.3.201    ansible_user=opstree   node_name=Node3                   
```
An inventory should look like this for galera cluster with read replica:-
```ini
[all:vars]
master_ip=192.168.4.154
slave_ip=192.168.1.210

[master]                 
192.168.1.198    ansible_user=ubuntu    node_name=Node1  server_id=1
192.168.2.208    ansible_user=centos    node_name=Node2  server_id=2
192.168.3.201    ansible_user=opstree   node_name=Node3  server_id=3
192.168.4.154    ansible_user=ubuntu    node_name=Node4  server_id=4 
[slave]
192.168.1.210    ansible_user=ubuntu    server_id=5 
```
An inventory should look like this to add new node in existing galera cluster:-
```ini
[master]                 
192.168.1.198    ansible_user=ubuntu    node_name=Node1
192.168.2.208    ansible_user=centos    node_name=Node2
192.168.3.201    ansible_user=opstree   node_name=Node3
192.168.4.154    ansible_user=ubuntu    node_name=Node4  donar_node_name=Node1
```
## Example Playbook
Here is an example playbook:-
```yml
---
- hosts: master
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
Future Proposed Changes
-----------------------

References
----------
- **[database](https://galeracluster.com/)**

## Author
**[Abhishek Vishwakarma](mailto:abhishek.vishwakarma@opstree.com)*
