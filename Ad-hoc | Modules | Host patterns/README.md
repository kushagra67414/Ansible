# Ad-hoc | Modules | Host patterns

node2 [172-21-32-33]
node1 [172-31-42-50]
server [172.31.38.62]

## Host Patterns =>

Here, we check numbers of nodes connected to our server in any pattern.

Commands:

ansible all --lists-hosts <br>
ansible <groupname> --lists-hosts<br>
ansible <groupname>[0] --lists-hosts<br>

where<br>
<groupname>[0] means first node<br>
<groupname>[1] means second node <br>
<groupname>[-1] means last node <br>
<groupname>[-2]  <br>
means second last node <br>
<groupname>1:4] means second node to fifth node <br>

![image](https://user-images.githubusercontent.com/46487696/120610282-a34c4d00-c470-11eb-8144-685f873b3b53.png)



## Ad-hoc | Modules | playbook in Ansible

Ad-hoc commands are simply Linux commands. Due to no idempotency, it executes the file again and again. Here, idempotency means the execution is done every time either it is already executed or not i.e. no overwrites of file. <br>
Module means single work done by a single command while the collection of modules execute are known as playbooks. <br>

**Ad-hoc:** <br>
They can be run individually to perform quick functions. <br>
These ad-hoc commands are not used for configuration management and deployment because commands are one-time usage. <br>
The Ansible ad-hoc commands use  /usr/bin/ansible the command-line tool to automate a single task. <br>


Commands:
  
ansible developers -a "ls"<br>
![image](https://user-images.githubusercontent.com/46487696/120610611-fb834f00-c470-11eb-992d-79c343ce16d1.png)

ansible all -a "ls" <br>
![image](https://user-images.githubusercontent.com/46487696/120610651-063de400-c471-11eb-9881-75bcd8dab597.png)

ansible all -a "touch kushagrabansal" <br>
![image](https://user-images.githubusercontent.com/46487696/120610714-1655c380-c471-11eb-95be-8bb6aecb3212.png)

node-1 <br>
![image](https://user-images.githubusercontent.com/46487696/120610730-1a81e100-c471-11eb-8be5-943d30bd71bf.png)

node-2 <br>
![image](https://user-images.githubusercontent.com/46487696/120610745-1d7cd180-c471-11eb-9060-5c6bc5ee0850.png)


ansible all -a "touch kushagrabansal" [If we run this ad-hoc command it will execute again i.e no idempotency] <br>
![image](https://user-images.githubusercontent.com/46487696/120610794-2a99c080-c471-11eb-9b6c-8a7a32968b89.png)


ansible developers  -a "ls -al" <br>
![image](https://user-images.githubusercontent.com/46487696/120610846-37b6af80-c471-11eb-9dfa-d29382cbcbec.png)


ansible developers -a "sudo yum install httpd -y" <br>
![image](https://user-images.githubusercontent.com/46487696/120610977-54eb7e00-c471-11eb-8640-b44dd26c7cfe.png) <br>
![image](https://user-images.githubusercontent.com/46487696/120610998-5a48c880-c471-11eb-8995-a96e8fadd253.png)

ansible developers -a " yum install httpd -y" <br>
![image](https://user-images.githubusercontent.com/46487696/120611048-66cd2100-c471-11eb-8de3-072681b33137.png)



ansible developers -a "sudo yum remove httpd -y"  <br>
OR <br>
ansible developers -ba "yum remove httpd -y" <br>
![image](https://user-images.githubusercontent.com/46487696/120611096-6e8cc580-c471-11eb-9c93-1ebb8a22a830.png)




  
  

## Ansible Modules:

Ansible ships with several modules (called ‘module library’) that can be executed directly on remote hosts or through ‘playbooks’. <br>
Also, the library of modules can reside on any machine and no servers, daemon, or database required. <br>
Ansible modules are stored in an inventory file and the default location is /etc/ansible/hosts <br>
Idempotency can be achieve by modules. <br>



**Commands of Ansible modules:** <br>

ansible <group-name> -b –m yum –a “pkg = httpd state = present” <br>
// to uninstall state = absent, to update state=latest, to start the service state=started  <br>
  
![image](https://user-images.githubusercontent.com/46487696/120611354-bb709c00-c471-11eb-930d-eeb798d8d69b.png)

![image](https://user-images.githubusercontent.com/46487696/120611365-be6b8c80-c471-11eb-8c87-dff769385f37.png)

If run this command again:

![image](https://user-images.githubusercontent.com/46487696/120611400-c62b3100-c471-11eb-8936-ffc8b6af96b4.png)


ansible developers -b -m user -a "name=kush" <br>
// create a user by name kush <br>

![image](https://user-images.githubusercontent.com/46487696/120611438-d3482000-c471-11eb-9303-3b8958c2a3ba.png)


ansible <group-name> -b –m user –a “name=kush state=absent” <br>
// to delete the user <br>

![image](https://user-images.githubusercontent.com/46487696/120611513-e6f38680-c471-11eb-86fe-9f71143471e0.png)


ansible <group-name> -b -m copy -a “src=file.txt dest=/tmp” <br>
// copying the file from server to nodes present in group <br>
![image](https://user-images.githubusercontent.com/46487696/120611538-ee1a9480-c471-11eb-9577-38d7addd0d4c.png)

![image](https://user-images.githubusercontent.com/46487696/120611551-f1158500-c471-11eb-80b1-7edbe81e4eca.png)


Idopotency can be achieved by module i.e setup module

ansible developers -m setup <br>
// to check all the present configurations of the node <br>
![image](https://user-images.githubusercontent.com/46487696/120611716-1609f800-c472-11eb-9e52-d0451853f400.png)

![image](https://user-images.githubusercontent.com/46487696/120611759-215d2380-c472-11eb-80bc-334030e5a15b.png)


ansible developers -m setup -a "filter=*ipv4*"
// To check configurations related to ipv4
  
![image](https://user-images.githubusercontent.com/46487696/120611791-291cc800-c472-11eb-8597-3c00681cbcb3.png)

