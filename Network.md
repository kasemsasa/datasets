# Network Documentation Report

**Company Name**: CSF432
**Date**: November 10, 2024  
**Prepared By**: Kasem Sasa
**Version**: 1.0

----------

## Table of Contents

1.  [Network Purpose Statement](#1-network-purpose-statement)
2.  [Network Overview](#2-network-overview)
3.  [Physical Network Diagram](#3-physical-network-diagram)
4.  [Network Devices and Configuration](#4-network-devices-and-configuration)
5.  [IP Addressing and Subnetting](#5-ip-addressing-and-subnetting)
6.  [Connection Medium and Demarcation Point](#6-connection-medium-and-demarcation-point)
7.  [Protocols and Services](#7-protocols-and-services)
8.  [Routing Policy](#8-routing-policy)
9.  [Backup and Security](#9-backup-and-security)
10.  [Business Continuity and Disaster Recovery](#10-business-continuity-and-disaster-recovery)

----------

### 1. Network Purpose Statement

The primary function of this network is to connect internal clients, DMZ servers, and administrators to facilitate internal operations, remote management, and secure access to external resources. The network supports business functions by providing essential communication, security, and data management capabilities. Key responsibilities include securing data, ensuring reliable access, and enabling managed access for external administrators.

----------

### 2. Network Overview

The network consists of a single location connected to an ISP. A Cisco Adaptive Security Appliance (ASA 5505) provides network security and Network Address Translation (NAT) to connect the internal corporate network, DMZ, and ISP. Three VLANs segment the network: Inside (for internal clients), Outside (connection to ISP), and DMZ (isolated servers). The network also includes three routers, simulating ISP layers and facilitating remote administration.

----------

### 3. Physical Network Diagram

**Diagram Description**:

-   **Cisco ASA 5505**: Primary edge security device connecting the network to the ISP.
-   **Switches**: Provide connections for LAN and DMZ areas.
-   **Routers (R1, R2, R3)**: Represent various ISP connection points and simulate external traffic.
-   **Devices**: PCs in the Inside and Outside networks, a server in the DMZ.

![enter image description here](https://github.com/kasemsasa/datasets/blob/03be3140fd7273591bf186e761926c301ff7cc76/network%20diagram.png)

----------

### 4. Network Devices and Configuration

#### 4.1 ASA 5505 (Adaptive Security Appliance)

-   **VLANs Configured**:
    -   VLAN 1 (Inside) - **IP**: 192.168.1.1 /24
    -   VLAN 2 (Outside) - **IP**: 209.165.200.226 /29
    -   VLAN 3 (DMZ) - **IP**: 192.168.2.1 /24
-   **Services**: DHCP, NAT, firewall

#### 4.2 Routers

-   **Router1 (R1)**
    -   G0/0: **IP**: 209.165.200.225 /29
    -   S0/0/0: **IP**: 10.1.1.1 /30 (Connected to R2)
-   **Router2 (R2)**
    -   S0/0/0: **IP**: 10.1.1.2 /30 (Connected to R1)
    -   S0/0/1: **IP**: 10.2.2.2 /30 (Connected to R3)
-   **Router3 (R3)**
    -   G0/1: **IP**: 172.16.3.1 /24
    -   S0/0/1: **IP**: 10.2.2.1 /30 (Connected to R2)

#### 4.3 Switches

-   **Switch0**: VLAN 1 - Connected to Inside PC-B.
-   **Switch1**: VLAN 3 - Connected to DMZ Server.
-   **Switch2**: VLAN for Outside network - Connected to PC-C.

#### 4.4 Devices

-   **DMZ Server**: IP - 192.168.2.3 /24, Gateway - 192.168.2.1 (ASA VLAN 3)
-   **PC-B (Inside)**: IP - 192.168.1.3 /24, Gateway - 192.168.1.1 (ASA VLAN 1)
-   **PC-C (Outside)**: IP - 172.16.3.3 /24, Gateway - 172.16.3.1 (R3)

----------


### 5. IP Addressing and Subnetting

| Device       | Interface | IP Address       | Subnet Mask       | Gateway       |
|--------------|-----------|------------------|--------------------|---------------|
| R1           | G0/0      | 209.165.200.225  | 255.255.255.248   | N/A           |
| R1           | S0/0/0    | 10.1.1.1         | 255.255.255.252   | N/A           |
| R2           | S0/0/0    | 10.1.1.2         | 255.255.255.252   | N/A           |
| R2           | S0/0/1    | 10.2.2.2         | 255.255.255.252   | N/A           |
| R3           | G0/1      | 172.16.3.1       | 255.255.255.0     | N/A           |
| R3           | S0/0/1    | 10.2.2.1         | 255.255.255.252   | N/A           |
| ASA VLAN 1   | E0/1      | 192.168.1.1      | 255.255.255.0     | N/A           |
| ASA VLAN 2   | E0/0      | 209.165.200.226  | 255.255.255.248   | N/A           |
| ASA VLAN 3   | E0/2      | 192.168.2.1      | 255.255.255.0     | N/A           |
| DMZ Server   | NIC       | 192.168.2.3      | 255.255.255.0     | 192.168.2.1   |
| PC-B         | NIC       | 192.168.1.3      | 255.255.255.0     | 192.168.1.1   |
| PC-C         | NIC       | 172.16.3.3       | 255.255.255.0     | 172.16.3.1    |

----------

### 6. Connection Medium and Demarcation Point

-   **Connection Medium**: Ethernet cables for internal devices; serial connections for inter-router (WAN) links.
-   **Demarcation Point**: ASA 5505, where the internal network connects to the ISP.

----------

### 7. Protocols and Services

-   **Protocols in Use**: IP, DHCP, NAT, ICMP (for troubleshooting), OSPF (for dynamic routing between routers).
-   **Network Services**: DHCP, NAT on ASA; firewalling on ASA; remote access enabled for external administrators.

----------

### 8. Routing Policy

-   **Routing**: OSPF is used for internal routing between routers R1, R2, and R3, ensuring dynamic and flexible path selection across the network.

----------

### 9. Backup and Security

#### 9.1 Backup

-   **Backup Strategy**: Server-based, differential backups to NAS device; regular weekly backups stored offsite for disaster recovery.

#### 9.2 Security

-   **Firewalls**: Cisco ASA 5505 configured with ACLs to filter inbound and outbound traffic.
-   **Anti-virus**: Centrally managed on client and server devices.
-   **Physical Security**: Access to network equipment limited to authorized personnel only.

----------

### 10. Business Continuity and Disaster Recovery

-   **Business Continuity Plan**: Utilizes backup power (UPS) for critical network devices, ensuring limited disruption during short power outages.
-   **Disaster Recovery Plan**: Offsite storage of critical backups, ASA and router configurations saved to a secure repository.
