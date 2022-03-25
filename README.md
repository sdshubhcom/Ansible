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
This is needed on the Ansible control machine to be reasonably efficient with connections. Thus paramiko is faster for most users on these platforms.

**2. python-httplib2**
This package only helps to monitor the urls via ansible.

**3. python2-cryptography** 
This package will help you to validate and control all SSL certificates.

11. 9.	Now create your own environment directory named **dev**.
12. Create **ansible.cfg** file under dev directory using command
```
vim ansible.cfg 
```
**In ansible.cfg type the following**
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

13. Now create a host file **vim hosts** add the ip address of the both the machines. If this directory is empty later if you list all the available servers 
    it will show an error because it wont be able to find any servers.

14.	Now to verify use the following commands
```
ansible all --list
```
```
ansible all -m ping
```
# First Yaml File
- This should be created in your own directory.
- Type vim first.yaml/yml

![image](https://user-images.githubusercontent.com/99954871/160097624-52f2ca5e-ab6c-41f2-b899-b1b94e6b8e9b.png)

- **Name**: you can give any name to its just the description what you are doing.
- **Hosts**: while execution it will go to ansible.cfg file and what file is mentioned in inventory it will go there and these will be check there.
- **Task**: it defines task that you will perfrom.
- **Yum**: it is the actual module name from which the server will be installed.
- **State**: it defines whether to remove, install, stop the server.

**To execute and check for the syntax:**

- **Executing**: ansible-playbook [file name].
- **Checking syntax**: ansible-playbook [file name] --syntax-check.
- **Simulation**: ansible-playbook [file name] -C. it will not actually run the playbook. but it will show what will be the output of the playbook.
- **Yum list httpd**: it will show the installed packeges.
- **Service httpd status**: it will show the status of the package.
- **For troubleshooting**: tailf /var/log/messages it will show the backend process of the tasks.

# Ansible Modules

- **ansible-doc -l**:  will show you the all the modules available.
- **ansible-doc -l | grep -i [package name]**:  It will list down the specific module that you required you just give the package name all related 
  modules will be displayed. Giving -i means it will be not case sensitive.
- **Ansible -doc [module name]**:  it will show all the example related to that package.

# Starting service after installing syntax

![image](https://user-images.githubusercontent.com/99954871/160098937-6873409c-e666-4595-a91b-394ad617860b.png)

- If you execute the same playbook multiple times it will not install the server multiple time sit will only check the whether the package is being 
  installed or not. There will no such ambiguity. 

# Write the one line command for installing the package
```
ansible all -m yum -a "name=httpd state=latest".
```
```
ansible all -m service -a “name=httpd state=stopped.”	
```

# Ansible Tags

- Tags are used for grouping of multiple tasks into one if you call that tag name then only that tasks which are associated with those tags are going to 
  be executed.
- Tags are denoted as : -t [tags names] or --tags [tags name].

**Use of tags**: 

![image](https://user-images.githubusercontent.com/99954871/160103068-dd511c4e-614f-4d4b-8a40-a3aa10304b9b.png)

**How to know the tags name without opening the file ?**

- ansible-playbook [file name] --list-tags
eg :

![image](https://user-images.githubusercontent.com/99954871/160104448-4204ec70-1255-4691-a602-88135bfdc71d.png)
 
It is showing all the tags available in the file.

# How to execute or allowing the specific task to execute without knowing what’s inside the file ?

- ansible-playbook [file name] --step
- what this command does is it will execute each task and ask you if you want to continue or not.

eg :

![image](https://user-images.githubusercontent.com/99954871/160104878-82ae251d-8a0f-49c6-8df4-7f78dfad0b70.png)

- It will ask for each step to execute or not. Difference between yes and continue is if you give yes  it will again ask for next step to execute or not 
  but if you give continue it won’t ask any further.
  




