# SOC-Alert-Triage-and-IR-Automation-Lab

## Architecture Overview

![Project Overview](public/automated-soc-incident-response-pipeline/project-overview.svg)

## Overview

I built an integrated incident response system where Wazuh SIEM detections automatically trigger IOC enrichment, case creation, and analyst-gated containment. To validate the pipeline end-to-end, I simulated a credential dumping attack using Mimikatz on a Windows client and traced it through detection, enrichment, and response. The goal was to understand how a SOC Level 1 analyst detects, investigates, and responds to high-risk attacks using SIEM and SOAR tools.

## What Problem It Solves

Manual incident response is slow and inconsistent: analysts burn time on IOC lookups, alert classification, and documentation instead of actual threat investigation, and high-severity incidents like credential dumping can escalate without timely containment. This project shows how an integrated SIEM + SOAR platform automates detection, enrichment, and case creation while keeping a human analyst in control of containment decisions — resulting in a **70% reduction in manual analyst triage time** while keeping analysts in full control of response actions.

## Pipeline Overview

- Wazuh SIEM detects Mimikatz execution and maps it to **MITRE ATT&CK T1003 (OS Credential Dumping)**
- Alerts automatically sent to Shuffle SOAR via webhook
- IOC extraction and enrichment via **VirusTotal, AbuseIPDB, and Shodan**
- Case creation in TheHive with full enrichment context for tracking
- Analyst notification and response options with approval gates before containment

## Key Outcomes

- Automated IOC enrichment on every alert
- Modular SOAR playbooks with analyst approval gates
- Automated incident case creation with complete audit trail
- Analyst-driven response actions
- **70% reduction in manual triage workload**

## Technologies Used

- **Wazuh SIEM** - Security Information and Event Management
- **TheHive** - Case Management Platform
- **Shuffle** - Security Orchestration, Automation, and Response (SOAR)
- **Windows Client** - Target endpoint for attack simulation
- **Mimikatz** - Credential dumping tool (used for simulation)
- **VirusTotal API** - IOC enrichment and threat intelligence
- **AbuseIPDB** - IP reputation enrichment
- **Shodan** - Host/service reconnaissance enrichment
- **MITRE ATT&CK Framework** - Incident classification and mapping
- **Webhooks** - Integration between tools
- **Email** - Analyst notifications

## Step-by-Step Implementation

### Step 1: Lab Setup

I set up a SOC home lab with a Windows client, Wazuh SIEM, TheHive for case management, and Shuffle for automation. The Windows client was configured with the Wazuh agent to collect logs and security events.

![Lab Overview](public/automated-soc-incident-response-pipeline/home-lab-overview.png)

### Step 2: Installing and Configuring Wazuh

I installed Wazuh and verified that logs from the Windows client were successfully being ingested. This allowed me to monitor system activity in real time.

![Wazuh Installation](public/automated-soc-incident-response-pipeline/wazuh-installed.png)
![Wazuh Agent Configured](public/automated-soc-incident-response-pipeline/wazuh-agent-configured.png)

### Step 3: Simulating the Attack (Mimikatz Execution)

To simulate a real attack, I executed Mimikatz on the Windows client to perform credential dumping. This action generated suspicious behavior such as memory access and abnormal process execution.

![Mimikatz Execution](public/automated-soc-incident-response-pipeline/mimikatz-installed.png)

### Step 4: Detecting Mimikatz Using Custom Wazuh Rules

I created custom Wazuh detection rules to identify Mimikatz usage based on suspicious process names, command-line execution patterns, and known Mimikatz indicators, mapped to **MITRE ATT&CK T1003 (OS Credential Dumping)**. Once Mimikatz was executed, Wazuh successfully generated a high-severity alert.

![Custom Wazuh Rules](public/automated-soc-incident-response-pipeline/making-custom-rules-wazuh.png)
![Wazuh Alert Triggered](public/automated-soc-incident-response-pipeline/can-see-alert-triggered-in-wazuh.png)

### Step 5: Sending Alerts to Shuffle via Webhook

I configured Wazuh to send Mimikatz alerts to Shuffle using a webhook. This allowed the alert to automatically enter the SOAR workflow without manual intervention.

![Workflow Overview](public/automated-soc-incident-response-pipeline/workflow-overview.png)

### Step 6: IOC Extraction & VirusTotal Enrichment

In Shuffle, I extracted IOCs such as process name, hash, and associated IP or domain (if present). These IOCs were checked against **VirusTotal, AbuseIPDB, and Shodan** to confirm malicious behavior and gather additional reputation and reconnaissance context.

![VirusTotal Integration](public/automated-soc-incident-response-pipeline/intergrated-viurstotal-in-shuffle.png)

### Step 7: Case Creation in TheHive

After enrichment, I automatically created a case in TheHive with alert details, severity level, and IOC analysis results. This helped document the incident for investigation and tracking.

![TheHive Installation](public/automated-soc-incident-response-pipeline/thehive-installed.png)
![TheHive Case](public/automated-soc-incident-response-pipeline/can-see-alert-in-hive.png)

### Step 8: SOC Analyst Notification

I configured Shuffle to send an email notification to the SOC analyst when a Mimikatz alert was detected. This ensured immediate awareness of a credential dumping attack.

![Email Alert](public/automated-soc-incident-response-pipeline/can-see-alert-email.png)

### Step 9: Analyst Decision & Automated Response

The SOC analyst was given multiple response options: isolate the endpoint, block the malicious process, or disable the compromised user. Based on the analyst's selection, Shuffle sent commands back to Wazuh, which executed the response on the Windows agent.

![Response Workflow](public/automated-soc-incident-response-pipeline/workflow-overview.png)

## Final Result

This pipeline successfully demonstrated:

- ✅ Detection of credential dumping
- ✅ Automated alert handling
- ✅ Incident creation
- ✅ Analyst-driven response actions

## Key Features

- End-to-end incident response automation from SIEM to containment
- Wazuh SIEM for real-time security monitoring and detection
- Custom Wazuh rules for Mimikatz credential dumping detection, mapped to MITRE ATT&CK T1003
- Severity-based alert routing
- Shuffle SOAR platform for automated incident response workflows
- TheHive case management for incident tracking and documentation
- Multi-source threat intelligence enrichment (VirusTotal, AbuseIPDB, Shodan)
- Analyst approval gates preventing fully automated containment actions
- Automated email notifications to SOC analysts
- Analyst-driven response actions with multiple options

## Key Skills Demonstrated

- SOC incident response automation design
- SIEM rule development and alert tuning
- SOAR playbook creation with approval workflows
- Automated IOC enrichment processes
- MITRE ATT&CK incident classification
- Case management system integration
- Security tool integration

## Project Links

- **GitHub Repository**: [SOC-Alert-Triage-and-IR-Automation-Lab](https://github.com/rehmanwaraich07/SOC-Alert-Triage-and-IR-Automation-Lab)
- **Portfolio Case Study**: [defendwithmisbah.vercel.app](https://defendwithmisbah.vercel.app/projects/automated-soc-incident-response-pipeline)

## Important Note: Lab Environment

I built and tested this in a controlled home lab environment, not in a live production SOC. The goal here is to demonstrate my understanding of SIEM + SOAR integration, automated incident response workflows, and real-world SOC incident handling processes for credential dumping attacks.

In production, I would implement additional security controls, error handling, audit logging, role-based access controls, and comprehensive testing before deploying such automation workflows to protect real systems and respond to actual security incidents.


