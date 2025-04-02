# Setup cPanel on Azure with Proxmox

## Project Overview
This guide provides step-by-step instructions on setting up **cPanel/WHM** on an **Azure Virtual Machine** using **Proxmox** as the virtualization platform, as outlined in the file `Set Up cPanel.pdf`.

## Features
- Deploy **Proxmox** on an Azure VM.
- Create and configure a **virtual machine** with **AlmaLinux**.
- Install and set up **cPanel/WHM**.
- Assign a **public IP** for external access.

## Prerequisites
Ensure you have the following before proceeding:
- An **Azure subscription** with access to create virtual machines.
- A **Proxmox ISO** or image to install on Azure.
- A **domain name** (optional but recommended for cPanel/WHM setup).

## Installation Steps

### Step 1: Deploy Proxmox on Azure
```sh
# Create an Azure Virtual Machine
az vm create --name ProxmoxVM --resource-group MyResourceGroup --image UbuntuLTS --size Standard_D2s_v3 --admin-username azureuser --generate-ssh-keys
```
- Configure network settings in Azure to allow Proxmox access.
- Connect to the VM via SSH and install Proxmox.

### Step 2: Install Proxmox VE
```sh
# Update system packages
sudo apt update && sudo apt upgrade -y

# Install Proxmox
sudo apt install proxmox-ve postfix open-iscsi -y
```
- Configure Proxmox networking.
- Access Proxmox web interface via `https://<public-ip>:8006`.

### Step 3: Create a Virtual Machine for cPanel
- Upload **AlmaLinux ISO** to Proxmox.
- Create a **new virtual machine** and install AlmaLinux.

### Step 4: Install cPanel/WHM
```sh
# Set hostname
hostnamectl set-hostname server.example.com

# Update system
yum update -y

# Install cPanel/WHM
cd /home && curl -o latest -L https://securedownloads.cpanel.net/latest && sh latest
```
- Follow the on-screen installation process.
- Access WHM via `https://<server-ip>:2087`.

## Configuration
- Assign a **public IP** in Azure.
- Configure **DNS settings** if using a domain.
- Set up firewall rules to allow **cPanel/WHM ports**.

## Usage
Once the setup is complete, log in to **WHM** to configure cPanel settings:
```sh
https://<server-ip>:2087
```
- Use **root credentials** to log in.
- Configure nameservers, security settings, and hosting accounts.

## Troubleshooting
- **Proxmox not accessible?** Ensure Azure firewall allows **port 8006**.
- **cPanel not working?** Check firewall settings and ensure **port 2087** is open.

## License
This guide follows open-source principles and is free to use and modify.

