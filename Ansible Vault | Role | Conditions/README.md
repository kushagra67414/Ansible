# Ansible Vault & Roles & Conditions

## Condition:

Conditions mean working on different scenarios different conditions are required to execute those conditions. 

In ansible YAML script we use the “when” command to give the condition.

Let’s take an example of a script:

Script:
```
--- # My Condition Playbook
- hosts: developers
  user: ansible
  become: yes
  connection: ssh
  gather_facts: yes
  tasks:
          - name: installing apache2 on debian
            command: apt-get -y install apache2
            when: ansible_os_family == "Debian"
          - name: installing httpd on linux
            command: yum install httpd -y
            when: ansible_os_family == "RedHat"

```

**Commands:**

> vi conditions.yml  // write the above YAML script

> ansible-playbook conditions.yml

![image](https://user-images.githubusercontent.com/46487696/120824566-3622df80-c576-11eb-9509-e0485fcd1d84.png)

![image](https://user-images.githubusercontent.com/46487696/120824583-39b66680-c576-11eb-9836-227bc259a722.png)


**Node1 Output:**

![image](https://user-images.githubusercontent.com/46487696/120824641-4470fb80-c576-11eb-8d99-1a4f7664fc34.png)



## Ansible Vault =>

It Keeps the sensitive data like playbooks in an encrypted manner.

> ansible-vault create vault.yml      // create a script in an encrypt mode

> ansible-vault edit vault.yml        // to edit in an existing encrypted script

![image](https://user-images.githubusercontent.com/46487696/120824990-944fc280-c576-11eb-8825-7855501e39d3.png)

Editing encrypted file using VI editor

> vault.yml                   // script output will be in an encrypted mode


To add an existing script to ansible-vault[i.e. In an encryption mode]

> ansible-vault encrypt <file-name.yml>

> ansible-vault encrypt target.yml

![image](https://user-images.githubusercontent.com/46487696/120825130-b2b5be00-c576-11eb-93c8-ea1cae4b801e.png)

To decrypt yml script
ansible-vault decrypt target.yml

![image](https://user-images.githubusercontent.com/46487696/120825144-b5b0ae80-c576-11eb-995d-59bb63f99a2a.png)


## Ansible Roles =>

We can use two techniques for reusing a set of tasks: include and roles.<br>
Roles are better for organizing tasks and encapsulating the data needed to accomplish those tasks.<br>

Types of roles:<br>
* Default
* files
* Handlers
* Meta
* Templates
* Tasks
* Vars (variables)

In roles, playbooks can be organized in a directory structure.<br>
Roles are important because adding more and more functionally to any file/script will make it more complex to handle.<br>

**Types of Roles:**<br>
**Default:** It stores the data about role/application default variables. example: port number.<br>
**Files:** It contains files needed to be transferred to the remote VM <br>
**Handler:** They are triggered from another task. they are also a task. <br>
**Meta:** The directory contains the file that establishes role dependencies. eg: name of the author, supported platform, and so on. <br>
**Tasks:** Contains all the tasks that are present in the playbooks. <br>
**Vars:** variables stored in this directory and can be used further for configurations <br>

![image](https://user-images.githubusercontent.com/46487696/120825477-0d4f1a00-c577-11eb-9e4f-4558ec22a434.png)


Let’s understand the structure of roles.<br>
Here playbook is the directory and master.yml and roles are the file and directory present inside the Playbook directory.<br>
targets[host name, connection type, user name] and roles are defined inside the master.yml and the roles information is stored inside the roles directory. master.yml and roles are at the same level or say present inside the same directory.<br>
In roles, different processes like Task, Handlers, var, and so on are executed one by one.<br>

Present tree structure on a Linux machine:

![image](https://user-images.githubusercontent.com/46487696/120825499-14762800-c577-11eb-8922-634b227f33d8.png)

Let’s create a role and run it from the playbook by YAML script. <br>.
Create parent directory means directory inside the directory. ‘-p’ flag is used to perform such task. <br>

> mkdir -p playbook/roles/webserver/tasks

![image](https://user-images.githubusercontent.com/46487696/120825567-25269e00-c577-11eb-80ef-3c4103c11cc8.png)

Create a main.yml file inside the tasks directory and master.yml at the same level of the roles directory.

**Commands:**
> cd playbook

> tree

> touch roles/webserver/tasks/main.yml        // role with  a name webserver has been created

> tree

> touch master.yml

> tree

![image](https://user-images.githubusercontent.com/46487696/120825686-41c2d600-c577-11eb-8df5-e5122f82dbb8.png)


Create master.yml inside the playbook directory [at the same level of roles directory].

**Script of master.yml:**
```
--- # The master script
- hosts: developers
  user: ansible
  become: yes
  connection: ssh
  roles:
          - webserver
```

Now here we have defined a role name webserver. let us create a process/task inside this role. <br>

> vi roles/webserver/tasks/main.yml

**Script for main.yml:**

```
- name: install apache on RedHat
  yum: pkg=httpd state=latest
```

Run the master.yml script to execute the playbook

> ansible-playbook master.yml

![image](https://user-images.githubusercontent.com/46487696/120825909-7df63680-c577-11eb-84ac-e0f436eed391.png)

![image](https://user-images.githubusercontent.com/46487696/120825927-82baea80-c577-11eb-9ff9-a67957ce51eb.png)




