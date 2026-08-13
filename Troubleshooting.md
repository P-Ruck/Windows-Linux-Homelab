# Windows & Linux Home Lab — Troubleshooting Log

This document records troubleshooting performed while building and configuring my Windows and Kali Linux home lab.

The current lab consists of:

* Windows PC
* Kali Linux virtual machine
* VirtualBox
* Virtual networking between Windows and Kali Linux

---

## 1. VirtualBox Network Configuration

### Problem

The Windows PC and Kali Linux virtual machine needed to communicate with each other for the home lab and SIEM environment.

The VirtualBox network configuration required adjustment to allow communication between the Windows host and Kali Linux VM.

### Investigation

I examined the network configuration on both Windows and Kali Linux.

On Windows:

```powershell
ipconfig
```

I also used PowerShell to view IPv4 addresses and their associated interfaces:

```powershell
Get-NetIPAddress -AddressFamily IPv4 |
    Format-Table InterfaceAlias,IPAddress
```

On Kali Linux:

```bash
ip addr
```

I also checked the routing table:

```bash
ip route
```

### Resolution

The VirtualBox network adapter configuration was adjusted to provide the required communication between Windows and the Kali Linux VM.

### What I Learned

VirtualBox networking can involve multiple virtual and physical interfaces. When troubleshooting communication between a host and virtual machine, it is important to verify:

1. VirtualBox adapter configuration
2. Network interface status
3. IP addresses
4. Subnet configuration
5. Routing
6. Firewall rules
7. Connectivity

---

## 2. Windows-to-Kali Connectivity

### Problem

Windows and Kali Linux were initially unable to communicate as expected.

This prevented reliable communication between the two systems and affected the SIEM setup.

### Investigation

I tested connectivity between the systems using `ping`.

On Kali Linux, I checked the routing table:

```bash
ip route
```

At one point, Kali returned:

```text
Network is unreachable
```

This indicated that the problem was related to network configuration or routing rather than the SIEM application itself.

I also checked the available network interfaces:

```bash
ip addr
```

Windows interfaces were checked with:

```powershell
ipconfig
```

### Resolution

The VirtualBox network configuration was reviewed and adjusted. Connectivity was then tested again between the Windows PC and Kali Linux VM.

### What I Learned

When troubleshooting a networked application, I should verify the underlying network before troubleshooting the application itself.

A useful troubleshooting order is:

```text
Network Interface
        ↓
IP Address
        ↓
Subnet
        ↓
Routing
        ↓
Firewall
        ↓
Connectivity
        ↓
Application
```

---

## 3. Windows Firewall and ICMP

### Problem

Windows firewall settings were investigated while troubleshooting connectivity between Windows and Kali Linux.

ICMP traffic, including `ping`, was not always behaving as expected.

### Investigation

I checked the Windows firewall profiles using:

```powershell
Get-NetFirewallProfile |
    Format-Table Name,Enabled,DefaultInboundAction
```

I also examined ICMP-related firewall rules:

```powershell
Get-NetFirewallRule -DisplayName "*ICMP*" |
    Select-Object DisplayName,Enabled,Direction,Action
```

### What I Learned

A failed `ping` does not automatically mean that a system is unreachable.

Windows Firewall can allow or block ICMP traffic independently of whether the system has a valid IP address and route.

When troubleshooting connectivity, I learned to consider:

* IP configuration
* Routing
* Firewall rules
* VirtualBox networking
* Interface status

---

## 4. Investigating Listening Ports

### Purpose

I investigated listening network ports on Windows to better understand which services were accepting connections.

This was also useful for learning how to distinguish expected services from potentially unfamiliar network activity.

### Investigation

I used PowerShell to list listening TCP connections:

```powershell
Get-NetTCPConnection -State Listen |
    Sort-Object LocalPort |
    Format-Table LocalAddress,LocalPort,State,OwningProcess
```

When an unfamiliar port was found, I used the associated process ID to investigate the process.

```powershell
Get-Process -Id <PID> |
    Select-Object Id,ProcessName,Path
```

### Investigation Process

I learned to correlate:

```text
Port
 ↓
PID
 ↓
Process
 ↓
Executable Path
 ↓
Expected Software or Service
```

### What I Learned

An open or listening port does not automatically indicate malicious activity.

The associated process and executable should be investigated to determine whether the service is expected.

---

## 5. Network Scanning with Nmap

### Purpose

I used Nmap from Kali Linux to investigate devices and services within my home lab.

A service/version scan was performed using:

```bash
sudo nmap -sT -sV <target>
```

A full TCP port scan was also performed:

```bash
sudo nmap -sT -sV -p- <target>
```

### Investigation

Nmap was used to identify:

* Open ports
* Running services
* Service versions
* Potentially unfamiliar services

During testing, a Steam In-Home Streaming service was identified.

### What I Learned

Nmap is useful for identifying services exposed by a system.

However, scan results need to be interpreted in context. An open port should be investigated to determine what service is responsible for it and whether that service is expected.

---

## 6. Packet Capture with tcpdump

### Purpose

I used `tcpdump` on Kali Linux while troubleshooting network connectivity.

Packet capture allowed me to determine whether traffic was reaching the expected network interface.

### Example

```bash
sudo tcpdump -i <interface>
```

### What I Learned

Packet capture can help determine whether:

* Traffic is being generated
* Traffic is reaching the correct interface
* Traffic is being blocked
* Traffic is reaching the destination
* A network problem is occurring before the application layer

This provided hands-on experience with lower-level network troubleshooting.

---

# Overall Lessons

Building the home lab gave me hands-on experience troubleshooting problems across multiple layers of a computer network.

The general troubleshooting process I developed was:

```text
Virtual Network
      ↓
Network Interface
      ↓
IP Configuration
      ↓
Routing
      ↓
Firewall
      ↓
Connectivity
      ↓
Ports & Services
      ↓
Application
```

## Tools Used

* VirtualBox
* Windows PowerShell
* `ipconfig`
* `ip addr`
* `ip route`
* `ping`
* Nmap
* tcpdump
* Windows Firewall
* Windows process/network tools

## Skills Developed

* Windows administration
* Linux administration
* Virtualization
* Network configuration
* Network troubleshooting
* Firewall troubleshooting
* Network scanning
* Packet analysis
* Technical problem solving
