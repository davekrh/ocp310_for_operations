# Lesson Seven: Interacting With Your Installation

The prior lessons build on each other. If there’s an issue along the way, it’s very unlikely the OpenShift cluster will be installed. All errors should be investigated, and many can be addressed in the inventory file.

Log onto master, and run the following to get information about your cluster.
```javascript
[root@master ~]# oc get nodes
NAME                 STATUS    ROLES          AGE       VERSION
master.example.com   Ready     infra,master   1h        v1.10.0+b81c8f8
node1.example.com    Ready     compute        1h        v1.10.0+b81c8f8
node2.example.com    Ready     compute        1h        v1.10.0+b81c8f8

[root@master ~]# oc get nodes -o wide
NAME                 STATUS    ROLES          AGE       VERSION           EXTERNAL-IP   OS-IMAGE                   KERNEL-VERSION              CONTAINER-RUNTIME
master.example.com   Ready     infra,master   1h        v1.10.0+b81c8f8   <none>        Red Hat Enterprise Linux   3.10.0-862.9.1.el7.x86_64   docker://1.13.1
node1.example.com    Ready     compute        1h        v1.10.0+b81c8f8   <none>        Red Hat Enterprise Linux   3.10.0-862.9.1.el7.x86_64   docker://1.13.1
node2.example.com    Ready     compute        1h        v1.10.0+b81c8f8   <none>        Red Hat Enterprise Linux   3.10.0-862.9.1.el7.x86_64   docker://1.13.1
[root@master ~]#

[root@master ~]# oc get projects
NAME                                DISPLAY NAME   STATUS
default                                            Active
kube-public                                        Active
kube-service-catalog                               Active
kube-system                                        Active
management-infra                                   Active
openshift                                          Active
openshift-ansible-service-broker                   Active
openshift-infra                                    Active
openshift-logging                                  Active
openshift-node                                     Active
openshift-sdn                                      Active
openshift-template-service-broker                  Active
openshift-web-console                              Active
```
Your output should look similar to the above. After you’ve explored a bit, attempt a login from the command line on master:
```javascript
[root@master ~]# oc login https://master.example.com:8443
```
You should be wondering what username and password to use. We haven’t specified our credentials yet.
```javascript
[root@master ~]# oc login https://master.example.com:8443
Authentication required for https://master.example.com:8443 (openshift)
Username: admin
Password:
Login failed (401 Unauthorized)
Verify you have provided correct credentials.
```
Referring to our OpenShift inventory file, we had specified htpasswd as a valid authentication method in our inventory file:
```javascript
openshift_master_identity_providers=[{'name': 'htpasswd_auth', 'login': 'true', 'challenge': 'true', 'kind': 'HTPasswdPasswordIdentityProvider'}]
```
We will create an admin user and password that will allow us to log in and see additional information:
```javascript
htpasswd -b /etc/origin/master/htpasswd admin ocpadmin
oc adm policy add-cluster-role-to-user cluster-admin admin
```
We now have an admin user and password created. Log in again:
```javascript
[root@master ~]# oc login https://master.example.com:8443
Authentication required for https://master.example.com:8443 (openshift)
Username: admin
Password:
Login successful.

You have access to the following projects and can switch between them with 'oc project <projectname>':

  * default
    kube-public
    kube-service-catalog
    kube-system
    management-infra
    openshift
    openshift-ansible-service-broker
    openshift-infra
    openshift-logging
    openshift-node
    openshift-sdn
    openshift-template-service-broker
    openshift-web-console

Using project "default".
```
You can also try a few different commands once logged in:
```javascript
[root@master ~]# oc whoami
admin
You have new mail in /var/spool/mail/root
[root@master ~]# oc whoami --show-server
https://master.example.com:8443
[root@master ~]# oc get projects
NAME                                DISPLAY NAME   STATUS
default                                            Active
kube-public                                        Active
kube-service-catalog                               Active
kube-system                                        Active
management-infra                                   Active
openshift                                          Active
openshift-ansible-service-broker                   Active
openshift-infra                                    Active
openshift-logging                                  Active
openshift-node                                     Active
openshift-sdn                                      Active
openshift-template-service-broker                  Active
openshift-web-console                              Active
```
Next we will create a user with limited authority. Perform the following on master:
```javascript
[root@master ~]# htpasswd -b /etc/origin/master/htpasswd user ocpuser
Adding password for user user
```
You now will have a user named “user” with the password of “ocpuser”. You can log in via the CLI by doing the following:
```javascript
[root@master ~]# oc whoami
admin
[root@master ~]# oc login -u user
Authentication required for https://master.example.com:8443 (openshift)
Username: user
Password:
Login successful.

You don't have any projects. You can try to create a new project, by running

    oc new-project <projectname>

[root@master ~]# oc whoami
user
[root@master ~]# oc whoami --show-server
https://master.example.com:8443
```
You can change users on the command line using “oc login”. It’s a good idea to determine which user is active before interacting with OpenShift. For the remaining labs, we will be using the “user” OpenShift account.
