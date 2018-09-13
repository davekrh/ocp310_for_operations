# Lesson Five: Creating the OpenShift Ansible Inventory

OpenShift uses a centralized Ansible inventory file to describe, install, and modify the environment. This file details the machines used by OpenShift, their roles, what features are enabled, and many other details. The inventory file is crucial for installation and is also utilized in running environments to make changes. One common task is to add or remove machines after the installation of the environment has been completed.

OpenShift requires access to DNS. Crucially, OpenShift requires the ability to utilize DNS wildcards. This is required for arbitrary applications to be created and reachable via DNS names. 

Our environment has pre-configured a wildcard DNS domain. On any machine, you can ping arbitrary DNS names on `cloud.example.com` to refer back to OpenShift’s master:
```
[student@workstation ~]$ ping jobu.cloud.example.com
PING jobu.cloud.example.com (10.0.0.11) 56(84) bytes of data.
64 bytes from node1.example.com (10.0.0.11): icmp_seq=1 ttl=64 time=1.32 ms
64 bytes from node1.example.com (10.0.0.11): icmp_seq=2 ttl=64 time=0.536 ms
^C

[root@master ~]# ping zappa.cloud.example.com
PING zappa.cloud.example.com (10.0.0.11) 56(84) bytes of data.
64 bytes from node1.example.com (10.0.0.11): icmp_seq=1 ttl=64 time=0.665 ms
64 bytes from node1.example.com (10.0.0.11): icmp_seq=2 ttl=64 time=0.516 ms
^C
```
It is important to have a DNS wildcard domain implemented well in advance of OpenShift’s installation. This domain is reflected as `openshift_master_default_subdomain` in the inventory file.

OpenShift’s installation documentation provides examples of the inventory file. You can also find examples under `/usr/share/doc/openshift-ansible-docs-$version/docs/example-inventories/`.

The inventory file resides on the master host. Log onto the master host:
```
[student@workstation ~]$ sudo ssh master
Last login: Thu Aug  2 10:39:17 2018 from workstation.example.com
Red Hat Enterprise Linux 7
[root@master ~]#
```
You should have a default, completely commented `/etc/ansible/hosts` file on master. Blanking this file is a good practice before creating your OpenShift inventory. Run the folling on master to blank the default inventory file:
```
[root@master ~]# > /etc/ansible/hosts
```
We will now build the inventory file used by OpenShift. Open a text editor and input the following in `/etc/ansible/hosts`:
```
# 20180821 -- begin OpenShift inventory file
# Create an OSEv3 group that contains the masters, nodes, and etcd groups
[OSEv3:children]
masters
nodes
etcd

# Set variables common for all OSEv3 hosts
[OSEv3:vars]
# SSH user, this user should allow ssh based auth without requiring a password
ansible_ssh_user=root
os_firewall_use_firewalld=True

# 20180521
openshift_master_default_subdomain=cloud.example.com

# 20180521, from msei-, we are *way* under-resourced
openshift_disable_check=memory_availability,disk_availability

openshift_deployment_type=openshift-enterprise
openshift_release=v3.10

# uncomment the following to enable htpasswd authentication; defaults to DenyAllPasswordIdentityProvider
openshift_master_identity_providers=[{'name': 'htpasswd_auth', 'login': 'true', 'challenge': 'true', 'kind': 'HTPasswdPasswordIdentityProvider'}]

# host group for masters
[masters]
master.example.com

# host group for etcd
[etcd]
master.example.com

# host group for nodes, includes region info
[nodes]
master.example.com openshift_node_group_name='node-config-master-infra'
node1.example.com openshift_node_group_name='node-config-compute'
node2.example.com openshift_node_group_name='node-config-compute'

# end OpenShift inventory
```

Please examine `/etc/ansible/hosts` to ensure everything appears correct.

TIP: In particular, depending on the tools and editor used to cut-and-paste, be sure that only the desired comment lines start with `#` and not the entire file!

At this point, we are ready to validate our setup and install OpenShift.
