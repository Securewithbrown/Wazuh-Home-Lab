# File Integrity Monitoring (FIM)

## Objective

Simulate unauthorized file modifications on both Windows and Linux endpoints 
and verify that Wazuh's File Integrity Monitoring detects and alerts on 
the changes in real time.

## Lab Environment

| System | Role |
|--------|------|
| Wazuh Server | Detection and Monitoring |
| Ubuntu Agent | Linux Target (Monitored Endpoint) |
| Windows 11 | Windows Target (Monitored Endpoint) |

## FIM Configuration
### Ubuntu Agent — Monitored Paths

| Path | Reason |
|------|--------|
| /etc | System configuration files. Attackers modify config files here |
| /var/log | System log files. Shows log tampering detection |
|/tmp | Easy to create/delete test files without breaking anything

### Windows 11 — Monitored Paths

| Path | Reason |
|------|--------|
| C:\Users\Public | Public user directory. Safe to modify, realistic attacker target |
| C:\Windows\System32 | Critical system files. Contains the Windows hosts file — realistic tampering target |

---

## Ubuntu Agent Simulation

### Attack Execution

Simulated unauthorized file modifications on the Ubuntu endpoint.

Commands used:

```bash
# Modify a monitored file
echo "unauthorized change" | sudo tee -a /etc/hosts

# Create a new file in a monitored directory
sudo touch /tmp/malicious-test.txt
```

### Detection Results

Wazuh generated alerts indicating file modifications in monitored directories.

#### Alert Details

| Field | Details |
|-------|---------|
| Alert Type | Integrity checksum changed |
| Monitored Path | /etc/hosts |
| Rule ID | 550 |
| Severity | Medium |
| Modified By | root |

### Investigation

Navigated to Wazuh Dashboard → Modules → Integrity Monitoring to 
review the change details.

#### File Change Details

| Field | Details |
|-------|---------|
| File Path | /etc/hosts |
| Change Type | Modified |
| Old MD5 Hash |doc81939554712e32e719929aa1cb3d821 |
| New MD5 Hash | bb057bc7b105280cef870799211a327f |
| Timestamp | Jun 4, 2026 @ 08:15:21.000 |

Verified the change on the agent:

```bash
# Check file content
cat /etc/hosts

# Check modification time
stat /etc/hosts

# Verify hash
md5sum /etc/hosts
```

### Restoration

Removed unauthorized changes and restored the file to its original state.

```bash
# Remove test entry
sudo sed -i '/unauthorized change/d' /etc/hosts

# Remove test file
sudo rm /etc/malicious-test.txt
```

### Evidence

#### File Modification Command
![Ubuntu FIM Modification](/screenshots/Attack-simulation/File-Integrity-Monitoring/Ubuntu-FIM-Modification.png)

#### Wazuh FIM Alert
![Ubuntu FIM Alert](/screenshots/Attack-simulation/File-Integrity-Monitoring/Ubuntu-FIM-Alert.png)

#### FIM Change Details
![Ubuntu FIM Details](/screenshots/Attack-simulation/File-Integrity-Monitoring/Ubuntu-FIM-Event-Details.png)

#### File Restored
![Ubuntu FIM Restored](/screenshots/Attack-simulation/File-Integrity-Monitoring/Ubuntu-FIM-restored.png)

---

## Windows 11 Simulation

### Attack Execution

Simulated unauthorized file modifications on the Windows 11 endpoint.

Commands used in PowerShell (run as Administrator):

```powershell
# Create a new file in a monitored directory
New-Item -Path "C:\Users\Public\malicious-test5.txt" -ItemType File

# Write content to the file
Add-Content -Path "C:\Users\Public\malicious-test5.txt" -Value "unauthorized change"
```

### Detection Results

Wazuh generated alerts indicating file creation and modification in 
monitored directories.

#### Alert Details

| Field | Details |
|-------|---------|
| Alert Type | File added to the system |
| Monitored Path | C:\Users\Public\malicious-test5.txt |
| Rule ID | 554 |
| Severity | Medium |
| Modified By | Administrators

### Investigation

Reviewed the alert in Wazuh Dashboard → Modules → Integrity Monitoring.

#### File Change Details

| Field | Details |
|-------|---------|
| File Path | C:\Users\Public\malicious-test.txt |
| Change Type | Added |
| MD5 Hash | d41d8cd98f00b204e9800998ecf8427e |
| Timestamp |Jun 4, 2026 @ 07:17:40.000 |

### Restoration

Removed the unauthorized file from the Windows endpoint.

```powershell
# Remove test file
Remove-Item -Path "C:\Users\Public\malicious-test.txt"
```

### Evidence

#### File Creation Command
![Windows FIM Creation](/screenshots/Attack-simulation/File-Integrity-Monitoring/windows-fim-creation.png)

#### Wazuh FIM Alert
![Windows FIM Alert](/screenshots/Attack-simulation/File-Integrity-Monitoring/windows-fim-alert.png)

#### FIM Change Details
##### File Added
![Windows FIM File Added](/screenshots/Attack-simulation/File-Integrity-Monitoring/windows-fim-file-added-event.png)
##### File Modified
![Windows FIM File Modiefied](/screenshots/Attack-simulation/File-Integrity-Monitoring/windows-fim-file-modified.png)

#### File Removed
![Windows FIM Restored](/screenshots/Attack-simulation/File-Integrity-Monitoring/windows-fim-file-deleted.png)

---

## Comparison Summary

| Detail | Ubuntu Agent | Windows 11 |
|--------|-------------|------------|
| File Modified | /etc/hosts | C:\Users\Public\malicious-test5.txt |
| Change Type | Modified | Added |
| Rule ID | 550 | 554 |
| Detection Time | Jun 4, 2026 @ 08:15:21.000 | 	Jun 4, 2026 @ 07:17:40.000 |
| Hash Changed | Yes | Yes |
| Restored | Yes | Yes |

---

# Lessons Learned

- Wazuh FIM successfully detected unauthorized file changes on both 
  Windows and Linux endpoints.
- Hash comparison confirmed the exact nature of each change.
- FIM provides critical visibility into configuration file tampering 
  across heterogeneous environments.
- Both endpoints generated alerts promptly after the modifications were made.

## Conclusion

The simulation demonstrated Wazuh's cross-platform FIM capability, 
detecting unauthorized file modifications on both Ubuntu and Windows 11 
endpoints with detailed hash comparison and timestamped alerts.