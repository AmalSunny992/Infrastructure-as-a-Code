# Introduction to Ansible

## Objectives
- Understand what Ansible is and its purpose
- Learn about Ansible's architecture and how it operates

## What is Ansible?
Ansible is an open-source automation tool used for configuration management, application deployment, and task automation. It simplifies IT automation by automating repetitive tasks and ensuring that configurations and deployments are consistent across multiple environments. Ansible is known for its simplicity and ease of use, making it a popular choice for both small and large-scale automation tasks.

## Key Features of Ansible
**Agentless:** Ansible does not require any agent software to be installed on the managed nodes. It uses SSH for communication, making it easy to set up and manage.
**Declarative Language:** Ansible uses YAML (Yet Another Markup Language) for writing playbooks, which describe the desired state of the system.
**Idempotent:** Ansible ensures that applying the same configuration multiple times will have the same effect as applying it once, preventing unintended changes.
**Extensible:** Ansible supports a wide range of modules that can be extended to meet specific needs, and it integrates well with other tools and services.

## Ansible Architecture
Ansible's architecture consists of the following key components:

**1. Control Node**
The control node is the machine where Ansible is installed and from where Ansible tasks are executed. This is typically the administrator's machine or a dedicated server for running Ansible playbooks.

**2. Managed Nodes**
Managed nodes are the target machines that Ansible controls. These nodes can be physical servers, virtual machines, cloud instances, or network devices. Ansible uses SSH (Linux/Unix) or WinRM (Windows) to communicate with the managed nodes.

**3. Inventory**
The inventory is a file that contains information about the managed nodes. It lists the hosts and groups of hosts that Ansible will manage, along with any relevant variables. The inventory can be a simple static file, a dynamic inventory script, or an inventory plugin.

Example of a static inventory file (hosts):

```ini
[webservers]
webserver1.example.com
webserver2.example.com

[databases]
dbserver1.example.com
dbserver2.example.com
```

**4. Modules**
Modules are the units of work that Ansible executes on managed nodes. Ansible includes a wide range of built-in modules for tasks like package installation, file manipulation, user management, and more. You can also write custom modules if needed.

Example of using a module in a playbook:

```yaml
- name: Install Apache web server
  yum:
    name: httpd
    state: present
```

**5. Playbooks**
Playbooks are YAML files that define a series of tasks to be executed on managed nodes. Playbooks allow you to organize tasks into plays, which target specific hosts or groups of hosts and specify the tasks to be performed.

Example of a simple playbook (site.yml):

```yaml
---
- name: Configure web servers
  hosts: webservers
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
```

**6. Roles**
Roles are a way to organize playbooks and tasks into reusable components. A role typically contains tasks, handlers, variables, files, templates, and metadata that can be applied to hosts or groups of hosts.

Example directory structure of a role (roles/webserver):

```css
roles/
  webserver/
    tasks/
      main.yml
    handlers/
      main.yml
    templates/
      httpd.conf.j2
    files/
      index.html
    vars/
      main.yml
    meta/
      main.yml
```

**7. Plugins**
Plugins extend Ansible's core functionality. There are several types of plugins, including callback plugins, connection plugins, lookup plugins, and more. Plugins can be used to customize Ansible's behavior.

**8. Ansible Galaxy**
Ansible Galaxy is a community repository where users can share roles and collections. It provides a way to find and reuse existing roles and collections, saving time and effort.

## Summary
In this lesson, you learned what Ansible is, its key features, and its architecture. Ansible's simplicity, agentless nature, and powerful automation capabilities make it an essential tool for IT automation and DevOps practices.