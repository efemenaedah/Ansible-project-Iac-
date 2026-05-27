# Ansible-project-Iac-
## Project Overview
This project demonstrates how to use **Ansible** as a configuration management and automation tool to manage multiple AWS EC2 instances from a centralized control node (Ansible Controller).

The setup includes:

- Launching an Ansible controller EC2 instance
- Installing Python and Ansible
- Configuring client EC2 instances
- Setting up SSH communication
- Creating an Ansible inventory file
- Testing connectivity with Ansible ping
- Running playbooks to automate software installation and configuration

---

# Architecture Overview

```text
                   +----------------------+
                   |  Ansible Controller  |
                   |   Amazon Linux EC2   |
                   +----------+-----------+
                              |
                --------------------------------
                |                              |
        +-------+--------+             +-------+--------+
        | Amazon Linux   |             | Ubuntu Client  |
        | EC2 Clients    |             | EC2 Clients    |
        +----------------+             +----------------+
```

---

# Technologies Used

- AWS EC2
- Amazon Linux 2023
- Ubuntu Server
- Ansible
- Python 3
- SSH
- Linux CLI

---

# Step 1 — Launch the Ansible Controller EC2 Instance

Launch an **Amazon Linux EC2 instance** that will act as the Ansible control node.

## Recommended Configuration

| Setting | Value |
|---|---|
| Key Pair | ansible-keypair.pem |
| Security Group | Allow SSH (Port 22) |

---

# Step 2 — * launch Amazon Linux and ubuntu EC2 instance** that will act as the Ansible clients.

Allow inbound SSH access.

| Type | Protocol | Port | Source |
|---|---|---|---|
| SSH | TCP | 22 | Your IP Address |

---

# Step 3 — Install Python on the Controller

Verify Python installation:

```bash
python3 --version
```

Install Python if necessary:

```bash
sudo yum install python3 -y
```

## Screenshot — Python Installation

```text
Add Screenshot (9) here
```

---

# Step 4 — Install Ansible

Install Ansible on the controller node:

```bash
sudo yum install ansible -y
```

Verify installation:

```bash
ansible --version
```

## Screenshot — Ansible Installation

```text
Add Screenshot (10) here
```

---

# Step 5 — Launch Client EC2 Instances

Launch multiple EC2 instances that will be managed by Ansible.

This project includes:

- Amazon Linux clients
- Ubuntu clients

Ensure:

- SSH access is enabled
- The same key pair is used
- Instances are reachable from the controller

---

# Step 6 — Copy SSH Key Pair to Remote Server

Use `scp` to securely copy the private key to the Ansible controller.

```bash
scp -i ansible-keypair.pem ansible-keypair.pem ec2-user@54.72.120.89:/home/ec2-user/
```

---

# Step 7 — SSH into Client Servers

Example connection to Amazon Linux:

```bash
ssh -i ansible-keypair.pem ec2-user@<private-ip>
```

Example connection to Ubuntu:

```bash
ssh -i ansible-keypair.pem ubuntu@<private-ip>
```

---

# Step 8 — Configure the Ansible Inventory File

Create an inventory file:

```bash
nano inventory
```

Add the following configuration:

```ini
[amzlinux_clients]
172.31.41.172 ansible_user=ec2-user
172.31.39.243 ansible_user=ec2-user

[ubuntu_clients]
172.31.36.34 ansible_user=ubuntu
172.31.38.140 ansible_user=ubuntu

[clients:children]
amzlinux_clients
ubuntu_clients
```

## Screenshot — Inventory Configuration

```text
Add Screenshot (11) here
```

---

# Step 9 — Test Connectivity with Ansible Ping

Run the following command:

```bash
ansible clients -i inventory -m ping --private-key=ansible-keypair.pem
```

Expected output:

```bash
SUCCESS => {
    "ping": "pong"
}
```

## Screenshot — Successful Ansible Ping

```text
Add Screenshot (12) here
```

---

# Step 10 — Run the Ansible Playbook

Example playbook execution:

```bash
ansible-playbook -i inventory playbook.yml --private-key=ansible-keypair.pem
```

The playbook automatically:

- Installs Apache on Ubuntu servers
- Configures services
- Starts Apache services
- Applies automated configurations to all managed nodes

---

# Sample Playbook

```yaml
---
- name: Install and start Apache on Ubuntu
  hosts: ubuntu_clients
  become: yes

  tasks:
    - name: Install apache2
      apt:
        name: apache2
        state: present
        update_cache: yes

    - name: Start Apache service
      service:
        name: apache2
        state: started
        enabled: yes
```

---

# Screenshot — Successful Playbook Execution

```text
Add Screenshot (13) here
```

---

# Project Outcomes

This project successfully demonstrates:

- Infrastructure automation using Ansible
- Centralized configuration management
- Multi-server administration
- Automated software installation
- SSH-based orchestration
- Cross-platform Linux management (Amazon Linux & Ubuntu)

---

# Key Ansible Commands Used

## Ping all managed nodes

```bash
ansible all -i inventory -m ping --private-key=ansible-keypair.pem
```

## Run a playbook

```bash
ansible-playbook -i inventory playbook.yml --private-key=ansible-keypair.pem
```

## Check inventory hosts

```bash
ansible-inventory -i inventory --list
```

---

# Future Improvements

- Use Ansible Roles
- Automate Nginx deployment
- Integrate with Jenkins CI/CD
- Use Dynamic AWS Inventory
- Store secrets using Ansible Vault
- Automate Docker installation
- Configure Kubernetes worker nodes

---

# Author

## Edah Efemena Evans

Cloud & DevOps Engineer  
AWS | Ansible | Docker | Kubernetes | CI/CD


