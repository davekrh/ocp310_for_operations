# Lesson Three: Install the Initial Software Packages

With the proper software repositories enabled, you can now install the initial packages needed for OpenShift. OpenShift's installation is a multi-step process. We describe the installation of the initial packages, and later we will discuss the creation of the OpenShift inventory file and subsequent steps.

Run the following from your workstation to install the initial packages:
```javascript
[student@workstation ~]$ sudo ansible -f 3 all -m shell -a 'yum install -y wget git net-tools bind-utils yum-utils iptables-services bridge-utils bash-completion kexec-tools sos psacct openshift-ansible docker vim'
```
The initial package installation shouldn’t take too long to complete. The flag we used above, "-f 3", ensures that the installation process is run in parallel on our three machines. In contrast, a typical for loop on the command line will execute the steps one host at a time, needlessly extending the amount of time for the task.

OpenShift also requires that NetworkManager be enabled on the cluster. We can ensure this by running the following:
```javascript
[student@workstation ~]$ sudo ansible -f 3 all -m shell -a "systemctl enable NetworkManager"
```
At this point, you should have the initial packages needed to begin the OpenShift installation in place.
