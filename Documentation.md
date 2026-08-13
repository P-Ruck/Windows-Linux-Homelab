# Windows & Linux Home Lab — Documentation

## Overview

This project is a personal home lab built to develop hands-on experience with Windows and Linux administration, virtualization, networking, troubleshooting, and cybersecurity.

The lab uses a Windows PC as the primary system and a Kali Linux virtual machine running through VirtualBox.

The environment was also used as the foundation for a separate SIEM project using Graylog and NXLog.

---

## Environment

### Windows PC

The Windows PC serves as the primary system in the lab.

Windows is used for:

* System administration
* PowerShell
* Windows Event Viewer
* Network configuration
* Firewall configuration
* Log generation
* Network troubleshooting
* Security monitoring

The Windows PC is also the primary log source for the SIEM project.

### Kali Linux Virtual Machine

Kali Linux runs as a virtual machine using VirtualBox.

Kali is used for:

* Linux administration
* Network troubleshooting
* Network discovery
* Packet capture
* Nmap
* tcpdump
* Hosting the Graylog SIEM environment

### VirtualBox

VirtualBox is used to run the Kali Linux virtual machine and configure communication between the Windows host and the virtual machine.

Different virtual networking configurations were tested while building the lab.

The lab eventually used a host-only network configuration to allow the Windows PC and Kali Linux VM to communicate through a dedicated virtual network.

---

# Network Environment

The home lab consists of the following primary systems:

```text
┌─────────────────────────┐
│       Windows PC        │
│                         │
│  Windows Administration │
│  Event Logs             │
│  PowerShell             │
└────────────┬────────────┘
             │
             │ VirtualBox
             │ Network
             │
┌────────────▼────────────┐
│     Kali Linux VM       │
│                         │
│  Linux Administration   │
│  Nmap                   │
│  tcpdump                │
│  Graylog SIEM            │
└─────────────────────────┘
```

The exact IP addresses and other network-specific information are intentionally not included in this public documentation.

---

# Networking

Networking was an important part of building the lab because the Windows system and Kali Linux VM needed to communicate with each other.

Network configuration and troubleshooting were performed on both operating systems.

## Windows

Windows networking was investigated using:

```powershell
ipconfig
```

PowerShell was also used to examine IPv4 addresses:

```powershell
Get-NetIPAddress -AddressFamily IPv4 |
    Format-Table InterfaceAlias,IPAddress
```

Network profiles were examined using:

```powershell
Get-NetConnectionProfile
```

## Kali Linux

Linux network interfaces were examined using:

```bash
ip addr
```

IPv4 configuration could be examined with:

```bash
ip -4 addr
```

The routing table was examined using:

```bash
ip route
```

Connectivity was tested using `ping`.

---

# Network Troubleshooting

The lab required troubleshooting communication between Windows and Kali Linux.

Issues investigated included:

* Incorrect or unexpected network interfaces
* VirtualBox adapter configuration
* IP addressing
* Routing
* Firewall rules
* ICMP connectivity
* Network traffic

At one point, Kali Linux returned:

```text
Network is unreachable
```

while attempting network communication.

This led to investigation of the Kali network interfaces, routing table, and VirtualBox adapter configuration.

---

# Firewall Investigation

Windows Firewall was examined during connectivity troubleshooting.

Firewall profiles were checked using:

```powershell
Get-NetFirewallProfile |
    Format-Table Name,Enabled,DefaultInboundAction
```

ICMP-related rules were also examined:

```powershell
Get-NetFirewallRule -DisplayName "*ICMP*" |
    Select-Object DisplayName,Enabled,Direction,Action
```

This provided hands-on experience understanding how firewall rules can affect network troubleshooting.

---

# Network Discovery

Nmap was used from Kali Linux to investigate systems and services within the lab.

A TCP service/version scan was performed using:

```bash
sudo nmap -sT -sV <target>
```

A full TCP port scan was also used:

```bash
sudo nmap -sT -sV -p- <target>
```

The scans were used to identify:

* Open ports
* Available services
* Service versions
* Potentially unfamiliar services

During the project, a Steam In-Home Streaming service was identified during scanning.

Nmap results were then investigated rather than assuming that an unfamiliar port represented malicious activity.

---

# Network Traffic Analysis

`tcpdump` was used on Kali Linux to inspect network traffic during troubleshooting.

Example:

```bash
sudo tcpdump -i <interface>
```

Packet captures helped determine whether traffic was reaching the expected network interface.

This provided experience with examining network traffic at a lower level when normal connectivity tests were insufficient.

---

# Windows Network and Process Investigation

Windows PowerShell was used to investigate listening network connections.

The following command was used:

```powershell
Get-NetTCPConnection -State Listen |
    Sort-Object LocalPort |
    Format-Table LocalAddress,LocalPort,State,OwningProcess
```

When a listening port required further investigation, the associated process ID was examined.

For example:

```powershell
Get-Process -Id <PID> |
    Select-Object Id,ProcessName,Path
```

This allowed network ports to be correlated with the processes responsible for them.

The investigation process was:

```text
Listening Port
      ↓
Process ID
      ↓
Process
      ↓
Executable Path
      ↓
Expected Service
```

This helped develop an understanding of how to investigate unexpected network activity without immediately assuming that it was malicious.

---

# Tools Used

| Tool                 | Purpose                                    |
| -------------------- | ------------------------------------------ |
| VirtualBox           | Virtual machine management and networking  |
| Windows PowerShell   | Windows administration and troubleshooting |
| Windows Event Viewer | Event investigation                        |
| Kali Linux           | Linux administration and security tools    |
| Nmap                 | Network discovery and service enumeration  |
| tcpdump              | Network traffic analysis                   |
| Windows Firewall     | Firewall configuration and investigation   |
| Graylog              | SIEM and centralized logging               |
| NXLog                | Windows log collection and forwarding      |

---

# Skills Developed

Through the home lab, I developed practical experience with:

* Windows administration
* Linux administration
* Virtualization
* Virtual networking
* IP configuration
* Routing
* Firewall troubleshooting
* Network connectivity troubleshooting
* Network discovery
* Port and service investigation
* Packet capture
* PowerShell
* Linux command-line tools
* Technical problem solving

---

# Project Goals

The primary goal of the lab is to build practical IT and cybersecurity skills through hands-on experimentation.

Areas of focus include:

* Understanding Windows and Linux systems
* Learning how computer networks operate
* Troubleshooting network connectivity
* Understanding ports and network services
* Learning how system logs can be monitored
* Developing familiarity with security tools
* Building a foundation for future IT and cybersecurity projects

---

# Related Project

This home lab also serves as the foundation for my Graylog SIEM project.

The SIEM uses the Windows PC as a log source and the Kali Linux VM as the Graylog server.

See:

**[Graylog SIEM Home Lab](../graylog-siem-homelab)**
