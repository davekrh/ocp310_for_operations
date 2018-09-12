# Appendix

**Create and distribute ssh keys**
Our lab has already populated hosts with passwordless ssh keys. These keys can be created easily however, and can be distributed to hosts using the "ssh-copy-id" command:
```
[student@workstation ~]$ sudo ssh-keygen -b 2048 -t rsa -f /root/.ssh/id_rsa -q -N ""
[student@workstation ~]$ for i in master node1 node2; do ssh-copy-id root@$i; done
```


