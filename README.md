# Windows & Linux Home Lab

## Overview

This project is a personal home lab built to develop hands-on experience with Windows and Linux administration, virtualization, networking, and technical troubleshooting.

The environment uses virtual machines to create an isolated environment for experimenting with different operating systems, network configurations, monitoring tools, and troubleshooting techniques.

## Environment

- Windows PC — primary host and monitored system
- Kali Linux virtual machine — SIEM server
- VirtualBox — virtualization and network configuration

## Goals

- Develop Windows and Linux administration skills
- Practice network configuration and troubleshooting
- Learn virtual machine networking
- Build and configure a centralized logging environment
- Practice security monitoring and log analysis
  
## Networking

The lab uses VirtualBox networking to allow communication between the host system and virtual machines.

Networking was configured and tested using tools such as:

- `ipconfig`
- `ip addr`
- `ip route`
- `ping`
- `tcpdump`

## Tools

| Tool | Purpose |
|---|---|
| VirtualBox | Virtual machine management |
| Kali Linux | Linux administration and security testing |
| Ubuntu | Linux administration |
| PowerShell | Windows administration and troubleshooting |
| Nmap | Network discovery and service enumeration |
| tcpdump | Network traffic analysis |
| Windows Event Viewer | Windows event investigation |

## Troubleshooting Experience

During development of the lab, I encountered and resolved networking and communication issues between Windows and Linux systems.

Troubleshooting involved:

- Checking IP addresses and network interfaces
- Examining routing tables
- Testing connectivity
- Checking firewall configurations
- Inspecting network traffic
- Configuring VirtualBox network adapters
- Verifying communication between virtual machines

## Related Projects

- [Graylog SIEM Home Lab](https://github.com/P-Ruck/Graylog-Siem-Homelab)

## Skills Demonstrated

- Windows administration
- Linux administration
- Virtualization
- Network configuration
- Network troubleshooting
- PowerShell
- Nmap
- tcpdump
- Technical problem solving
