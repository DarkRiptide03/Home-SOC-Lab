# Scenario 03: Windows Defender EICAR Detection and Quarantine

## Overview

This scenario demonstrates endpoint malware detection and remediation using Microsoft Defender Antivirus and Wazuh. A harmless EICAR test file was created on the Windows endpoint to simulate a malware detection event. Microsoft Defender detected the file through real-time protection and then quarantined it. Wazuh collected and displayed both the detection and remediation events.

The purpose of this scenario was to validate that the lab environment can detect antivirus events, identify affected files and users, and confirm whether remediation was successfully performed.

## Lab Environment

| Component        | Details                                        |
| ---------------- | ---------------------------------------------- |
| SIEM/XDR         | Wazuh                                          |
| Endpoint         | WIN11-ENDPOINT                                 |
| Endpoint OS      | Windows 11 Pro                                 |
| Log Source       | Microsoft-Windows-Windows Defender/Operational |
| Security Tool    | Microsoft Defender Antivirus                   |
| Monitoring Agent | Wazuh Agent                                    |
| Test File        | EICAR test file                                |

## Activity Performed

The EICAR test string was written to a text file on the user's Desktop using PowerShell.

```powershell
Set-Content -Path "$env:USERPROFILE\Desktop\eicar-test.txt" -Value 'X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*'
```

The EICAR file is not real malware. It is a standard test file used to verify that antivirus products can detect and respond to malicious-file simulations.

## Detection Summary

| Field            | Value                                                     |
| ---------------- | --------------------------------------------------------- |
| Endpoint         | WIN11-ENDPOINT                                            |
| Detected Threat  | Virus:DOS/EICAR_Test_File                                 |
| File Path        | C:\Users\alexei\Desktop\eicar-test.txt                    |
| Detection Source | Real-Time Protection                                      |
| Detection User   | WIN11-ENDPOINT\alexei                                     |
| Process Name     | C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe |
| Severity         | Severe                                                    |
| Category         | Virus                                                     |
| Defender Action  | Quarantine                                                |
| Remediation User | NT AUTHORITY\SYSTEM                                       |

## Key Events Observed

| Event ID | Meaning                                                              |
| -------- | -------------------------------------------------------------------- |
| 1116     | Microsoft Defender detected malware or potentially unwanted software |
| 1117     | Microsoft Defender applied a remediation action                      |

## Wazuh Rule Descriptions

| Rule Description                                                                                             | Purpose                                                    |
| ------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------- |
| Windows Defender: Antimalware platform detected potentially unwanted software                                | Indicates Defender detected a suspicious or malicious file |
| Windows Defender: Antimalware platform performed an action to protect you from potentially unwanted software | Indicates Defender performed a remediation action          |

## Investigation Findings

| Question                                           | Answer                                                                                   |
| -------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| Where did it happen?                               | WIN11-ENDPOINT                                                                           |
| What detected/logged it?                           | Microsoft-Windows-Windows Defender/Operational                                           |
| What was detected?                                 | Virus:DOS/EICAR_Test_File                                                                |
| Which file was involved?                           | C:\Users\alexei\Desktop\eicar-test.txt                                                   |
| Which process created or interacted with the file? | C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe                                |
| Which user was involved?                           | Detection user: WIN11-ENDPOINT\alexei; remediation user: NT AUTHORITY\SYSTEM             |
| What action did Defender take?                     | Quarantine                                                                               |
| What event IDs appeared?                           | 1116 and 1117                                                                            |
| Was remediation successful?                        | Yes. Defender quarantined the file and reported that no additional actions were required |

## Analyst Assessment

This activity would be considered suspicious and high priority for investigation in a real environment. Although the EICAR file used in this lab is harmless, a Defender alert for a severe virus on a user desktop should always be reviewed.

The event showed that the file was created or interacted with by PowerShell. PowerShell can be used legitimately by administrators, but it is also commonly abused by attackers for scripting, payload staging, and malware execution. The combination of a detected virus, a user desktop path, and PowerShell involvement would justify further investigation.

In this lab, Microsoft Defender successfully detected the test file through real-time protection and quarantined it. Wazuh received both the detection and remediation events, confirming that Defender alerts are being collected by the monitoring environment.

## Evidence

The screenshot below shows Wazuh alerts for the EICAR detection and quarantine events.

![Windows Defender EICAR detection and quarantine](https://i.imgur.com/AYUZs7J.png)

## Cleanup

After the test, the file was no longer present on the Desktop because Microsoft Defender quarantined it. This was confirmed using:

```powershell
Test-Path "$env:USERPROFILE\Desktop\eicar-test.txt"
```

Expected result:

```text
False
```

## Conclusion

This scenario confirmed that Microsoft Defender Antivirus can detect and quarantine the EICAR test file, and that Wazuh can collect and alert on Defender security events from the Windows endpoint.

The scenario demonstrates basic endpoint detection and response workflow skills, including identifying the affected host, reviewing the threat name, locating the file path, identifying the process involved, confirming remediation, and documenting the incident in a structured SOC-style report.
