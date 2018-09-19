# Lesson Six: Installing the OpenShift Cluster

With DNS verified, prerequisite packages installed, and the inventory file in place, we are ready to perform the OpenShift installation. The installation is contained within two steps. The first step completes prerequisites prior to installation, and the second step performs the actual installation. The first step could take as little as five minutes, while the install can take thirty or more minutes.

These steps will be performed on the master virtual machine. Log in now:
```
[student@workstation ~]$ sudo ssh master
Last login: Thu Aug  2 10:39:23 2018 from workstation.example.com
Red Hat Enterprise Linux 7
[root@master ~]#
```
If this is the first time you've tried to log onto other hosts from master, you'll be prompted to accept fingerprints. We need to accept the fingerprints of our cluster otherwise the installation will prompt us. We can ensure all fingerprints are accepted by running ansible:
```
[root@master ~]# ansible all -m ping
```
If you have fingerprints that haven't been accepted yet, you'll see something like the following:
```
[root@master ~]# ansible all -m ping
The authenticity of host 'master.example.com (10.0.0.11)' can't be established.
ECDSA key fingerprint is SHA256:pqEzU0KcYUE6PnN10cZidWqyDGbjMzYkyrfQuqlKTKk.
ECDSA key fingerprint is MD5:03:85:97:10:f3:78:37:b9:a4:a2:88:99:04:81:b8:23.
Are you sure you want to continue connecting (yes/no)? The authenticity of host 'node2.example.com (10.0.0.13)' can't be established.
ECDSA key fingerprint is SHA256:pqEzU0KcYUE6PnN10cZidWqyDGbjMzYkyrfQuqlKTKk.
ECDSA key fingerprint is MD5:03:85:97:10:f3:78:37:b9:a4:a2:88:99:04:81:b8:23.
Are you sure you want to continue connecting (yes/no)? The authenticity of host 'node1.example.com (10.0.0.12)' can't be established.
ECDSA key fingerprint is SHA256:pqEzU0KcYUE6PnN10cZidWqyDGbjMzYkyrfQuqlKTKk.
ECDSA key fingerprint is MD5:03:85:97:10:f3:78:37:b9:a4:a2:88:99:04:81:b8:23.
Are you sure you want to continue connecting (yes/no)? yes
master.example.com | SUCCESS => {
    "changed": false, 
    "failed": false, 
    "ping": "pong"
}
yes
node2.example.com | SUCCESS => {
    "changed": false, 
    "failed": false, 
    "ping": "pong"
}
yes
node1.example.com | SUCCESS => {
    "changed": false, 
    "failed": false, 
    "ping": "pong"
}
[root@master ~]# 
```
Onward to installation. Become root on the master (if needed) and run the following:
```
[student@workstation ~]$ sudo ssh master
Last login: Thu Aug  2 10:39:23 2018 from workstation.example.com
Red Hat Enterprise Linux 7
[root@master ~]# id
uid=0(root) gid=0(root) groups=0(root) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
[root@master ~]# hostname
master.example.com

[root@master ~]# time ansible-playbook \
    /usr/share/ansible/openshift-ansible/playbooks/prerequisites.yml

[root@master ~]# time ansible-playbook \
    /usr/share/ansible/openshift-ansible/playbooks/deploy_cluster.yml
```
You should see the following after about thirty minutes:
```
...

PLAY RECAP ***********************************************************************************************************************************************************************************************************************************************************************
localhost                  : ok=14   changed=0    unreachable=0    failed=0   
master.example.com         : ok=628  changed=276  unreachable=0    failed=0   
node1.example.com          : ok=120  changed=56   unreachable=0    failed=0   
node2.example.com          : ok=120  changed=56   unreachable=0    failed=0   


INSTALLER STATUS *****************************************************************************************************************************************************************************************************************************************************************
Initialization              : Complete (0:00:32)
Health Check                : Complete (0:00:36)
Node Bootstrap Preparation  : Complete (0:06:12)
etcd Install                : Complete (0:01:07)
Master Install              : Complete (0:05:26)
Master Additional Install   : Complete (0:02:24)
Node Join                   : Complete (0:04:25)
Hosted Install              : Complete (0:01:45)
Web Console Install         : Complete (0:00:57)
Service Catalog Install     : Complete (0:03:32)

real    27m16.016s
user    5m53.063s
sys    3m50.009s
You have new mail in /var/spool/mail/root
[root@master ~]#
```
Errors occurring during the installation will appear in bright red. Provided the installation completed successfully, we may now proceed to test and interact with the environment.

[Lesson Seven: Interacting With Your Installation](07-lesson-interacting.md)
