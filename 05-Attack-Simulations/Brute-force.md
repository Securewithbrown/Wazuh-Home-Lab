# SSH Brute Force Detection and Response

## Objective

Simulate an SSH brute-force attack against a Linux endpoint and evaluate Wazuh's detection and response capabilities.

## Lab Environment

| System | Role |
|----------|----------|
| Kali Linux | Attacker |
| Ubuntu Agent | Target |
| Wazuh Server | Detection and Response |

---

# Phase 1: Detection Only

## Goal

Verify that Wazuh can detect repeated SSH authentication failures.

## Attack Execution

Tool:
- Hydra

Command:

```bash
hydra -l Brown -P wordlist.txt ssh://192.168.1.3
```

## Detection Results

Wazuh generated alerts indicating multiple failed SSH login attempts.

### Alert Details

- Alert Type: SSH Authentication Failure
- Source IP: 192.168.1.5
- Target Host: Ubuntu Agent
- Severity: Medium

## Evidence

### Hydra Attack

![Hydra Attack](/screenshots/Attack-simulation/BruteForce/Hydra-Attack.png)

### Wazuh Alert

![Brute Force Alert](/screenshots/Attack-simulation/BruteForce/Wazuh-alert.png)

## Findings

Multiple SSH-related alerts were generated during the brute-force simulation.

Observed rule IDs:
- 5710 (Attempt to login using a non-existent user)
- 5503 (PAM user login failed)
- 5758 (Maximum authentication attempts exceeded)
- 2502 (Repeated password failures)

Rule 5758 was selected as the primary indicator because it represents a repeated authentication failure pattern consistent with brute-force activity.

---

# Phase 2: Detection with Active Response

## Goal

Verify that Wazuh can automatically block the attacker's IP address.

## Active Response Configuration

Configured firewall-drop active response on the Ubuntu server using rule 5758 which represents repeated authentication failure pattern.

## Attack Execution

Repeated the Hydra brute-force attack.

```bash
hydra -l Brown -P wordlist.txt ssh://192.168.1.3
```

## Response Results

Wazuh detected the attack and automatically executed the firewall-drop response.

### Response Details

- Trigger: Multiple SSH authentication failures
- Action: Block attacker IP
- Source IP Blocked: 192.168.1.5

## Evidence

### Active Response Alert

![Active Response](/screenshots/Attack-simulation/BruteForce/active-response-alert.png)

### Firewall Block Verification

![Firewall Block](/screenshots/Attack-simulation/BruteForce/firewall-block.png)

## Findings

The attack was detected and automatically contained through Wazuh Active Response.

---

# Lessons Learned

- Wazuh successfully detected SSH brute-force activity.
- Active Response reduced manual intervention.
- Automated IP blocking improved containment speed.

## Conclusion

The lab demonstrated both detection and automated response capabilities against SSH brute-force attacks.