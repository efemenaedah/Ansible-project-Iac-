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

# Step 2 — * Launch multiple EC2 instances that will be managed by Ansible** 
This project includes:

- Amazon Linux clients
- Ubuntu clients
Allow inbound SSH access.
The same key pair is used
- Instances are reachable from the controller

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

<img width="1826" height="864" alt="Screenshot (9)" src="https://github.com/user-attachments/assets/03b032a3-9324-4db0-a8ef-630477d4e463" />

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

<img width="1888" height="975" alt="Screenshot (10)" src="https://github.com/user-attachments/assets/8f651578-4c96-4c6e-9f49-9a3f81071794" />

# Step 5 — Copy SSH Key Pair to Remote Server

Use `scp` to securely copy the private key to the Ansible controller.

```bash
scp -i ansible-keypair.pem ansible-keypair.pem ec2-user@54.72.120.89:/home/ec2-user/
```

---

# Step 6 — SSH into Client Servers

- Example connection to Amazon Linux:
- Example connection to Ubuntu:

# Step 7 — Configure the Ansible Inventory File

Adding the following configuration for the inventory:

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
<img width="1880" height="989" alt="Screenshot (11)" src="https://github.com/user-attachments/assets/eb28d94f-c2b8-4c28-8b46-cdcafbc8a1cd" />


# Step 8 — Test Connectivity with Ansible Ping

Run the following command:

```bash
ansible clients -i inventory -m ping --private-key=ansible-keypair.pem
```

Expected output:

## Screenshot — Successful Ansible Ping
<img width="1867" height="991" alt="Screenshot (12)" src="https://github.com/user-attachments/assets/bfdeb4c3-e92a-4e97-beb2-5c3bf5eac61a" />


# Step 9 — Run the Ansible Playbook

Example playbook execution:
<img width="1882" height="975" alt="Screenshot (13)" src="https://github.com/user-attachments/assets/9cdba125-c717-4588-91d3-c4a7e17569fc" />


The playbook automatically:

- Installs Apache on Ubuntu & Amz linux servers
- Configures services
- Starts Apache services
- Applies automated configurations to all managed nodes
- install Git
---

# Project Outcomes

This project successfully demonstrates:

- Infrastructure automation using Ansible
- Centralized configuration management
- Multi-server administration
- Automated software installation
- SSH-based orchestration
- Cross-platform Linux management (Amazon Linux & Ubuntu)

# Author

## Edah Efemena Evans

Cloud & DevOps Engineer  
AWS | Ansible | 


