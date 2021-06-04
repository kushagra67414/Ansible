# Ansible Playbook | handlers | loops


## Ansible Playbooks:

The playbook in Ansible is written in YAML format. It is a human-readable data serialization language and used for the configuration file. <br>
In the playbook, we can write codes consist of Var, tasks, handler, files, templates, and roles. <br>
Each playbook is composed of one or more modules in a list. <br>

The playbook is divided into various sections like: <br>
**target section:**  defines the host agents. <br>
**Variable section:** Define variables <br>
**task section:** list of module that we want to run in an order <br>

Let’s create a playbook:
A.
go to Ansible server and follow the below command

> vi target.yml

**code:**
```
---# Target Playbook
- hosts: developers
- user: ansible
- become: yes
- connection: ssh
- gather_facts: yes
```

To execute this playbook use following command:  <br>
> ansible-playbook <name-of-script.yml>

> ansible-playbook kushtarget.yml

![image](https://user-images.githubusercontent.com/46487696/120756818-62663e00-c52d-11eb-8b41-79e72d377280.png)

![image](https://user-images.githubusercontent.com/46487696/120756845-67c38880-c52d-11eb-81dd-c5f944cdeef6.png)


B.

> vi kushTask.yml

> ansible-playbook kushTask.yml

![image](https://user-images.githubusercontent.com/46487696/120756881-714cf080-c52d-11eb-8e04-41d23db6e9bf.png)
![image](https://user-images.githubusercontent.com/46487696/120756886-7447e100-c52d-11eb-89bb-be695a938fb5.png)


Node Output: <br>
![image](https://user-images.githubusercontent.com/46487696/120756893-7742d180-c52d-11eb-9bc7-c16a2e1af69d.png)




## Variables:

It is used to loop through a set of given values. <br>
variables are defined before tasks so that we can use them later. <br>

> vi vars.yml

> ansible-playbook vars.yml

![image](https://user-images.githubusercontent.com/46487696/120756940-888bde00-c52d-11eb-811a-0ad11fa3f764.png)


## Handlers Section:

Handlers are the same as tasks but they execute when called by another task.
the task contains a command called notify which indicates that another task is ready to be executed after the present task executes.

First, remove the httpd server from nodes

> sudo yum remove httpd

![image](https://user-images.githubusercontent.com/46487696/120756969-92addc80-c52d-11eb-882e-7f3dcc2fa7c7.png)


Second, a script for handlers :

> vi handlers.yml

![image](https://user-images.githubusercontent.com/46487696/120756996-9b9eae00-c52d-11eb-964f-3f9d08bac41d.png)


**Dry run:** It means we can check our script without the execution of the script.

Command: <br>

> ansible-playbook handlers.yml --check

![image](https://user-images.githubusercontent.com/46487696/120757061-b1ac6e80-c52d-11eb-9d0a-b0297a438e3a.png)

![image](https://user-images.githubusercontent.com/46487696/120757083-b7a24f80-c52d-11eb-89af-acc080686398.png)


Now run the playbook without Dry run: <br>
![image](https://user-images.githubusercontent.com/46487696/120757111-bcff9a00-c52d-11eb-97b0-5fbbd6a5909f.png)

![image](https://user-images.githubusercontent.com/46487696/120757119-bf61f400-c52d-11eb-8d3e-e8933c7b4dd6.png)


## Loops Sections:

As we know loop means repeating a task multiple times. Ansible loop includes changing ownership on several files or directories with the file module, creating multiple users with the user module, and repeating a polling step until a certain result is reached.<br>

Command:

> vi loop.yml

> ansible-playbook loop.yml

**Script:**
```
--- # My loop Playbook
- hosts: developers
  user: ansible
  become: yes
  connection: ssh
  gather_facts: yes
  tasks:
          - name: add a list of user
            user: name='{{item}}' state=present
            with_items:
                    - kushagra
                    - shivani
                    - devashish
                    - abhishek
```




![image](https://user-images.githubusercontent.com/46487696/120757240-e91b1b00-c52d-11eb-87da-b5c1927225d9.png)

![image](https://user-images.githubusercontent.com/46487696/120757247-ed473880-c52d-11eb-8920-8530324b3704.png)

![image](https://user-images.githubusercontent.com/46487696/120757259-f20bec80-c52d-11eb-83a6-8791b8043e81.png)



**Node output** <br>
![image](https://user-images.githubusercontent.com/46487696/120757292-ffc17200-c52d-11eb-8da8-00928007055a.png)
