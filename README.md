# 🛡️ Enterprise SIEM Deployment (Splunk)

## 📌 Project Overview
Deployed and configured a Splunk Enterprise Security Information and Event Management (SIEM) environment to monitor, ingest, and analyze security telemetry from a vulnerable Active Directory domain. This project demonstrates proficiency in enterprise log management, custom SPL (Splunk Processing Language) querying, and active threat hunting.

## 🏗️ Architecture & Naming Convention
To mirror an enterprise environment, strict naming conventions and isolated virtual networking were enforced:
* **`ET-DC01`:** Windows Server Domain Controller (Log Source / Victim Machine)
* **`ET-SIEM01`:** Ubuntu Server 24.04 LTS (Headless Splunk Enterprise Engine)
* **Network:** Both machines share an isolated, host-only NAT network to ensure secure telemetry delivery.

---

## 🛠️ Phase 1: SIEM Provisioning & Installation
Provisioned a dedicated, headless Linux server (`ET-SIEM01`) to host the Splunk Enterprise engine, prioritizing resource allocation for high-speed log parsing. Remote administration was established via SSH to execute the deployment.

*(Screenshot 1: SSH Connection to ET-SIEM01)*
![SSH Connection](Screenshots/01-ssh-connection.png)

*(Screenshot 2: Splunk Service Running)*
![Splunk Service](Screenshots/02-splunk-service-running.png)

---

## 📡 Phase 2: Telemetry & Log Ingestion
Configured `ET-DC01` (Windows Server) to act as the primary log source. Installed and configured the Splunk Universal Forwarder to continuously ship Windows Event Logs (Security, System, Application) over the internal NAT network to the `ET-SIEM01` indexer.

*(Screenshot 3: Windows Server Universal Forwarder Configuration)*
*(Screenshot 4: Splunk Receiving Logs)*
---

## ⚔️ Phase 3: Attack Simulation & Threat Hunting
Simulated an adversary attempting to compromise the Active Directory Domain Controller via a brute-force credential attack. Leveraged custom SPL (Splunk Processing Language) queries to detect the anomalous authentication behavior and track Event ID 4625 (Failed Logon).

*(Screenshot 5: Brute Force Attack Execution)*
*(Screenshot 6: Splunk Dashboard Querying Event ID 4625)*
