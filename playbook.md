| Author       | Created on   | Version | Last updated by | Last edited on |
| :----------- | :----------- | :------ | :-------------- | :------------- |
| Hifza  | 2025-07-18   | 1.0     | Hifza     | 2025-07-18     |

# Ansible Playbook Documentation

A comprehensive guide and template for creating, understanding, and maintaining Ansible Playbooks—a YAML-based automation language for IT infrastructure.

---

## Table of Contents

1. [Introduction](#introduction)
2. [Why Use Ansible Playbooks?](#why-use-ansible-playbooks)
3. [Purpose](#purpose)
4. [Key Features](#key-features)
5. [Getting Started](#getting-started)
    - [Pre-requisites](#pre-requisites)
    - [Software Overview](#software-overview)
    - [System Requirements](#system-requirements)
    - [Important Ports](#important-ports)
6. [Dependencies](#dependencies)
    - [Run-time Dependency](#run-time-dependency)
    - [Other Dependency](#other-dependency)
7. [How to Setup/Install Ansible](#how-to-setupinstall-ansible)
    - [Step-by-step Installation](#step-by-step-installation-instruction)
8. [How to Create and Run an Ansible Playbook](#how-to-create-and-run-an-ansible-playbook)
    - [Step 1: Create Inventory File](#step-1-create-inventory-file)
    - [Step 2: Write Your Playbook](#step-2-write-your-playbook)
    - [Step 3: Run Your Playbook](#step-3-run-your-playbook)
    - [Step 4: Verify Results](#step-4-verify-results)
9. [Configuration](#configuration)
10. [Maintenance](#maintenance)
11. [Monitoring](#monitoring)
12. [Disaster Recovery](#disaster-recovery)
13. [High Availability](#high-availability)
14. [Troubleshooting](#troubleshooting)
15. [FAQs](#faqs)
16. [Contact Information](#contact-information)
17. [References](#references)
18. [Sample Playbook](#sample-playbook)
19. [Sample Execution Report](#sample-execution-report)
20. [Key Takeaways](#key-takeaways)

---

## Introduction

**What is an Ansible Playbook?**

An Ansible Playbook is a YAML file that automates IT tasks across multiple systems. It allows you to configure servers, deploy software, orchestrate workflows, and enforce desired states in a repeatable and human-readable way.

---

## Why Use Ansible Playbooks?

**Why are Ansible Playbooks important and widely adopted?**

- **Automation:** Eliminates manual, error-prone, and repetitive tasks by automating infrastructure and application management.
- **Consistency:** Ensures all systems are configured the same way every time, reducing configuration drift and human error.
- **Idempotency:** Playbooks are designed to be safely re-run; they only make changes when necessary.
- **Speed and Efficiency:** Enables rapid deployment and scaling of infrastructure or applications.
- **Version Control:** Playbooks are just code—store them in git for auditability, rollback, and collaboration.
- **Agentless Architecture:** No need to install agents on managed nodes; uses standard SSH.
- **Human Readable:** YAML syntax makes it easy for anyone to read, understand, and modify playbooks.
- **Extensible and Reusable:** Roles and modules can be shared across projects and teams.
- **Cloud-ready:** Integrates with AWS, Azure, GCP, and more.

---

## Purpose

Ansible Playbooks can be used for:

- Automating server setup and configuration
- Application deployments and updates
- Consistent environment provisioning
- Security and compliance enforcement
- Orchestrating multi-tier workflows

---

## Key Features

- **Automation:** Reduces manual, repetitive tasks
- **Idempotency:** Safe to re-run; only changes what’s necessary
- **Scalability:** Manages one to thousands of servers
- **Declarative Syntax:** Describes desired state, not step-by-step scripts
- **Reusability:** Modular roles and tasks, easy to share
- **Extensibility:** Supports custom modules, plugins, and integrations
- **Transparency:** Human-readable YAML, clear logs and reports

---

## Getting Started

### Pre-requisites

| License Type | Description                                      | Commercial Use | Open Source                                   |
| :----------- | :----------------------------------------------- | :------------- | :--------------------------------------------- |
| Open Source  | Ansible is free to use and modify (GPLv3)        | Yes            | Yes                                            |

### Software Overview

| Software | Version   |
| :------- | :-------- |
| Ansible  | 2.15+     |

### System Requirements

| Requirement             | Minimum               | Recommendation        |
| :---------------------- | :-------------------- | :-------------------- |
| Processor/Instance Type | Dual-Core/T2.medium   | Quad-Core or better   |
| RAM                     | 2 GB or Higher        | 4 GB or Higher        |
| Disk Space (ROM)        | 5 GB or Higher        | 20 GB or Higher       |
| OS Required             | Linux (Ubuntu/CentOS) | Latest LTS Preferred  |
| Python                  | 3.8 or Higher         | 3.10 or Higher        |

### Important Ports

| Port | Description                                                                      |
| :--- | :------------------------------------------------------------------------------- |
| 22   | **SSH** – Required for Ansible to connect to managed nodes. (Default SSH port)   |
| 443  | **HTTPS** – Used for downloading roles/modules from Ansible Galaxy (optional)    |

**Note:**  
- Port 22 must be open on all managed servers for remote SSH access.
- You may need to adjust firewall settings or security groups (e.g., AWS EC2) to allow access.

---

## Dependencies

### Run-time Dependency

| Run-time Dependency | Version      | Description                                 |
| :------------------ | :----------- | :------------------------------------------ |
| Python              | 3.8+         | Required by Ansible                         |
| sshpass             | latest       | For password-based SSH authentication       |

### Other Dependency

| Other Dependency | Version | Description                      |
| :--------------- | :------ | :------------------------------- |
| ansible-lint     | latest  | For linting and validating YAML  |

---

## How to Setup/Install Ansible

### Step-by-step Installation Instruction

**On Ubuntu/Debian:**

```bash
sudo apt update
sudo apt install -y ansible python3-pip
```

**On CentOS/RHEL:**

```bash
sudo yum install -y epel-release
sudo yum install -y ansible python3-pip
```

**Verify Installation:**

```bash
ansible --version
```

---

## How to Create and Run an Ansible Playbook

### Step 1: Create Inventory File

The inventory file lists all the hosts or groups you want to manage.  
**Example (`hosts.ini`):**

```ini
[webservers]
web1 ansible_host=192.168.1.10 ansible_port=22 ansible_user=ubuntu
web2 ansible_host=192.168.1.11 ansible_port=22 ansible_user=ubuntu

[dbservers]
db1 ansible_host=192.168.1.20 ansible_port=22 ansible_user=ubuntu
```
- `ansible_host`: The IP or DNS of the managed node.
- `ansible_port`: SSH port (default 22).
- `ansible_user`: SSH username.  
- You can also specify SSH keys and become methods.

### Step 2: Write Your Playbook

Create a YAML file (e.g., `site.yml`) describing the steps you want Ansible to perform.

**Example (`site.yml`):**
```yaml
---
- name: Configure Web Servers
  hosts: webservers
  become: yes
  tasks:
    - name: Install NGINX
      apt:
        name: nginx
        state: present
    - name: Start NGINX
      service:
        name: nginx
        state: started
```

### Step 3: Run Your Playbook

Use the `ansible-playbook` command to execute your playbook on the defined inventory:

```bash
ansible-playbook -i hosts.ini site.yml
```
- The `-i` flag specifies your inventory file.

### Step 4: Verify Results

- Check terminal output for success (`ok`) and changes (`changed`).
- Log into your servers to ensure software has been installed/configured as expected.
- Use Ansible ad-hoc commands for status checks:
    ```bash
    ansible all -m ping -i hosts.ini
    ```

---

## Configuration

- Create an inventory file (e.g., `hosts.ini`) listing target systems.
- Write your playbooks in YAML (e.g., `site.yml`).
- Use `group_vars/` or `host_vars/` directories for variable management.
- Organize reusable roles in the `roles/` directory.

**Example Directory Structure:**

```
project/
├── hosts.ini
├── site.yml
├── group_vars/
│   └── all.yml
├── roles/
│   └── webserver/
│       ├── tasks/
│       └── templates/
└── README.md
```
- Place SSH keys in the appropriate user home directory (`~/.ssh/authorized_keys`) or use `--ask-pass`/`--private-key` in the command.

---

## Maintenance

- **Update Ansible:**
    ```bash
    sudo apt update && sudo apt upgrade ansible
    ```
- **Upgrade with pip:**
    ```bash
    pip3 install --upgrade ansible
    ```
- **Restart managed service via playbook:**
    ```yaml
    - name: Restart web servers
      hosts: webservers
      tasks:
        - name: Restart nginx
          service:
            name: nginx
            state: restarted
    ```

---

## Monitoring

- Check SSH connectivity to all nodes:
    ```bash
    ansible all -m ping -i hosts.ini
    ```
- Run playbook:
    ```bash
    ansible-playbook site.yml -i hosts.ini
    ```
- View logs (if enabled):
    ```bash
    tail -f /var/log/ansible.log
    ```
- Check SSH service status on managed nodes:
    ```bash
    sudo systemctl status ssh
    ```
- Troubleshoot SSH login:
    ```bash
    ssh -i /path/to/key.pem ubuntu@<host_ip> -p 22
    ```

---

## Disaster Recovery

- **Version Control:** Store playbooks and inventory in git repositories.
- **Backups:** Regularly back up critical files and Ansible Vault secrets.
- **Rollback:** Use versioned playbooks and git tags for safe rollback.
- **Idempotency:** Rerun playbooks to restore systems to the desired state.
- **SSH Key Management:** Ensure backup and rotation of SSH keys for secure access.

---

## High Availability

- Use multiple Ansible control nodes for redundancy (advanced setups).
- Use Ansible Tower/AWX for enterprise features, including HA.
- Apply playbooks in batches (serial/batch) to avoid downtime.
- Use handlers for controlled, conditional restarts.
- Monitor SSH port 22 and ensure redundancy in network access.

---

## Troubleshooting

Common issues and solutions:

- **SSH Authentication Failures:**  
  - Ensure hosts are reachable on port 22.
  - Correct SSH keys or passwords are used.
  - User has appropriate permissions.
- **YAML Syntax Errors:**  
  - Validate playbooks with `ansible-lint` or a YAML linter.
- **Module Not Found:**  
  - Install missing dependencies with pip or apt.
- **Permission Denied:**  
  - Use `become: yes` to run tasks as root or another user.
- **Unexpected "changed" status:**  
  - Review task output/logs for details.
- **Firewall Issues:**  
  - Confirm port 22 is open on all managed nodes and not blocked by firewalls or security groups.

---

## FAQs

| Question | Answer |
| :------- | :----- |
| Is Ansible free to use? | Yes, open-source under GPLv3. |
| Can Ansible manage Windows servers? | Yes, with additional setup. |
| Do managed nodes need agents? | No, Ansible is agentless. |
| Can Ansible be used with cloud platforms? | Yes, modules for AWS, Azure, GCP, etc. |
| Which port must be open? | Port 22 (SSH) must be open for Ansible to connect. |

---

## Contact Information

| Name        | Email address      |
| :---------- | :---------------- |
| Hifza | hifza6907@gmail.com  |

---

## References

| Links | Descriptions |
| :---- | :----------- |
| https://docs.ansible.com/ansible/latest/user_guide/playbooks.html | Official Ansible Playbook Documentation |
| https://galaxy.ansible.com/ | Community Contributed Roles |
| https://github.com/ansible/ansible-examples | Example Playbooks |
| https://www.jenkins.io/doc/book/installing/linux/#debianubuntu | Format inspiration |
| https://amplifi.com/user-guide/FAQs.html | For FAQ/Table of Contents structure |
| https://thecontentauthority.com/blog/introduction-vs-overview | Difference between Overview & Intro |

---

## Sample Playbook

```yaml
---
- name: Configure Web Servers
  hosts: webservers
  become: yes
  vars:
    http_port: 80
  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: present
    - name: Copy Apache config
      template:
        src: apache.conf.j2
        dest: /etc/apache2/apache2.conf
      notify: Restart Apache
  handlers:
    - name: Restart Apache
      service:
        name: apache2
        state: restarted
```

---

## Sample Execution Report

| Playbook         | Execution Time     | Target Hosts          |
| :--------------- | :---------------- | :-------------------- |
| site.yml         | 2025-07-18 10:27  | web1, web2            |

**Playbook Summary:**

- Gathering facts
- Installing Apache
- Deploying configuration
- Restarting service

**Execution Output:**

```plaintext
ok: [web1]
ok: [web2]
changed: [web1]
changed: [web2]
changed: [web1]
changed: [web2]
changed: [web1]
changed: [web2]
```

**Play Recap:**

| Host  | Status    | Changed | Failed |
| :-----| :-------- | :------ | :----- |
| web1  | ✅ Success| 3       | 0      |
| web2  | ✅ Success| 3       | 0      |

---

## Key Takeaways

- Ansible Playbooks are clear, powerful, and scalable for IT automation.
- Idempotency ensures safe, repeatable usage.
- Troubleshoot with clear logs and outputs.
- Use roles, variables, and handlers for maintainable automation.
- **Port 22 (SSH)** is critical for all Ansible operations—ensure it's open!

---

**Documentation End**  
[Hifza] | [hifza6907@gmail.com] | 
