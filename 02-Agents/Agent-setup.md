# Wazuh Agent Setup
## Objective 
Install wazuh agents on both Windows and Ubuntu operating system to collect and forward telemetry data (such as system logs, security events, and configuration details) to the Wazuh server. This enables centralized security monitoring, threat detection, and compliance reporting across the environment.
## Ubuntu agent deloyment
### Environment
* OS: Ubuntu 22.04
* RAM: 2GB
* CPU: 2vCPU
* Disk: 25GB
* Hostname: Ubuntu-Agent
* IP: 192.168.1.3

### Installation
Downloaded Wazuh agent  
**Command used**:  
sudo wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.14.5-1_amd64.deb && sudo WAZUH_MANAGER='192.168.1.2' WAZUH_AGENT_NAME='Ubunt-agent' dpkg -i ./wazuh-agent_4.14.5-1_amd64.deb
![](screenshots\Agent-setup\wazuh-agent-installed-ubuntu.png)

Started and enabled the agent.  
**Command used**:  
 sudo systemctl daemon-reload  
sudo systemctl enable wazuh-agent  
sudo systemctl start wazuh-agent

### Verification
Verified the agent installation by checking the status of the agent.
**Command used**: sudo systemctl status wazuh-agent.service  
![](/screenshots/Agent-setup/Ubuntu-agent-status.png)

Confirmed active connection with the wazuh server.
![wazuh-agent-on-server](/screenshots/Agent-setup/Ubuntu-agent-installation-verification.png)

---

## Windows Agent Setup
### Environment
* OS: Windows 11
* CPU: 2vCPU
* Ram: 5GB
* Disc: 80 GB
* Hostname: Windows-Agent
* IP:192.168.1.4

### Installation
Downloaded and started Wazuh agent.  
**Command Used**:  
Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.14.5-1.msi -OutFile $env:tmp\wazuh-agent; msiexec.exe /i $env:tmp\wazuh-agent /q WAZUH_MANAGER='192.168.1.2' WAZUH_AGENT_NAME='Windows-agent'

NET START Wazuh

![windows-agent-installed](/screenshots/Agent-setup/Windows-agent-installed.png)

### Verification
Confirmed active connection with the wazuh server 
![installation-verification](/screenshots/Agent-setup\Windows-agent-installation-verification.png)

---

## Result
Both agents successfully connected to the Wazuh Manager
![Agents connection](/screenshots/Agent-setup/Agents-connection-verification.png)


 
