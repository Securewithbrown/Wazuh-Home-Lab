# Sysmon Configuration

## Objective
Enhance endpoint visibility using Sysmon for detailed logging.

## Installation

Installed Sysmon on Windows 11 using a predefined configuration file.

Command used:
./Sysmon64.exe -i sysmon-config.xml 

![sysmon](/screenshots/Sysmon/Sysmon-install.png)


## Configuration Source
Sysmon configuration sourced from community baseline (SwiftOnSecurity).

## Reason for Selection
Chosen for balanced logging and compatibility with Wazuh-based detection.

## Enabled Event Types
- Event ID 1 (Process Creation)
- Event ID 3 (Network Connections)
- Event ID 7 (Image Load)
- Event ID 11 (File Creation)

## Verification
Confirmed Sysmon service is running and logs are being generated.

![](/screenshots/Sysmon/sysmon-event-viewer.png)

## Outcome
Sysmon successfully deployed and integrated with Wazuh for enhanced detection.

## Why Sysmon is Important
Sysmon provides enhance telemetry such process creation, file creation, and network connections. Enabling Wazuh to detect advance attack techniques such as Powershell abuse and lateral movements.
