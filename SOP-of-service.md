| Author | Created on | Version | Last updated by | Last edited on |
| :----- | :--------- | :------ | :-------------- | :------------- |
| Hifza    | 17-07-25   | version 1 | Hifza           |   18-07-25     |

# Standard Operating Procedure (SOP): Linux Service Management with `systemctl`

---

## Table of Contents

- [1. Purpose](#1-purpose)
- [2. Scope](#2-scope)
- [3. Prerequisites](#3-prerequisites)
- [4. Step-by-Step Procedures](#4-step-by-step-procedures)
  - [4.1. Installing a Service](#41-installing-a-service)
  - [4.2. Managing Services](#42-managing-services)
  - [4.3. Autostart Configuration](#43-autostart-configuration)
  - [4.4. Advanced Service Control](#44-advanced-service-control)
  - [4.5. Troubleshooting & Logs](#45-troubleshooting--logs)
- [5. Example Workflow](#5-example-workflow)
- [6. Best Practices](#6-best-practices)
- [7. References](#7-references)
- [8. Document Control](#8-document-control)
- [Quick Links](#quick-links)

---

## 1. Purpose

This document provides detailed, step-by-step instructions for **installing, managing, and troubleshooting Linux services using `systemctl`**. It is designed to be self-sufficient for all experience levels, allowing users to follow commands without additional guidance.

---

## 2. Scope

This SOP applies to **systemd-based Linux distributions** such as **RHEL, CentOS, Ubuntu, Debian**, and similar.

---

## 3. Prerequisites

- Terminal access (**sudo** or **root** privileges required)
- Internet connection (required for installing new services)
- Basic familiarity with Linux command-line operations

---

## 4. Step-by-Step Procedures

### 4.1. Installing a Service

> **Example Services:** Nginx, Apache, Docker

<details>
<summary><strong>Debian/Ubuntu (APT)</strong></summary>

```bash
sudo apt update
sudo apt install <package_name> -y
# Example:
sudo apt install nginx -y
```
</details>

<details>
<summary><strong>RHEL/CentOS (YUM/DNF)</strong></summary>

```bash
sudo yum install <package_name> -y
# OR (for newer systems)
sudo dnf install <package_name> -y
# Example:
sudo dnf install httpd -y
```
</details>

**Verify Installation:**
```bash
which <service_name>
# Example:
which nginx
```
If the installation is successful, the command will show the path to the service executable. If not, re-check the installation step or package name.

---

### 4.2. Managing Services

| **Action**   | **Command**                          | **Example**                    |
|--------------|--------------------------------------|--------------------------------|
| Start        | `sudo systemctl start <service>`     | `sudo systemctl start nginx`   |
| Stop         | `sudo systemctl stop <service>`      | `sudo systemctl stop apache2`  |
| Restart      | `sudo systemctl restart <service>`   | `sudo systemctl restart sshd`  |
| Reload       | `sudo systemctl reload <service>`    | `sudo systemctl reload nginx`  |
| Check Status | `sudo systemctl status <service>`    | `sudo systemctl status docker` |

**Note:**  
- Use `reload` if the service supports reloading configuration without a full restart (safer for production).
- Always check status after making changes.

---

### 4.3. Autostart Configuration

| **Action**         | **Command**                         | **Example**                     |
|--------------------|-------------------------------------|---------------------------------|
| Enable at Boot     | `sudo systemctl enable <service>`   | `sudo systemctl enable mysql`   |
| Disable at Boot    | `sudo systemctl disable <service>`  | `sudo systemctl disable ufw`    |
| Check if Enabled   | `systemctl is-enabled <service>`    | `systemctl is-enabled cron`     |

---

### 4.4. Advanced Service Control

| **Action**         | **Command**                                   | **Example**                                             |
|--------------------|-----------------------------------------------|---------------------------------------------------------|
| Mask (Block)       | `sudo systemctl mask <service>`               | `sudo systemctl mask apache2`                           |
| Unmask (Allow)     | `sudo systemctl unmask <service>`             | `sudo systemctl unmask nginx`                           |
| List All Services  | `systemctl list-units --type=service`         | `systemctl list-units --type=service --state=running`   |
| List All Unit Files| `systemctl list-unit-files`                   | `systemctl list-unit-files | grep nginx`               |

**Note:**  
- Masked services cannot be started manually or automatically until unmasked.

---

### 4.5. Troubleshooting & Logs

| **Task**                   | **Command**                                        | **Example**                      |
|----------------------------|----------------------------------------------------|----------------------------------|
| **Check Service Logs**     | `journalctl -u <service> -xe`                      | `journalctl -u nginx -xe`        |
| **Verify Service Name**    | `systemctl list-unit-files | grep <service>`       | `systemctl list-unit-files | grep ssh`   |
| **Check Status**           | `sudo systemctl status <service>`                  | `sudo systemctl status nginx`    |
| **Reload systemd**         | `sudo systemctl daemon-reload`                     |                                  |
| **Reinstall if corrupted** | `sudo apt reinstall <package_name>` (Debian/Ubuntu)|                                  |
|                            | `sudo yum reinstall <package_name>` (RHEL/CentOS)  |                                  |

**General Troubleshooting Steps:**
1. Check the service status for errors.
2. Review logs with `journalctl`.
3. Ensure the service name is correct.
4. Reload systemd if you’ve changed unit files.
5. Reinstall the package if corruption is suspected.

---

## 5. Example Workflow

### Scenario: Setting Up Nginx Web Server (Ubuntu)

```bash
# Update package lists and install Nginx
sudo apt update && sudo apt install nginx -y

# Start & Enable Nginx to run at boot
sudo systemctl start nginx
sudo systemctl enable nginx

# Check if Nginx is running
sudo systemctl status nginx

# After configuration changes, restart Nginx
sudo systemctl restart nginx
```

<img width="2048" height="5774" alt="carbon (10)" src="https://github.com/user-attachments/assets/276fb535-ab19-4a09-93dd-ddf3d1267064" />

**Expected Output:**
- `systemctl status nginx` should show `Active: active (running)`.
- If errors occur, refer to `journalctl -u nginx -xe` for details.

---

![Nginx Status Example](https://github.com/user-attachments/assets/416566db-01bc-402a-978c-4428bb103793)

## 6. Best Practices

- ✔ **Always check service status after changes.**
- ✔ **Use `reload` instead of `restart` when possible to minimize downtime.**
- ✔ **Test all service changes in a staging environment before applying to production.**
- ✔ **Use descriptive names and comments when modifying unit files.**
- ✔ **Document any custom changes for future reference.**

---

## 7. References

- [PhoenixNAP: Manage Linux Services](https://phoenixnap.com/kb/manage-services-linux)
- [Official systemd Documentation](https://www.freedesktop.org/wiki/Software/systemd/)
- [systemctl Man Page](https://man7.org/linux/man-pages/man1/systemctl.1.html)
- [Nginx Documentation](https://nginx.org/en/docs/)

---

## 8. Document Control

| **Version** | **Last Updated**    | **Approved By**       |
|-------------|---------------------|-----------------------|
| 1.0         | 2025-07-17          | [Name/Title]          |

---

## Quick Links

- [systemctl Man Page](https://man7.org/linux/man-pages/man1/systemctl.1.html)
- [Debian Service Management](https://wiki.debian.org/systemd)

---
