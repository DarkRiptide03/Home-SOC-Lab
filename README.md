# SOC Lab with Wazuh and Sysmon

## Overview

This project documents the creation of a home Security Operations Centre (SOC) lab using Wazuh, Sysmon, Windows 11 Pro, and Ubuntu Server. The aim of the lab is to simulate common attacker and administrator activity, collect endpoint telemetry, and investigate security alerts through Wazuh.

## Lab Objectives

- Deploy a Wazuh server on Ubuntu Server.
- Configure a Windows 11 Pro endpoint with the Wazuh Agent.
- Install and configure Sysmon for detailed Windows telemetry.
- Verify that Sysmon events are ingested into Wazuh.
- Simulate detection scenarios such as account discovery, suspicious PowerShell execution, and network reconnaissance.
- Document findings in SOC-style incident reports.

## Lab Architecture

- Wazuh Server: Ubuntu Server
- Endpoint: Windows 11 Pro
- Telemetry: Sysmon + Wazuh Agent
- Virtualization: VirtualBox
- Network: NAT + Host-only Adapter

## Current Status

- Wazuh server installed and dashboard accessible.
- Windows 11 endpoint connected as an active Wazuh agent.
- Sysmon installed and generating process creation telemetry.
- Wazuh confirmed to ingest Sysmon events from the Windows endpoint.

## Disclaimer

This lab is for educational and defensive security purposes only. All testing is performed in an isolated virtual environment.
