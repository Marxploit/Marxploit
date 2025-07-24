# Cisco Network Topology Simulation with Syslog and ACLs

A Cisco Packet Tracer lab simulating VLAN segmentation, router-on-a-stick routing, ACL-based access control, and centralized Syslog logging to model a basic SOC (Security Operations Center) environment.

## Project Overview

This simulation demonstrates how to design a secure segmented network with VLANs and ACLs while incorporating logging best practices via a centralized Syslog server.

The lab is ideal for entry-level information security professionals seeking hands-on experience in network segmentation, access control, and event monitoring.

## Features

- Segmented VLAN architecture across switches
- Router-on-a-stick inter-VLAN routing
- Access Control Lists (ACLs) to enforce network isolation
- Centralized Syslog logging from all devices
- Static IP assignment and default gateway configuration
- Basic SOC simulation with DMZ and Management zones

## Requirements

- Cisco Packet Tracer 8.x or later
- Devices used:

    - Cisco 2911 Router (EdgeRouter)
    - Two Cisco 2960 Switches (SW1, SW2)
    - 2 User PCs (VLAN 10)
    - 1 Internal Server (VLAN 20)
    - 1 Web Server (DMZ - VLAN 30)
    - 1 Admin Workstation and 1 Syslog Server (VLAN 99)

## File Structure

network-topology-lab  
| File/Asset         | Description                                         |
|:-------------------|:----------------------------------------------------|
| `topology.pkt`     | Cisco Packet Tracer file with full configuration    |
| `README.md`        | Project documentation                               |
| `lab-notes.txt`    | Configuration notes and IP addressing plan          |
| `syslog-server.cfg`| Example syslog configuration for the Linux server   |

## VLAN Configuration

    VLAN 10: User PCs  
    VLAN 20: Internal Server  
    VLAN 30: DMZ (Web Server)  
    VLAN 99: Management (Admin PC + Syslog Server)

## Router Configuration (Router-on-a-Stick)

Each VLAN is configured as a sub-interface on the router’s trunk port:

    interface GigabitEthernet0/0.10
      encapsulation dot1Q 10
      ip address 192.168.10.1 255.255.255.0

    interface GigabitEthernet0/0.20
      encapsulation dot1Q 20
      ip address 192.168.20.1 255.255.255.0

    interface GigabitEthernet0/0.30
      encapsulation dot1Q 30
      ip address 192.168.30.1 255.255.255.0

    interface GigabitEthernet0/0.99
      encapsulation dot1Q 99
      ip address 192.168.99.1 255.255.255.0

## Access Control

    access-list 100 deny ip 192.168.10.0 0.0.0.255 192.168.99.0 0.0.0.255
    access-list 100 permit ip any any

    interface GigabitEthernet0/0.10
      ip access-group 100 in

ACL 100 blocks access from VLAN 10 to VLAN 99, preventing unauthorized access to the management network.

## Syslog Configuration

    Router(config)# logging 192.168.99.100
    Switch(config)# logging 192.168.99.100

Syslog server at `192.168.99.100` is configured to receive logs from all devices. Events such as port status changes are logged for audit and troubleshooting.

## Testing & Validation

    - Verified that User1 in VLAN 10 could not access Admin PC in VLAN 99
    - Verified connectivity between VLAN 10 and Web Server in VLAN 30
    - Simulated port down event to generate log message on Syslog server

## Troubleshooting

    - Syslog not receiving logs: Verified IP address and syslog daemon running
    - Devices not routing: Ensured correct sub-interface and encapsulation setup
    - ACL not working: Verified access-group placement and ACL syntax

## Lessons Learned

    - VLANs are fundamental to segmenting enterprise networks
    - ACLs enforce security boundaries effectively
    - Router-on-a-stick simplifies inter-VLAN routing for small networks
    - Centralized logging is essential for monitoring and compliance
    - Proper configuration validation is critical at each stage
