# Enterprise Threat Detection & Architectural Engineering Portfolio
**Author:** Joshua Langley  
**Objective:** Practical design, configuration, and verification of automated defensive alert pipelines, network forensics, host integrity baselines, and authentication telemetry.

---

## Lab 1: Host-Based Threat Detection & Network Packet Analysis

### Executive Summary
A simulated authentication attack was executed against an isolated Linux local node to validate endpoint logging capabilities within the Wazuh SIEM/XDR platform and cross-examine network traffic anomalies using Wireshark. The objective was to successfully log, analyze, and document an unauthorized access attempt mimicking automated credential probing.

### Topology & Tools
- **Operating System:** Ubuntu Linux VM
- **SIEM Engine:** Wazuh Architecture (Single-Node Stack)
- **Network Analyzer:** Wireshark Packet Inspection
- **Targeted Protocol:** SSH (Port 22)

### Phase 1: Host-Based Log Telemetry (Wazuh SIEM)
Upon executing an unauthenticated access sequence targeting non-existent system accounts, the Wazuh analysis engine successfully triggered an Alert Level 5 security flag. This event correlates directly with **MITRE ATT&CK Technique T1110 (Brute Force)**.

#### Telemetry Evidence
*(Note: Refer to local project documentation repository for associated endpoint authentication capture logs).*

### Phase 2: Deep Packet Forensic Investigation (Wireshark)
To cross-reference host logs with raw network data, network layer analysis was initiated to monitor traffic anomalies. The targeted connection requests—occurring systematically within milliseconds—characterize automated script activity rather than a standard human authentication error.

---

## Lab 2: Real-Time File Integrity Monitoring (FIM) & Security Metrics

### Executive Summary
This phase involved configuring and verifying an automated File Integrity Monitoring (FIM) detection pipeline utilizing the Wazuh SIEM/XDR architecture. By establishing active directory tripwires on high-value file systems (`/var/www/html`), the environment successfully identified unauthorized file modifications and system configuration drifting in real-time.

### Tools & Environment
- **SIEM Engine:** Wazuh XDR Architecture
- **Target Host:** Ubuntu Linux Node (Agent 000)
- **Core Technology:** FIM (File Integrity Monitoring / Syscheck Engine)

### Phase 1: Real-Time Tampering Detection
An unauthorized data injection attack was simulated against the host's web directory assets. The Wazuh file integrity monitoring engine detected the modification within seconds of execution, tracking cryptographic change metrics.

#### Forensic Telemetry Evidence
![FIM Event Timeline](FIM1.png)

![FIM Delta Analysis](FIM2.png)

**Key Forensic Data Extracted (Rule 550 / MITRE T1565.001):**
- **Monitored Object:** `/var/www/html/index.html`
- **Triggered Event:** File Modified / Integrity Checksum Changed
- **Pre-Attack MD5:** `5f083213de4267a0aec7920e6d5aca9a`
- **Post-Attack MD5:** `ee1e1d3f894d190b2ea5b5721e04009e`
- **Defensive Alignment:** Maps directly to **Stored Data Manipulation** tactics, providing immediate detection against malicious website defacement or backdooring.

#### Metadata Attribute Inspection
![FIM Attributes](FIM4.png)

The detailed metadata view proves full audit tracking down to the exact filesystem attributes, including file size drift (from 24 to 28 bytes) and real-time modification timestamps.

### Phase 2: Centralized Telemetry Dashboarding
System-wide behavioral metrics were evaluated using the centralized SIEM Integrity Monitoring reporting dashboard to isolate real-time activity anomalies from standard baseline system behavior.

#### Compliance Telemetry Evidence
![System Metrics Dashboard](SysAudit.png)

---

## Lab 3: Privilege Escalation & Insider Threat Detection (Authentication Auditing)

### Executive Summary
This phase targeted internal system telemetry by validating Wazuh's capacity to intercept and alert on unauthorized credential probing and localized privilege escalation attempts. By tracking core system authentication vectors, the environment monitors for internal threat actors attempting to compromise the root administrative account.

### Tools & Environment
- **SIEM Engine:** Wazuh XDR Architecture
- **Log Source:** Linux Core Authentication Log (`/var/log/auth.log`)
- **Monitored Action:** Native `su` (Switch User) transactions

### Phase 1: Localized Account Probing Simulation
A malicious actor simulation was executed locally by targeting the `root` account with a series of randomized, high-velocity incorrect password entries, attempting to force unauthorized entry.

#### Forensic Telemetry Evidence
![Authentication Failure Alert](auth_failure.png)

**Key Operational Insights (Rule 5301 / MITRE T1078 - Valid Accounts):**
- **Triggered Policy:** `su: Authentication failure`
- **Targeted Target Account:** `root`
- **Alert Level:** 5 (Security anomaly verified)
- **Defensive Alignment:** Provides real-time notifications when an adversary or rogue internal asset attempts to brute-force local account credentials or move laterally across local Linux terminal loops.
