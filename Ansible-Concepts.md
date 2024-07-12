# Advanced Ansible Concepts

## Objectives
- Understand Ansible roles and how to use them
- Learn about Ansible Galaxy and how to leverage it for reusable content

## Ansible Roles

### What is an Ansible Role?
An Ansible role is a way to organize playbooks and tasks into reusable components. Roles allow you to break down complex playbooks into smaller, more manageable pieces. Each role has its own directory structure and can include tasks, handlers, variables, templates, files, and metadata.

### Directory Structure of a Role
**A typical role directory structure looks like this:**

```css
roles/
  myrole/
    tasks/
      main.yml
    handlers/
      main.yml
    templates/
      template.j2
    files/
      file.txt
    vars/
      main.yml
    defaults/
      main.yml
    meta/
      main.yml
```

### Creating a Role

- Create the Role Directory Structure:
    - Use the ansible-galaxy command to create the role structure.

```sh
ansible-galaxy init myrole
```

- Define Tasks:
    - Edit the tasks/main.yml file to define the tasks for the role.

```yaml
# roles/myrole/tasks/main.yml
---
- name: Install Apache
  yum:
    name: httpd
    state: present

- name: Start Apache service
  service:
    name: httpd
    state: started
    enabled: true
```

- Define Variables:
    - Variables can be defined in the vars/main.yml or defaults/main.yml file.

```yaml
# roles/myrole/vars/main.yml
---
apache_package: httpd
```

- Use the Role in a Playbook:
    - Reference the role in your playbook.

```yaml
# site.yml
---
- name: Configure web servers
  hosts: webservers
  become: yes
  roles:
    - myrole
```

- Run the Playbook:
    - Execute the playbook to apply the role.

```sh
ansible-playbook -i hosts site.yml
```

## Ansible Galaxy

### What is Ansible Galaxy?
Ansible Galaxy is a community repository for sharing Ansible roles. It allows you to find, download, and use roles created by others, making it easier to manage and reuse configurations.

### Using Ansible Galaxy
Search for Roles:
Visit Ansible Galaxy to search for roles that meet your needs.

Install a Role:
Use the ansible-galaxy command to install a role from Ansible Galaxy.

```sh
ansible-galaxy install username.rolename
```

- Use the Installed Role:
    - Reference the installed role in your playbook. Installed roles are typically placed in the roles/ directory.

```yaml
# site.yml
---
- name: Configure web servers
  hosts: webservers
  become: yes
  roles:
    - username.rolename
```

- Create and Share Your Own Roles:
    - You can create your own roles and share them on Ansible Galaxy. Follow the guidelines on the Ansible Galaxy website to publish your roles.

## Example: Using a Role from Ansible Galaxy
1. Install the Role:
    - For example, to install the geerlingguy.apache role:

```sh
ansible-galaxy install geerlingguy.apache
```

2. Reference the Role in Your Playbook:

```yaml
# site.yml
---
- name: Configure web servers
  hosts: webservers
  become: yes
  roles:
    - geerlingguy.apache
```

3. Run the Playbook:
    - Execute the playbook to apply the role.

```sh
ansible-playbook -i hosts site.yml
```

## Summary
In this lesson, you learned about Ansible roles and how to use them to organize playbooks into reusable components. You also learned about Ansible Galaxy, a community repository for sharing and reusing roles. By leveraging roles and Ansible Galaxy, you can simplify your Ansible configurations and improve reusability and maintainability.