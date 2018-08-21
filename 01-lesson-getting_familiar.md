# Lesson One: Getting Familiar with the Environment

These steps are to be performed after your instructor has provisioned an environment for you. Using an ssh client from your laptop, log into the workstation machine. You will log in using the “student” user name, and your instructor will provide the password. 

Your workstation virtual machine has a unique, publicly-accessible IP address and DNS name. Your OpenShift machines will communicate via private IP addresses. Because these addresses cannot be routed to your laptop, the workstation acts as a jump host and can access the entire hosted environment.

Ensure you can log into the workstation. You may become root via “sudo -i”. 

Log into the workstation and build an ansible hosts file suitable for managing your OpenShift environment:
```javascript
[student@workstation ~]$ sudo vim /etc/ansible/hosts
[ocp]
master
node1
node2
```

This file allows you to manage your OpenShift environment with ansible. Ansible allows us to easily execute time-consuming commands in parallel during our workshop, among other advantages.

Ansible requires that hosts be reachable without intervention. For this training, we've created keys that allow password-less logins via root. See the appendix for additional information about creating and distributing ssh keys.

Try the following commands to ensure your OpenShift virtual machines are reachable via ansible:
```javascript
[student@workstation ~]$ sudo ansible all -m ping
```
If everything is configured properly, you should see something like the following:
```javascript

node1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
node2 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
master | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```
You may also execute arbitrary shell commands via ansible:
```javascript
[student@workstation ~]$ sudo ansible all -m shell -a 'uptime'
node2 | SUCCESS | rc=0 >>
 15:29:00 up 38 min,  1 user,  load average: 0.07, 0.08, 0.08

node1 | SUCCESS | rc=0 >>
 15:29:00 up 38 min,  1 user,  load average: 0.27, 0.15, 0.15

master | SUCCESS | rc=0 >>
 15:29:00 up 38 min,  1 user,  load average: 0.55, 0.62, 0.77
```
Please ensure ansible is working properly and that your OpenShift virtual machines are reachable before proceeding.

