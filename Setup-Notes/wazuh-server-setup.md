# Wazuh Server Setup

## Overview

This document records the setup process for the Wazuh server used in the Home SOC Lab. The Wazuh server was deployed on an Ubuntu Server virtual machine in Oracle VirtualBox.

## Virtual Machine Configuration

| Setting          | Value                     |
| ---------------- | ------------------------- |
| VM Name          | Wazuh-Server              |
| Operating System | Ubuntu Server 24.04.4 LTS |
| RAM              | 8 GB                      |
| CPU              | 4 vCPUs                   |
| Disk             | 80 GB                     |
| Virtualization   | Oracle VirtualBox         |

## Network Configuration

The Wazuh server uses two network adapters:

| Adapter   | Type              | Purpose                                           |
| --------- | ----------------- | ------------------------------------------------- |
| Adapter 1 | NAT               | Internet access for updates and package downloads |
| Adapter 2 | Host-only Adapter | Private communication with lab endpoints          |

The host-only adapter was configured with a static IP address:

```text
192.168.19.3/24
```

The server interfaces are:

| Interface | Purpose           |
| --------- | ----------------- |
| enp0s3    | NAT adapter       |
| enp0s8    | Host-only adapter |

## Static IP Configuration

The host-only adapter `enp0s8` was configured using Netplan.

Example configuration:

```yaml
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: true
    enp0s8:
      dhcp4: false
      addresses:
        - 192.168.19.3/24
```

After editing the Netplan configuration, the changes were applied using:

```bash
sudo netplan apply
```

The IP address was verified using:

```bash
ip addr
```

## Wazuh Installation

The Wazuh all-in-one installation script was downloaded and executed:

```bash
curl -sO https://packages.wazuh.com/4.12/wazuh-install.sh
sudo bash ./wazuh-install.sh -a
```

After installation, the Wazuh dashboard was accessed from the host machine using VirtualBox port forwarding:

```text
https://127.0.0.1:8443
```

## Confirmed Functionality

The following functionality was confirmed:

* Wazuh dashboard was accessible from the Windows host.
* Wazuh server services started successfully.
* Windows endpoint was registered as an active agent.
* Sysmon telemetry from the Windows endpoint was visible in Wazuh Threat Hunting.

## Notes

A VirtualBox snapshot was taken after confirming that the Wazuh server was working, the static IP was configured, and the Windows endpoint was successfully sending telemetry.
