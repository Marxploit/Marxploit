# Windows & Kali Incident Response Lab

This project simulates a realistic internal network attack within an isolated home lab environment, demonstrating incident response skills using Splunk, Sysmon, and attacker tools such as Nmap and Metasploit. It highlights the full workflow from attack to detection and documentation.

## Overview

- **Environment:** Oracle VirtualBox
- **Virtual Machines:** 
  - Windows 10 (target)
  - Kali Linux (attacker)
- **Tools Used:**
  - Splunk Enterprise
  - Sysmon
  - Splunk Universal Forwarder
  - Nmap
  - Metasploit / msfvenom
- **Network Setup:** Internal NAT with manually assigned static IPs

## Objectives

- Deploy Windows 10 and Kali Linux virtual machines
- Configure Splunk Universal Forwarder on the Windows VM
- Install and configure Sysmon for endpoint-level event capture
- Simulate a cyber attack using:
  - Nmap (reconnaissance)
  - msfvenom and Metasploit (exploit delivery and shell access)
- Detect and analyze all activity using Splunk
- Troubleshoot logging and connectivity issues
- Create professional-level documentation reflecting all phases

## Architecture

[Windows 10 VM] ← internal-only VirtualBox network → [Kali Linux VM]
│
Sysmon
↓
Splunk Universal Forwarder
↓
Splunk Enterprise Listener


## Lab Setup

- **Splunk Input Configuration:** A custom `input.conf` was used to collect logs from multiple sources, including Sysmon, Security, Application, PowerShell, Defender, and System.
- **Sysmon Deployment:** Installed on the Windows VM using a PowerShell script and custom XML configuration to ensure detailed event logging.
- **Firewall Adjustments:** The Windows firewall was modified to allow ICMP traffic and enable payload reception for simulation.

## Attack Simulation

**1. Reconnaissance**  
Performed an Nmap scan from Kali to identify open ports on the Windows host. The scan returned no open ports, validating the system’s external stealth.

**2. Payload Creation and Delivery**  
A reverse TCP Meterpreter shell was generated using msfvenom and hosted on a simple HTTP server running on Kali. The Windows host downloaded and executed the payload.

**3. Exploitation**  
After execution, the payload established a reverse shell connection back to the Metasploit handler on Kali.

## Detection and Analysis

- All malicious activity was captured and indexed in Splunk.
- Sysmon and Security logs provided visibility into:
  - Process creation
  - Network connections
  - Executable payloads
- Custom Splunk queries were used to track the attacker’s behavior throughout the lab session.

## Troubleshooting Highlights

- **Networking:** Solved VM communication issues by correctly configuring the internal adapter and assigning static IPs.
- **Log Forwarding:** Diagnosed missing logs by reviewing and correcting the `input.conf` and ensuring the Universal Forwarder service was running.
- **Attack Sequence:** Adjusted the order of operations to ensure logs reflected a realistic attacker timeline.

## Lessons Learned

- Built a functioning end-to-end detection pipeline using enterprise tools
- Improved problem-solving by working through realistic setup and misconfiguration errors
- Practiced red team and blue team workflows within the same project
- Developed a deeper understanding of how log data flows into Splunk from the endpoint
- Learned how to validate attacker activity and correlate it with defensive visibility

## Report

A detailed PDF report including configuration, screenshots, analysis, and takeaways is available here:

[Download Full Report (PDF)](./windows-kali-report.pdf)
