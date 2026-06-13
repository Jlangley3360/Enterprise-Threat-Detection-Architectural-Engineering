# Enterprise Threat Detection & SIEM Engineering Portfolio
**Author:** Joshua Langley  
**Objective:** Practical design, deployment, configuration, and verification of automated defensive alert pipelines, host integrity baselines, and active internal privilege auditing.

---

## Lab 1: Real-Time File Integrity Monitoring (FIM) & System Auditing

### Executive Summary
This project involved configuring and verifying an automated File Integrity Monitoring (FIM) detection pipeline utilizing the Wazuh SIEM/XDR architecture. By establishing active directory tripwires on high-value file systems (`/var/www/html`), the environment successfully identified unauthorized file modifications, integrity drift, and configuration tampering in real-time.

### Tools & Environment
- **SIEM Engine:** Wazuh XDR Architecture (Single-Node Stack)
- **Target Host Node:** Ubuntu Linux (Agent 000)
- **Core Engine Feature:** Syscheck (File Integrity Monitoring Daemon)

### Phase 1: Real-Time File Tampering Detection
An unauthorized data injection attack was simulated against the host's web directory assets to mirror a malicious defacement or web shell drop. The Wazuh file integrity monitoring engine detected the modification within seconds of execution, tracking cryptographic change metrics.

#### Forensic Telemetry Evidence
![FIM Event Timeline](FIM1.png)

![FIM Delta Analysis](FIM2.png)

**Key Forensic Data Extracted (Rule 550 / MITRE ATT&CK T1565.001):**
- **Monitored Object:** `/var/www/html/index.html`
- **Triggered Event:** File Modified / Integrity Checksum Mismatch
- **Pre-Attack MD5:** `5f083213de4267a0aec7920e6d5aca9a`
- **Post-Attack MD5:** `ee1e1d3f894d190b2ea5b5721e04009e`
- **Defensive Alignment:** Maps to **Stored Data Manipulation** tactics, providing immediate visibility to stop persistent web threats and unapproved changes to configuration settings.

#### Metadata Attribute & Signature Inspection
![FIM Attributes](FIM4.png)

The detailed document expansion proves full security tracking down to the exact filesystem metadata attributes. The engine captured an explicit file size expansion shifting from **24 bytes to 28 bytes** and updated the modification cryptographic baseline instantly.

### Phase 2: Centralized Telemetry Dashboarding
System-wide behavioral metrics were evaluated using the centralized SIEM reporting dashboard to isolate real-time activity anomalies from standard system behavior.

#### Integrity Metrics Verification
![System Metrics Dashboard](SysAudit.png)

**Key Operational Takeaways:**
- Demonstrated practical capability in configuring a centralized SIEM to capture host file modifications, parse cryptographic signature drift, and display focused alert indicators on the analytical timeline.

---

## Lab 2: Privilege Escalation & Insider Threat Detection

### Executive Summary
This phase targeted internal system telemetry by validating the SIEM's capability to intercept and alert on unauthorized credential probing and localized privilege escalation attempts. By monitoring core authentication logs, the environment alerts security operations when an active user attempts to compromise administrative accounts.

### Tools & Environment
- **Log Source:** Linux Core Authentication Infrastructure (`journald` / `/var/log/auth.log`)
- **Monitored Action:** Native `su` (Switch User) transactions
- **SIEM Rule Association:** Rule 5301 (su: Authentication failure)

### Technical Proof of Concept (Account Probing)
An insider threat scenario was simulated by targeting the administrative `root` profile with a sequence of rapid, unauthenticated password attempts from a local terminal session to mock a manual brute-force or credential-guessing attack loop.

#### Telemetry Evidence
![Authentication Failure Alert](auth_failure.png)

![Authentication Failure Log Analysis](auth_failure2.png)

**Key Operational Insights (MITRE ATT&CK T1078 - Valid Accounts):**
- **Log Output:** `FAILED SU (to root) vboxuser on pts/0`
- **Targeted Account:** `root`
- **Alert Level:** 5 (Security anomaly verified)
- **Defensive Alignment:** Ensures immediate, actionable alert routing the moment a threat actor or rogue internal asset attempts to abuse existing local accounts, brute-force credentials, or move laterally across system boundaries.
