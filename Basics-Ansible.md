# Ansible Basics

## Objectives
- Learn how to write Ansible playbooks
- Understand how to manage Ansible inventory

## Writing Ansible Playbooks

### What is a Playbook?
Ansible playbooks are YAML files that define a series of tasks to be executed on managed nodes. Playbooks allow you to organize tasks into plays, which target specific hosts or groups of hosts and specify the tasks to be performed. Playbooks are the core of Ansible's configuration, deployment, and orchestration language.

### Structure of a Playbook
A playbook is composed of one or more plays. Each play targets a group of hosts and defines a set of tasks to be executed.

**Example of a simple playbook (site.yml):**

```yaml
---
- name: Configure web servers
  hosts: webservers
  become: yes
  tasks:
    - name: Install Apache
      yum:
        name: httpd
        state: present

    - name: Start Apache service
      service:
        name: httpd
        state: started
        enabled: true

- name: Configure database servers
  hosts: databases
  become: yes
  tasks:
    - name: Install MySQL
      yum:
        name: mariadb-server
        state: present

    - name: Start MySQL service
      service:
        name: mariadb
        state: started
        enabled: true
```
### Writing Your First Playbook

1. Create a Playbook File:
    - Create a file named site.yml in your project directory.

2. Define the Hosts:
    - Specify the hosts or groups of hosts that the play will target.

```yaml
- name: Configure web servers
  hosts: webservers
  become: yes
  tasks:
Add Tasks:
```

3. Define the tasks that will be executed on the target hosts.

```yaml
- name: Install Apache
  yum:
    name: httpd
    state: present

- name: Start Apache service
  service:
    name: httpd
    state: started
    enabled: true
Run the Playbook:
```

4. Use the ansible-playbook command to run your playbook.

```sh
ansible-playbook -i hosts site.yml
```

## Managing Ansible Inventory
### What is an Inventory?
The inventory is a file that contains information about the managed nodes. It lists the hosts and groups of hosts that Ansible will manage, along with any relevant variables. The inventory can be a simple static file, a dynamic inventory script, or an inventory plugin.

### Structure of an Inventory File
An inventory file can be in INI or YAML format. The simplest form of an inventory file is a list of hostnames or IP addresses.

**Example of a Static Inventory File (INI format)**

```ini
[webservers]
webserver1.example.com
webserver2.example.com

[databases]
dbserver1.example.com
dbserver2.example.com
```

**Example of a Static Inventory File (YAML format)**

```yaml
all:
  hosts:
    webserver1.example.com:
    webserver2.example.com:
    dbserver1.example.com:
    dbserver2.example.com:
  children:
    webservers:
      hosts:
        webserver1.example.com:
        webserver2.example.com:
    databases:
      hosts:
        dbserver1.example.com:
        dbserver2.example.com:
```

### Managing Inventory Variables
You can define variables in the inventory file to customize the behavior of your playbooks.

**Example of an Inventory File with Variables (INI format)**

```ini
[webservers]
webserver1.example.com ansible_user=admin ansible_port=2222
webserver2.example.com ansible_user=admin ansible_port=2222

[databases]
dbserver1.example.com ansible_user=root ansible_become=true
dbserver2.example.com ansible_user=root ansible_become=true
```

**Example of an Inventory File with Variables (YAML format)**
```yaml
all:
  hosts:
    webserver1.example.com:
      ansible_user: admin
      ansible_port: 2222
    webserver2.example.com:
      ansible_user: admin
      ansible_port: 2222
    dbserver1.example.com:
      ansible_user: root
      ansible_become: true
    dbserver2.example.com:
      ansible_user: root
      ansible_become: true
  children:
    webservers:
      hosts:
        webserver1.example.com:
        webserver2.example.com:
    databases:
      hosts:
        dbserver1.example.com:
        dbserver2.example.com:
```

### Dynamic Inventory
Dynamic inventory allows you to generate inventory data on the fly. This is useful for environments that change frequently, such as cloud environments. Ansible supports dynamic inventory scripts and plugins.

**Example of Using a Dynamic Inventory Script**

1. Create a Dynamic Inventory Script:

```sh
#!/bin/bash
cat <<EOF
{
  "webservers": {
    "hosts": ["webserver1.example.com", "webserver2.example.com"]
  },
  "databases": {
    "hosts": ["dbserver1.example.com", "dbserver2.example.com"]
  }
}
EOF
```

2. Make the Script Executable:

```sh
chmod +x dynamic_inventory.sh
```

3. Run the Playbook with Dynamic Inventory:

```sh
ansible-playbook -i dynamic_inventory.sh site.yml
```

## Summary
In this lesson, you learned how to write Ansible playbooks and manage Ansible inventory. Playbooks allow you to define tasks that will be executed on managed nodes, while the inventory defines the hosts and groups of hosts that Ansible will manage. With this knowledge, you can start automating your infrastructure and deployments using Ansible.