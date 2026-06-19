# SOC Lab Architecture

## Overview

This document describes the current architecture of the Home SOC Lab. The lab is designed to simulate a small security monitoring environment using Wazuh, Sysmon, Windows 11 Pro, and Ubuntu Server.

The purpose of this architecture is to support endpoint monitoring, event collection, alert generation, and basic SOC-style investigations in a controlled virtual environment.

## Lab Components

| Component        | Role                    | Operating System | Purpose                                                   |
| ---------------- | ----------------------- | ---------------- | --------------------------------------------------------- |
| Wazuh Server     | SIEM/XDR Server         | Ubuntu Server    | Collects, indexes, analyzes, and displays security events |
| Windows Endpoint | Monitored Endpoint      | Windows 11 Pro   | Generates endpoint activity and security telemetry        |
| Sysmon           | Telemetry Source        | Windows 11 Pro   | Captures detailed Windows process and system activity     |
| Wazuh Agent      | Endpoint Agent          | Windows 11 Pro   | Sends Windows and Sysmon logs to the Wazuh server         |
| VirtualBox       | Virtualization Platform | Windows Host     | Runs the lab virtual machines                             |

## Network Architecture

The lab uses two VirtualBox network adapters:

| Adapter Type      | Purpose                                                                    |
| ----------------- | -------------------------------------------------------------------------- |
| NAT               | Provides internet access for updates and package downloads                 |
| Host-only Adapter | Allows private communication between the Wazuh server and Windows endpoint |

## IP Addressing

| System           | Interface  | IP Address      | Purpose                        |
| ---------------- | ---------- | --------------- | ------------------------------ |
| Wazuh Server     | enp0s3     | DHCP via NAT    | Internet access                |
| Wazuh Server     | enp0s8     | 192.168.19.3/24 | Host-only lab communication    |
| Windows Endpoint | Ethernet 2 | 192.168.19.4/24 | Communicates with Wazuh server |

The Wazuh server host-only adapter was configured with a static IP address:

```text
192.168.19.3/24
```

This address is used by the Windows Wazuh Agent to send logs to the Wazuh manager.

## Data Flow

```text
Windows 11 Endpoint
        |
        | Sysmon captures process and system activity
        v
Wazuh Agent
        |
        | Sends event data over the host-only network
        v
Wazuh Server
        |
        | Parses, indexes, and correlates events
        v
Wazuh Dashboard
        |
        | Analyst reviews alerts in Threat Hunting
        v
SOC Investigation
```

## Current Architecture Diagram

```text
+-----------------------------+
|        Windows Host         |
|     Oracle VirtualBox       |
|                             |
|  +-----------------------+  |
|  |   Wazuh Server VM     |  |
|  |   Ubuntu Server       |  |
|  |                       |  |
|  | enp0s3: NAT           |  |
|  | enp0s8: 192.168.19.3  |  |
|  |                       |  |
|  | Wazuh Manager         |  |
|  | Wazuh Indexer         |  |
|  | Wazuh Dashboard       |  |
|  +-----------^-----------+  |
|              |              |
|      Host-only Network      |
|              |              |
|  +-----------+-----------+  |
|  | Windows11-Endpoint    |  |
|  | Windows 11 Pro        |  |
|  |                       |  |
|  | IP: 192.168.19.4      |  |
|  | Wazuh Agent           |  |
|  | Sysmon                |  |
|  +-----------------------+  |
|                             |
+-----------------------------+
```

## Logging and Detection Pipeline

The current logging pipeline is:

```text
Endpoint Activity
→ Sysmon Event Logs
→ Wazuh Agent
→ Wazuh Manager
→ Wazuh Indexer
→ Wazuh Dashboard
→ Threat Hunting / Alert Review
```

## Confirmed Functionality

The following functionality has been confirmed:

* Wazuh dashboard is accessible from the host system.
* Windows 11 endpoint appears as an active Wazuh agent.
* Sysmon is installed and running on the Windows endpoint.
* Sysmon events are visible in Wazuh Threat Hunting.
* Wazuh detects local account/group discovery activity.
* The command `net localgroup` was captured with process and parent process details.

## Notes

This architecture is intended for educational and defensive security purposes only. All activity is performed in an isolated virtual environment.
