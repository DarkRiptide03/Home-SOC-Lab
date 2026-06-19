# Windows Endpoint Setup

## Overview

This document records the setup process for the Windows 11 endpoint used in the Home SOC Lab. The endpoint acts as a monitored workstation and generates telemetry for investigation in Wazuh.

## Virtual Machine Configuration

| Setting          | Value              |
| ---------------- | ------------------ |
| VM Name          | Windows11-Endpoint |
| Operating System | Windows 11 Pro     |
| RAM              | 8 GB               |
| CPU              | 4 vCPUs            |
| Disk             | 100 GB             |
| Virtualization   | Oracle VirtualBox  |

## Network Configuration

The Windows endpoint uses two VirtualBox network adapters:

| Adapter   | Type              | Purpose                         |
| --------- | ----------------- | ------------------------------- |
| Adapter 1 | NAT               | Internet access                 |
| Adapter 2 | Host-only Adapter | Communication with Wazuh server |

The endpoint received the following host-only IP address:

```text
192.168.19.4
```

The endpoint communicates with the Wazuh server at:

```text
192.168.19.3
```

## Wazuh Agent Installation

The Wazuh Agent was deployed from the Wazuh dashboard using the **Deploy new agent** option.

Configuration used:

| Field                | Value          |
| -------------------- | -------------- |
| Operating System     | Windows        |
| Architecture         | x86_64         |
| Wazuh Server Address | 192.168.19.3   |
| Agent Name           | WIN11-ENDPOINT |

After installation, the Wazuh Agent service was started:

```powershell
NET START WazuhSvc
```

The service status was checked using:

```powershell
Get-Service WazuhSvc
```

## Confirmed Functionality

The endpoint appeared in the Wazuh dashboard as:

| Field            | Value          |
| ---------------- | -------------- |
| Agent Name       | WIN11-ENDPOINT |
| Agent IP         | 192.168.19.4   |
| Status           | Active         |
| Operating System | Windows 11 Pro |
| Agent Version    | 4.12.0         |

## Notes

The Wazuh Agent starts automatically when the Windows VM boots. The endpoint only sends new telemetry while both the Windows endpoint and Wazuh server are powered on.
