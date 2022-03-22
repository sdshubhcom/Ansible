# Ansible

# What is Automation and Orchestration?

- Automation is the grouping different repeatative task into one and writing a script for the task and execute them as one.
- Orchestration means prioritizing the task which task is to be perform after the completion of another task and also defining the dependences of the tasks.

# What is Push and Pull servers?

- Pull server means simply fetching the information from the server all the script will be execute on the client side in this type of server there is no need 
  of high-end CPU and memory but medium level is required. Eg : Puppet, Chef, Salt Stack.
  
- In Push server the script and all other commands are executed on the server side it requires high end CPU, disk and memory. Eg : Ansible, Terraform, 
  Puppet

- In the benchmark report we have to include that if the server is used for pushing the data or pulling the data.

# Ansible Setup (Installation)

1. Create three instance of centos 7.5 private using community AMI’s of 1 CPU AND 1 GB RAM.
2. Now install epel package for extra packages for linux
```
yum install epel-release -y 
```
- install vi editor command 
```
yum install vim -y
```
3. Now you have to enable the **password authentication** **yes** in all the machines to do so you have to write this command in all the machines
```
vim /etc/ssh/sshd_config 
```
4. In all 3 machines now you restart sshd service
```
systemctl restart sshd
```
5. Now add user name “ansible” in all the machines and set the password.
```
useradd ansible
```
```
passwd ansible
```
6. Add the ansible in the suders list with the help of **visudo** command.
```
ansible ALL=(ALL)       NOPASSWD:       ALL
```
7. Login as **ansible** user into ansible server.
```
sudo su - ansible
```
8. Generate the key for ansible with the help of command.
```
ssh-keygen
```
9. From ansible server,from ansible user using ssh-copy-id command exchange the ssh keys to node1,node2 servers.

- **For GCP you can give directly name of the VM and for AWS you have to give IP address of the VM.**

```
ssh-copy-id node1
```
```
ssh-copy-id node2
```
10. you can check the connection by command 
```
ssh (ip address of the machine).
```
10. Now install ansible only in the "ansible" server.

- if you use -y you wont be able to see which dependencies packages are getting install. In general, you don’t use -y to avoid unnecessarily installing 
  of  dependencies packages

```
sudo yum install ansible -y
```

**Make sure following dependency should also be installed**
**1. python-paramiko**
paramiko package after installed only we can maintain the older version clients.
The paramiko transport is provided because many distributions, in particular EL6 and before do not support ControlPersist in their SSH implementations. 
This is needed on the Ansible control machine to be reasonably efficient with connections. Thus paramiko is faster for most users on these platf**orms.

**2. python-httplib2**
This package only helps to monitor the urls via ansible.

**3. python2-cryptography** 
This package will help you to validate and control all SSL certificates.

11. 9.	Now create your own environment directory named **dev**.
12. Create **ansible.cfg** file under dev directory using command
```
vim ansible.cfg 
```
**In ansible.cfg type the following : **
```
[defaults]
inventory=hosts  // hosts is the file name which contains the type of the srevers. You can give any name instead of hosts.

[privilege_escalation]
become=True
become_method=sudo
become_user=root
become_ask_pass=False
```
- The above part you will get through : **cat /etc/ansible/ansible.cfg**. This will give the sudo access to execute the ansible playbooks.
