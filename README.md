# Splunk-SIEM-Enterprise-Deployment

Deployment documentation and standard operating procedures (SOPs) for the enterprise Security Information and Event Management (SIEM) infrastructure, featuring Splunk Enterprise provisioning, Universal Forwarder log ingestion, and active threat hunting for Everett Technologies.

## 🛠️ Infrastructure Topology & Deployment Roadmap

- **ET-DC01**: Windows Server Domain Controller (Log Source / Target)
- **ET-SIEM01**: Ubuntu Server 24.04 LTS (Headless Splunk Enterprise Engine)
- **Network Architecture**: Isolated, host-only NAT network ensuring secure telemetry delivery.

## ✅ Phase 1: SIEM Engine Provisioning & Installation
- **Status:** ✅ Completed
- **Documentation:** View Phase 1 SOP
- **Description:** Provisioned a dedicated, headless Linux server (`ET-SIEM01`) to host the Splunk Enterprise engine, prioritizing resource allocation for high-speed log parsing. Remote administration established via SSH.

### 📸 Phase 1 Quality Assurance (QA) Validation

*1. SSH Remote Administration*
![SSH Connection](Screenshots/01-ssh-connection.png)

*2. Splunk Service Initialization*
![Splunk Service](Screenshots/02-splunk-service-running.png)

## ✅ Phase 2: Telemetry Pipeline & Log Ingestion
- **Status:** ✅ Completed
- **Documentation:** View Phase 2 SOP
- **Description:** Configured the primary log source (`ET-DC01`) with the Splunk Universal Forwarder to continuously ship Windows Event Logs (Security, System, Application) over the internal NAT network to the central indexer.

### 📸 Phase 2 Quality Assurance (QA) Validation

*1. Universal Forwarder Configuration*
![Universal Forwarder](Screenshots/03-universal-forwarder.png)

*2. Log Ingestion Verification*
![Logs Received](Screenshots/04-logs-received.png)

## ✅ Phase 3: Adversary Simulation & Threat Hunting
- **Status:** ✅ Completed
- **Documentation:** View Phase 3 SOP
- **Description:** Simulated an adversary attempting to compromise the Active Directory Domain Controller via a brute-force credential attack. Leveraged custom SPL (Splunk Processing Language) queries to filter anomalous authentication behavior (Event ID 4625).

### 📸 Phase 3 Quality Assurance (QA) Validation

*1. Brute Force Attack Execution & Detection*
![Brute Force Attack](Screenshots/05-brute-force-attack.png)

## ⚠️ Technical Challenges & Resolutions

* **CLI Service Limitations:** Encountered "invalid service name" errors when attempting to restart the Splunk Universal Forwarder using classic `net start/stop` commands. **Resolution:** Pivoted to the Windows `services.msc` Graphical User Interface (GUI) to force service reboots, bypassing localized CLI syntax blockers.
* **Hidden Configuration Extensions:** The log pipeline initially failed because Windows Notepad silently appended a `.txt` extension to the custom configuration files (e.g., `inputs.conf.txt`), which Splunk strictly rejects. **Resolution:** Enabled "File name extensions" in the Windows View ribbon to expose and correct the hidden extensions, ensuring exact `.conf` formatting.
* **Telemetry Synchronization (Clock Drift):** Initially, successful brute-force attack telemetry was invisible in the Splunk dashboard. Diagnosed a hypervisor clock drift where the VM was stamping logs hours into the future. **Resolution:** Shifted Splunk search parameters from "Last 24 Hours" to "All Time" to successfully capture the future-stamped logs before correcting the hypervisor NTP sync.

## 📁 Repository Directory Structure
- **/SOPs** — Step-by-step deployment documentation, configuration guides, and validation walkthroughs.
- **/Screenshots** — Visual QA validation of successful log ingestion, telemetry mapping, and threat hunting.
- **/Configurations** — Custom `inputs.conf` and `outputs.conf` files for Universal Forwarder deployment.
