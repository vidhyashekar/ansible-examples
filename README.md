# 📘 Ansible — Ready Guide

## ✅ What is Ansible?

**Ansible** is an open-source **IT automation and configuration management tool** used to automate:

- Server provisioning  
- Application deployment  
- Configuration management  
- Orchestration  

It works using **SSH** and does **NOT require agents** on target machines.

👉 It uses **YAML** language called **Playbooks**.

---

## 🎯 Why Do We Use Ansible?

### Problems Without Ansible

- Manual server setup takes time  
- Configuration drift occurs  
- Human errors are common  
- Difficult to manage multiple servers  

---

### Benefits of Ansible

- Agentless architecture  
- Easy to learn (YAML based)  
- Idempotent (runs safely multiple times)  
- Scalable automation  
- Fast deployment  
- Centralized control  

---

# ⚙️ Prerequisites

## 1️⃣ Install Ansible

### Update System
```
sudo apt update
```

### Install Ansible
```
sudo apt install ansible -y
```

### Verify Installation
```
ansible --version
```

## 2️⃣ Setup Ansible Server & Target EC2 Servers
Architecture

Ansible Control Node (Local Machine)
            |
            | SSH
            |
   ------------------------
   |                      |
EC2 Target 1        EC2 Target 2

## Setup Steps

Step 1: Generate SSH Key in Ansible server
```
ssh-keygen
```

Step 2: Copy id_rsa.pub Key to Target Servers .sss/authenticated_key

Step 3: Create Inventory File
Example: hosts
[web]
172.31.10.1
172.31.20.2

Test connectivity:
```
ansible web -i hosts -m ping
```

# 🚀 Ansible Ad-Hoc Commands (Simple Guide)

## ✅ What are Ad-Hoc Commands?

Ad-Hoc commands are **one-line commands** used to perform quick tasks on remote servers without creating a playbook.

They run directly from the terminal.

---

## 🎯 When to Use Ad-Hoc Commands

Use them for:

- Testing server connection
- Running quick commands
- Installing a package quickly
- Checking system info
- Restarting services

👉 Not used for complex automation.

---

## 🧠 Basic Syntax

```
ansible <host-group> -i <inventory-file> -m <module> -a "arguments"
```

``` 
ansible web -i hosts -a "uptime"
ansible web -m service -a "name=nginx state=started" --become
```

# 📘 Ansible Playbook (Simple Guide)

## ✅ What is an Ansible Playbook?

An **Ansible Playbook** is a YAML file used to automate tasks on remote servers.

It allows you to:

- Install software
- Configure servers
- Deploy applications
- Manage services

Playbooks are **reusable and organized automation scripts**.

---

## 🎯 When to Use Playbooks

Use playbooks when you need to:

- Automate multiple tasks
- Configure servers consistently
- Deploy applications
- Perform repeatable operations

---

## 🧠 Basic Structure of a Playbook

A playbook contains:

- **Hosts** → Target servers
- **Tasks** → Actions to perform
- **Modules** → Tools used to perform tasks

---

## 🧪 Simple Playbook Example

### 👉 Install Nginx on Servers

File: `install_nginx.yml`

```yaml
- name: Install Nginx Web Server
  hosts: web
  become: yes

  tasks:
    - name: Install nginx package
      apt:
        name: nginx
        state: present

    - name: Start nginx service
      service:
        name: nginx
        state: started
```

▶️ How to Run a Playbook
```
ansible-playbook -i inventory install_nginx.yml
```

# 📦 Ansible Galaxy Roles (Simple Guide)

## ✅ What is Ansible Galaxy?

**Ansible Galaxy** is an official repository where you can:

- Download ready-made Ansible roles
- Share your own roles
- Reuse automation created by others

It helps avoid writing automation from scratch.

---

## ✅ What is an Ansible Role?

A **Role** is a structured way of organizing playbooks into reusable components.

Roles make automation:

- Modular
- Reusable
- Easy to manage
- Scalable

---

## 🎯 When to Use Roles

Use roles when:

- Working on large projects
- Managing complex setups
- Reusing automation code
- Collaborating in teams

---

# 🧠 Install a Role from Galaxy

## Install a Role

```
ansible-galaxy install geerlingguy.nginx
```

## View Installed Roles
```
ansible-galaxy list
```

🛠️ Create Your Own Role

## Step 1: Create Role Structure
```
ansible-galaxy init nginx_role
```

This creates folders like:
nginx_role/
├── defaults
├── files
├── handlers
├── meta
├── tasks
├── templates
├── tests
└── vars

✏️ Add Tasks to Role
Edit:
roles/nginx_role/tasks/main.yml

Example:
```yml
- name: Install nginx
  apt:
    name: nginx
    state: present

- name: Start nginx
  service:
    name: nginx
    state: started
```
▶️ Use Role in Playbook
Create a playbook:
```yml
- name: Use nginx role
  hosts: web
  become: yes

  roles:
    - nginx_role
```
Run Playbook
```
ansible-playbook -i inventory.ini site.yml
```















