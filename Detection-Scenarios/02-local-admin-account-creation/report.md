# Scenario 02: Local Administrator Account Creation

## Overview

This scenario demonstrates the detection of a local Windows account being created and added to the local Administrators group. The activity was performed in a controlled lab environment and monitored using Wazuh.

The purpose of this scenario is to investigate account creation and privilege assignment activity, which may indicate persistence or privilege escalation if observed unexpectedly in a real environment.

## Lab Environment

| Component        | Details                             |
| ---------------- | ----------------------------------- |
| SIEM/XDR         | Wazuh                               |
| Endpoint         | WIN11-ENDPOINT                      |
| Endpoint OS      | Windows 11 Pro                      |
| Log Source       | Microsoft-Windows-Security-Auditing |
| Monitoring Agent | Wazuh Agent                         |
| Test Account     | backupadmin                         |

## Activity Performed

The following commands were executed on the Windows endpoint:

```cmd
net user backupadmin P@ssword123! /add
net localgroup administrators backupadmin /add
```

The first command created a new local user account named `backupadmin`.

The second command added `backupadmin` to the local `Administrators` group.

## Detection Summary

Wazuh generated alerts related to account creation, account enablement, and administrator group modification.

Observed Wazuh rule descriptions included:

* `User account enabled or created`
* `Administrators Group Changed`

The activity was logged by:

```text
Microsoft-Windows-Security-Auditing
```

## Key Events Observed

| Event                      | Meaning                                                               |
| -------------------------- | --------------------------------------------------------------------- |
| Account creation           | A local user account named `backupadmin` was created                  |
| Account enablement         | The `backupadmin` account was enabled                                 |
| Administrator group change | The `backupadmin` account was added to the local Administrators group |

## Investigation Findings

| Question                                     | Answer                                                                                                          |
| -------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| Where did it happen?                         | WIN11-ENDPOINT                                                                                                  |
| What logged it?                              | Microsoft-Windows-Security-Auditing                                                                             |
| What happened first?                         | A local account named `backupadmin` was created                                                                 |
| What happened second?                        | The `backupadmin` account was enabled                                                                           |
| What account was affected?                   | backupadmin                                                                                                     |
| Who performed the action?                    | WIN11-ENDPOINT\alexei                                                                                           |
| Was the account added to a privileged group? | Yes, `backupadmin` was added to the local Administrators group                                                  |
| Wazuh rule descriptions                      | `User account enabled or created`, `Administrators Group Changed`                                               |
| Severity                                     | Wazuh showed medium-level alerts, including rule level 8 for account creation and rule level 5 for group change |

## MITRE ATT&CK Mapping

Wazuh mapped the group-change activity to:

| Technique                          | Tactic                                |
| ---------------------------------- | ------------------------------------- |
| T1484 - Domain Policy Modification | Defense Evasion, Privilege Escalation |

Although Wazuh mapped one event to T1484, the behaviour also relates conceptually to account manipulation and privilege assignment because a local account was created and added to a privileged group.

## Analyst Assessment

This activity would be suspicious in a real company unless it was expected and approved.

Creating a local account and adding it to the Administrators group could indicate:

* Unauthorized account creation
* Persistence
* Privilege escalation
* Preparation for future access
* Misuse of administrator privileges

However, this activity is not automatically malicious. It should be investigated in context by checking whether the change was approved, who performed it, when it occurred, and whether any related suspicious activity happened before or after the account creation.

## Evidence

Screenshot:

```text
screenshots/01-backupadmin-added-to-administrators.png
```

The screenshot should show the Wazuh event related to `backupadmin` being created or added to the local Administrators group.

## Cleanup

After the investigation, the test account should be removed:

```cmd
net localgroup administrators backupadmin /delete
net user backupadmin /delete
```

## Conclusion

This scenario confirmed that Wazuh can detect local account creation and administrator group modification on a Windows endpoint. The activity was successfully logged through Windows Security Auditing and surfaced in Wazuh for investigation.

In a real environment, this type of activity would require validation because unauthorized local administrator account creation can be a sign of persistence or privilege escalation.
