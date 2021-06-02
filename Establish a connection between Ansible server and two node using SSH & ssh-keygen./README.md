
# Establish a connection between Ansible server and two node using SSH & ssh-keygen


Ansible is an open-source automation engine that automates software provisioning, configuration management, and application deployment.

**Pro’s**

Push-based configuration management tool. <br>
Ansible is agentless. No need to install any services on nodes (client). <br>

It is secure due to its agentless capabilities and open SSH Security Features. <br>

Ansible does not need any System Administrator skills to install and use it. <br>

**Con’s**

With an insufficient user interface, Ansible tower is GUI-based but still in an early development stage. <br>
Cannot achieve full automation by ansible. Less limited support because it is a new tool. <br>


Establish a connection between the Ansible server and two-node using ssh and ssh-keygen. 

We can also read this simply as a connection between two VM’s using ssh. <br>


**Step-1=>** <br>
Create three instances one is for ansible server and two nodes i.e node1 and node2

Configurations:
![image001](https://user-images.githubusercontent.com/46487696/120502474-1954a380-c3e0-11eb-9b8a-cf6713bdba97.png)


![image003](https://user-images.githubusercontent.com/46487696/120502482-1a85d080-c3e0-11eb-890f-7b836cf9b5b4.png)

Ansible Server:

![image005](https://user-images.githubusercontent.com/46487696/120502483-1b1e6700-c3e0-11eb-87f5-13fbb9302656.png)

node1:

![image007](https://user-images.githubusercontent.com/46487696/120502486-1b1e6700-c3e0-11eb-8700-a0e59480ae34.png)

node2:
![image009](https://user-images.githubusercontent.com/46487696/120502488-1bb6fd80-c3e0-11eb-81df-f067bc0ed781.png)


**Step-2 =>**
go to Ansible server instances<br>
Download the below package which consists of Ansible server files<br>

```
wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum install epel-release-latest-7.noarch.rpm –y
yum update –y 
```

Also, need to download some extra packages :<br>

```yum install git python python-level python-pip openssl ansible –y```

To verify: Ansible --version

In ansible, there is a file named host file or Inventory file in which private IPs of the nodes are stored.<br>
create a group and add the IP’s of node follow below syntax<br>

 command: vi /etc/ansible/hosts     

```
[developers]
172.31.39.234
172.31.33.163
```

![image](https://user-images.githubusercontent.com/46487696/120503817-3d64b480-c3e1-11eb-857a-0fa059acc728.png)

Now, to activate this inventory we need to configure ansible.cfg file.<br>

Command: vi /etc/ansible/ansible.cfg

Uncomment the below configurations:
```
inventory = /etc/ansible/hosts
sudo-user = root
```
![image](https://user-images.githubusercontent.com/46487696/120503866-45bcef80-c3e1-11eb-8d48-6b9970bab476.png)

![image](https://user-images.githubusercontent.com/46487696/120503881-49507680-c3e1-11eb-9e90-4a1376319da5.png)

Also, still, we have not set up a link by which the nodes and server can’t communicate with each other.


**Step-3=>**<br>
Now, first, let’s try connecting Ansible server to node1 using ssh.<br>

![image](https://user-images.githubusercontent.com/46487696/120503919-52414800-c3e1-11eb-8424-0b1127961b2b.png)

It shows an error because we have not configured ssh.<br>
Remember any configuration will be done in the root user as it is the superuser and permissions are given by root to the user.<br>
The Below configuration will be done in all VMs i.e node1 and node2 and Ansible server.<br>
go to sshd_config present in Ansible server/node1/node2 and configure it.<br>

vi /etc/ssh/sshd_config<br>

this same configuration will be done on both node1 and node2.<br>

uncomment:

```
PermitRootLogin Yes
PasswordAuthentication yes
```
comment:

```PasswordAuthentication no```

![image](https://user-images.githubusercontent.com/46487696/120503958-5a998300-c3e1-11eb-9f19-3fbc5f919527.png)

![image](https://user-images.githubusercontent.com/46487696/120503973-5f5e3700-c3e1-11eb-9ced-bbaed483ace4.png)

After successful connection, the Ansible server can access node1 and node2 using which any update can be done on nodes using the server.


**Step-4=>** <br>

Now, we will create a user in each machine because we do not want to share root user credentials. Access your VMs using the putty tool. <br>
Note: must keep password same for each user of different machines to prevent errors. If we create a user with a different password in each machine it will show an error. So to prevent error we might create the same user with the same password on every machine<br>

For ansible server: create a user by name “ansible” and password: <samepasswd>

![image](https://user-images.githubusercontent.com/46487696/120504136-8452aa00-c3e1-11eb-93c8-5390b6b4260d.png)
![image](https://user-images.githubusercontent.com/46487696/120504162-8a488b00-c3e1-11eb-9820-a7888e22208b.png)
![image](https://user-images.githubusercontent.com/46487696/120505898-0db6ac00-c3e3-11eb-9b6b-66ec0a7949bc.png)


For node1: create a user by name “node1” and passwd:<samepasswd><br>
 
![image](https://user-images.githubusercontent.com/46487696/120504180-8e74a880-c3e1-11eb-8af7-83706c3ea7fc.png)

For node2: create a user by name “node2” and passwd: <samepasswd >

![image](https://user-images.githubusercontent.com/46487696/120504289-a3e9d280-c3e1-11eb-9ac4-29a4acabf938.png)

Since the user doesn’t have any root privileges it will say “ansible is not in the sudoers file” if we download any package or software inside the user.<br>
Go to a "sudoers" directory using the command: visudo<br>
Write the following to give the root privileges to all 3 ec2 machines i.e one ansible server and two nodes<br>
ansible ALL=(ALL) NOPASSWD: ALL<br>
// Here NOPASSWD: ALL means require no password when the user invokes sudo command<br>
  
for ansible:

![image](https://user-images.githubusercontent.com/46487696/120504393-b8c66600-c3e1-11eb-900d-2d18d45f24db.png)

for node1: here we have taken a user with the same username with the same password

![image](https://user-images.githubusercontent.com/46487696/120504434-c11ea100-c3e1-11eb-8877-60bfd09f17f0.png)

For node2:  Here we have taken a user with a different name but the same password

![image](https://user-images.githubusercontent.com/46487696/120504452-c54abe80-c3e1-11eb-9997-7a0bbe4515d7.png)




**Step-5=>**
As we will not work directly on root users because of security concerns,  we created a  user with root permissions in each machine.
go to Ansible server machine -> go to Ansible user using “su – ansible” command and invoke the other node using their private IP.
use command:
ssh <private IP address of node>

now it will ask for the password. Here what issue comes if a password is different of the user of node machine from the root it will show an error and if a password is same of every user in every machine no error will be there. Also, if user names are also the same then chances of errors are reduced up to maximum.<br>
if passwords are different, either type password of your server user or node user it will show an error.<br>

![image](https://user-images.githubusercontent.com/46487696/120504550-dd224280-c3e1-11eb-972b-8ed427f7071f.png)

Now, give the password as we keep the same password for each user. it will redirect you to the user of that password from that machine.<br>

server (172.31.38.62) to node1(172.31.42.50)

![image](https://user-images.githubusercontent.com/46487696/120504607-ead7c800-c3e1-11eb-891d-779fb68c4736.png)
![image](https://user-images.githubusercontent.com/46487696/120504622-edd2b880-c3e1-11eb-8e59-9ba5fde8b707.png)


server (172.31.38.62) to node2(172.31.32.33)

![image](https://user-images.githubusercontent.com/46487696/120504643-f2976c80-c3e1-11eb-8349-3d00683a01d4.png)

**Step-6=>**
Now when we access another machine we have to give the password again and again.<br>
for this, we can use ssh-keygen.<br>
Go to Ansible server machine and redirect to Ansible user and follow the below command.<br>

command:<br>
 
```
ssh-keygen
ls –a      //to see hidden directories<br>
cd .ssh/ 
ls
ssh-copy-id  
<username>@<Private IP>                             //node1
ssh-copy-id 
<username>@<Private IP>                             //node2

```
 
from server to node1

![image](https://user-images.githubusercontent.com/46487696/120504848-19ee3980-c3e2-11eb-8d6c-fee6eedd9a00.png)


from server to node2

![image](https://user-images.githubusercontent.com/46487696/120504871-1f4b8400-c3e2-11eb-83ba-2dde546cb529.png)


**Now** will try to log in to you your node. The below output shows successful login.

node2 [172-21-32-33]

![image](https://user-images.githubusercontent.com/46487696/120505072-46a25100-c3e2-11eb-9ea1-4044b782e8a0.png)


 node 1 [172-31-42-50]
 
![image](https://user-images.githubusercontent.com/46487696/120505098-4bff9b80-c3e2-11eb-805a-7d156f5c0010.png)
