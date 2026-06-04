# Wazuh Server Installation
## Objective
Deploy the Wazuh Server on Ubuntu OS for centralized security monitoring of Windows and Ubuntu endpoints.

## Environment
* OS: Ubuntu 22.04
* CPU: 2vCPU
* RAM: 4GB
* Disk: 50GB
* IP: 

## System preparation
Updated package repositories.

Command used: sudo apt update && sudo apt upgrade -y

![ubuntu-update](/screenshots/Installation/Ubuntu-system-update.png)

## Installation Steps
Dowloaded Wazuh installer.

Command used: curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh && sudo bash ./wazuh-install.sh -a

![wazuh-install](/screenshots/Installation/Wazuh-install-complete.png)

## Network Configuration
NAT Interface(Internet): 

**enp0s3**: 10.0.2.15


Internal Lab Interface:

**enp0s8**: 192.168.1.2

The Internal Network Interface is used for agent enrollment and dashboard access.

## Verification
Checked wazuh manager status to verified if it's running.

Command used: systemctl status wazuh-manager
![Wazuh-status](/screenshots/Installation\Wazuh-manager-status.png)

Accessed Wazuh dashboad on the web browser with the ubuntu ip address.
![Wazuh-login-dashboard](/screenshots/Installation/Wazuh-web-dashboard.png)

## Challenges
**Issue**: Unable to establish a connection with the ubuntu package server after system restart.

**Resolution** : Replaced the default package mirrors (archive.ubuntu.com and security.ubuntu.com) with the global mirror list (mirror://mirrors.ubuntu.com/mirrors.txt) in the /etc/apt/sources.list file. This allows apt to automatically select a working mirror.
![Updated package mirror](/screenshots/Installation/updated-package-mirror.png)