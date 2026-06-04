# Incident Report 001: SSH Brute-Force Attack

## Executive Summary

An SSH brute-force attack was simulated from a Kali Linux attacker system against an Ubuntu endpoint monitored by Wazuh. The objective was to validate the detection and response capabilities of the Wazuh SIEM platform.

Wazuh successfully detected repeated authentication failures, generated multiple correlated alerts, and executed an Active Response action that automatically blocked the attacking IP address.

---

## Incident Information

| Field              | Value                  |
| ------------------ | ---------------------- |
| Incident ID        | INC-001                |
| Incident Type      | SSH Brute-Force Attack |
| Severity           | Medium                 |
| Detection Platform | Wazuh                  |
| Source System      | Kali Linux             |
| Target System      | Ubuntu Agent           |
| Status             | Contained              |

---

## Incident Description

A brute-force attack was launched against the SSH service running on the Ubuntu endpoint using Hydra. Multiple login attempts were performed using invalid credentials to simulate password-guessing activity.

The attack generated several SSH authentication-related alerts within Wazuh, indicating repeated login failures and excessive authentication attempts.

---

## Detection Details

### Tool Used

* Hydra

### Attack Command

```bash
hydra -l Brown -P wordlist.txt ssh://192.168.1.3
```

### Alerts Observed

| Rule ID | Description                                 |
| ------- | ------------------------------------------- |
| 5710    | SSH login attempt using a non-existent user |
| 5503    | PAM user login failed                       |
| 2502    | User missed password more than one time     |
| 5758    | Maximum authentication attempts exceeded    |

Rule 5758 was identified as the primary indicator of brute-force activity because it represents repeated authentication failures over a short period.

---

## Investigation Findings

The source IP address was identified as 192.168.1.5.

Log analysis confirmed that multiple failed authentication attempts originated from the attacking system.

No successful authentication occurred during the simulation.

---

## Containment

Wazuh Active Response was configured using the firewall-drop command.

Upon triggering Rule 5758, Wazuh automatically executed the response action and blocked the attacking IP address.

### Response Action

* Command: firewall-drop
* Trigger Rule: 5758
* Result: Attacker IP blocked

---

## Impact Assessment

No unauthorized access was achieved.

The attack remained limited to authentication failures and did not result in system compromise or privilege escalation.

---

## Lessons Learned

* Wazuh effectively detects SSH brute-force activity.
* Correlation of multiple authentication alerts improves visibility.
* Active Response reduces the time required to contain attacks.
* Automated IP blocking can significantly reduce exposure to repeated attack attempts.

---

## Conclusion

The simulation successfully demonstrated both detection and automated containment of SSH brute-force activity. Wazuh generated relevant alerts, enabled rapid investigation, and automatically blocked the attacker through Active Response, validating the effectiveness of the monitoring architecture.
