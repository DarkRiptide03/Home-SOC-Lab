# Home SOC Lab

## Overview

This repository documents the buildout of a home Security Operations Centre (SOC) lab using Wazuh, Sysmon, Windows 11 Pro, Ubuntu Server, and VirtualBox.

The purpose of this lab is to practise blue-team skills in a controlled virtual environment, including endpoint monitoring, log collection, alert triage, threat hunting, and SOC-style investigation reporting.

## Project Goals

The main goals of this project are to:

* Build a working Wazuh-based monitoring environment.
* Connect a Windows endpoint to Wazuh using the Wazuh Agent.
* Collect Windows telemetry using Sysmon.
* Simulate common endpoint activity.
* Investigate alerts using Wazuh Threat Hunting.
* Document findings in structured incident reports.

## Repository Structure

```text
Home-SOC-Lab
│
├── Architecture
│   └── architecture.md
│
├── Detection-Scenarios
│   └── 01-local-group-discovery
│       ├── report.md
│       └── Screenshots
│
├── References
│   └── references.md
│
├── Screenshots
│
├── Setup-Notes
│   ├── wazuh-server-setup.md
│   ├── windows-endpoint-setup.md
│   └── sysmon-configuration.md
│
└── README.md
```

## Lab Status

| Area                            | Status    |
| ------------------------------- | --------- |
| Wazuh Server                    | Completed |
| Windows 11 Endpoint             | Completed |
| Wazuh Agent Deployment          | Completed |
| Sysmon Installation             | Completed |
| Sysmon Log Ingestion            | Confirmed |
| Local Group Discovery Scenario  | Completed |
| Suspicious PowerShell Scenario  | Planned   |
| Network Reconnaissance Scenario | Planned   |

## Current Detection Scenarios

| Scenario                                 | Status    | Description                                                                                |
| ---------------------------------------- | --------- | ------------------------------------------------------------------------------------------ |
| 01 - Local Group Discovery via `net.exe` | Completed | Detection of local group discovery activity from a Windows endpoint using Sysmon and Wazuh |
| 02 - Suspicious PowerShell Execution     | Planned   | Detection of suspicious PowerShell command-line behaviour                                  |
| 03 - Network Reconnaissance              | Planned   | Detection of scanning activity from an attacker VM                                         |

## Key Skills Demonstrated

This project demonstrates practical experience with:

* SIEM/XDR deployment
* Endpoint monitoring
* Wazuh Agent configuration
* Sysmon telemetry collection
* Windows process creation analysis
* Parent-child process investigation
* Threat hunting in Wazuh
* Basic MITRE ATT&CK mapping
* SOC-style report writing

## Documentation

Detailed documentation is separated into individual files:

* Lab design: `Architecture/architecture.md`
* Wazuh setup: `Setup-Notes/wazuh-server-setup.md`
* Windows endpoint setup: `Setup-Notes/windows-endpoint-setup.md`
* Sysmon setup: `Setup-Notes/sysmon-configuration.md`
* Detection reports: `Detection-Scenarios/`
* References: `References/references.md`

## Disclaimer

This project is for educational and defensive security purposes only. All activity was performed in an isolated virtual lab environment.
