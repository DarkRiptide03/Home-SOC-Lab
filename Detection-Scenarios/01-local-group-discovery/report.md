# Scenario 01: Local Group Discovery via net.exe

## Overview

This scenario demonstrates the detection of local group discovery activity on a Windows 11 Pro endpoint using Sysmon and Wazuh.

The command `net localgroup` was executed from Command Prompt. Sysmon captured the process creation event, the Wazuh Agent forwarded the telemetry to the Wazuh server, and the event was reviewed in the Wazuh Threat Hunting dashboard.

## Lab Environment

| Component        | Details           |
| ---------------- | ----------------- |
| SIEM/XDR         | Wazuh             |
| Server OS        | Ubuntu Server     |
| Endpoint OS      | Windows 11 Pro    |
| Endpoint Name    | WIN11-ENDPOINT    |
| Endpoint IP      | 192.168.19.4      |
| Telemetry Source | Sysmon            |
| Agent            | Wazuh Agent       |
| Virtualization   | Oracle VirtualBox |

## Activity Performed

The following command was executed on the Windows endpoint:

```cmd
net localgroup
```

This command lists local groups on the Windows system.

## Detection Summary

Wazuh generated an event showing that local group discovery activity had occurred on the endpoint.

Key event fields observed:

| Field          | Value                         |
| -------------- | ----------------------------- |
| Agent name     | `WIN11-ENDPOINT`              |
| Agent IP       | `192.168.19.4`                |
| Command line   | `net localgroup`              |
| Process image  | `C:\Windows\System32\net.exe` |
| Parent process | `C:\Windows\System32\cmd.exe` |
| Event source   | Sysmon                        |
| Event type     | Process creation              |

## Process Relationship

The event showed the following parent-child process relationship:

```text
cmd.exe
  └── net.exe localgroup
```

This confirms that the `net localgroup` command was launched from Command Prompt.

## Why This Matters

The `net localgroup` command is a legitimate Windows administrative command. However, it can also be used by attackers during the discovery phase after gaining access to a machine.

An attacker may use this command to identify:

* Local groups
* Administrator groups
* Privileged accounts
* Potential targets for privilege escalation

In a real SOC environment, this alert would require context. The analyst would need to determine whether the command was expected administrative activity or suspicious discovery behaviour.

## MITRE ATT&CK Mapping

Potential mappings:

| Technique | Description                 |
| --------- | --------------------------- |
| T1087     | Account Discovery           |
| T1069     | Permission Groups Discovery |

## Analyst Notes

This activity was manually generated in a controlled lab environment for testing purposes.

The event is useful because it confirms that the lab can:

* Capture Windows process creation events.
* Identify command-line activity.
* Show parent-child process relationships.
* Forward Sysmon telemetry into Wazuh.
* Support basic SOC investigation workflows.

## Evidence

Screenshot:

```text
04-net-localgroup-expanded-event.png
```

This screenshot shows the expanded Wazuh event containing the command line, process image, parent process, agent name, and endpoint IP.

## Conclusion

This scenario confirms that the SOC lab is capable of detecting Windows discovery activity through Sysmon and Wazuh. The `net localgroup` command was successfully captured and displayed in Wazuh Threat Hunting, demonstrating basic endpoint visibility and alert investigation capability.
