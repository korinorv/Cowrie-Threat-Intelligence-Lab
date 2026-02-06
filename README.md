# Cowrie-Threat-Intelligence-Lab
A deep-dive threat analysis of automated SSH botnet behaviour. This repository documents the deployment of a Cowrie medium-interaction honeypot in the EU-West region, capturing 5,800+ attack vectors, analysing a 3-phase post-exploitation lifecycle, and identifying Mirai/Gafgyt malware variants captured over a 24-hour window.

## Executive Summary
This project involved deploying an intentionality vulnerable SSH service to the Frankfurt (EU) region to capture and analyze real-world attack patterns. 

**Key Metrics:**
- **Time to First Hit:** 4 Minutes
- **Total Connections:** 5,842
- **Unique Malware Samples:** 22 (Mirai & Gafgyt variants)

## Architecture
- **Honeypot:** Cowrie (Medium-Interaction)
- **Exposure:** Ngrok TCP Tunnel (Frankfurt Node)
- **Target Host:** Linux VM (jinguban)

## Attack Lifecycle Captured
The analysis revealed a consistent 3-phase automated attack pattern:
1. **Reconnaissance:** System fingerprinting (`uname -a`, `cpuinfo`).
2. **Persistence/Cleanup:** Clearing bash history and logs.
3. **Infection:** Downloading binary payloads via `wget` or `curl`.

## Data Insights
- **Top Credentials:** `root/123456`, `admin/admin`
- **Geographic Focus:** Significant traffic originating from global botnet clusters targeting EU-West infrastructure.

## Recommendations
- Implementation of Key-based Authentication.
- Port obfuscation (moving from default 22).
- Use of Fail2Ban for automated IP blacklisting.
