
# 🔐 Secure Enterprise Network – VPN, Firewall & Network Security

A comprehensive **enterprise network security project** designed, configured, and tested using **Cisco Packet Tracer**. The project integrates secure routing, site-to-site IPsec VPN connectivity, firewall policies, network segmentation, access control, authentication, and centralized monitoring.

The implementation demonstrates practical experience with **Cisco networking, network security, routing protocols, VPN technologies, and enterprise infrastructure configuration**.

## 📌 Project Overview

This project focuses on designing and securing a multi-segment enterprise network containing internal networks, remote sites, guest networks, an ISP connection, and a demilitarized zone (DMZ).

The network combines multiple security mechanisms to protect communication, restrict unauthorized access, secure device management, and maintain connectivity between authorized network segments.

### Key Features

- IPv4 addressing and enterprise network configuration
- Dynamic routing using OSPF
- OSPF routing authentication
- Secure remote device management using SSH
- Site-to-site IPsec VPN between remote networks
- Zone-Based Policy Firewall (ZPF)
- Stateless packet filtering using extended ACLs
- Cisco ASA firewall configuration
- Network Address Translation (NAT/PAT)
- VLAN and switch port security
- AAA authentication with RADIUS configuration
- DMZ segmentation and controlled server access
- DHCP, DNS, FTP, email, and web services
- Centralized Syslog monitoring and event timestamping

## 🏗️ Network Architecture

The enterprise network consists of multiple interconnected components:

| Component | Purpose |
|---|---|
| R1 | Internal routing, VPN, ACLs, and authentication |
| R2 | Secure routing, VPN, and zone-based firewall |
| ISP Router | Connectivity between network segments |
| SW1 & SW2 | Access-layer switching and port security |
| Cisco ASA | Stateful firewall, NAT, and DMZ security |
| Internal LAN | Trusted enterprise devices |
| Guest Network | Restricted external connectivity |
| DMZ | Controlled access to public-facing services |
| Network Servers | DNS, DHCP, FTP, email, web, and logging services |

The architecture demonstrates layered security across routing, switching, authentication, and application services.

## ⚙️ Implementation

### 1. IP Addressing & Network Configuration

- Configured static IPv4 addresses, subnet masks, and default gateways.
- Assigned IP addresses to router and switch interfaces.
- Configured network services and host connectivity.
- Verified interface status and connectivity using Cisco CLI commands.

### 2. OSPF Dynamic Routing

Configured **Open Shortest Path First (OSPF)** to enable communication between enterprise networks.

- Implemented OSPF process 10 across R1, R2, and the ISP router.
- Advertised relevant networks in Area 0.
- Configured default route propagation.
- Verified OSPF neighbor relationships and routing tables.

**Routing Security:** Implemented OSPF message-digest authentication to protect routing exchanges.

The original requirement specified SHA-256; however, Cisco Packet Tracer limitations required using the supported message-digest authentication mechanism instead.

### 3. Secure Remote Management (SSH)

Configured SSH on network devices to provide authenticated and encrypted remote administration.

- Configured device hostnames and domain names.
- Generated RSA keys.
- Created local administrative accounts.
- Restricted VTY access to SSH.
- Verified successful remote administration.

### 4. Layer 2 Switch Security

Implemented multiple switch security mechanisms:

- **Unused Port Protection:** Disabled unused switch interfaces and assigned them to a dedicated BlackHole VLAN (888).
- **Port Security:** Enabled sticky MAC address learning and MAC address limits.
- **Violation Handling:** Applied restrict mode to block unauthorized MAC addresses.
- **PortFast:** Optimized endpoint connectivity.
- **BPDU Guard:** Protected access ports against unexpected STP messages.
- **DHCP Snooping:** Restricted unauthorized DHCP activity and configured trusted interfaces.

These controls protect the access layer against unauthorized connections and common Layer 2 attacks.

### 5. Site-to-Site IPsec VPN

Configured a **site-to-site IPsec VPN** between R1 and R2 to protect communication between remote LANs over the ISP network.

Implementation included:

- Defining interesting traffic with ACLs.
- Configuring IKE Phase 1 policies.
- Configuring IPsec Phase 2 policies.
- Creating IPsec transform sets.
- Applying crypto maps to router interfaces.
- Testing connectivity before and after VPN configuration.

The Packet Tracer implementation used AES encryption and authentication settings supported by the simulation environment.

**Verification:** Confirmed successful VPN operation by generating traffic between the two LANs and observing increasing encrypted/decrypted packet counters using:

```bash
show crypto ipsec sa
```

### 6. Zone-Based Policy Firewall (ZPF)

Configured a **Zone-Based Policy Firewall on R2** to control communication between different security zones.

| Source | Destination | Security Policy |
|---|---|---|
| INSIDE | OUTSIDE | Permit and inspect authorized traffic |
| GUEST | OUTSIDE | Restrict to HTTP, HTTPS, and DNS |
| OUTSIDE | INSIDE | Permit only specified VPN traffic |
| OUTSIDE | GUEST | Block unsolicited access |

Configuration involved:

- Security zones
- Class maps
- Policy maps
- Zone pairs
- Traffic inspection policies
- Interface-to-zone assignments

The firewall enforces network segmentation and least-privilege access.

### 7. Stateless Firewall Using Extended ACLs

Implemented a named extended ACL on R1 to enforce protocol-specific security restrictions.

Configured policies to:

- Block HTTP/HTTPS access from a specified host to the web server.
- Restrict FTP access from a specified host.
- Deny selected ICMP traffic.
- Permit remaining authorized traffic.

The ACL was applied inbound on the interface closest to the traffic source.

**Verification:** Tested permitted and denied traffic and checked ACL hit counters to confirm correct filtering.

### 8. Cisco ASA Firewall Configuration

Configured a Cisco Adaptive Security Appliance (ASA) firewall using the command-line interface.

Features included:

- INSIDE, OUTSIDE, and DMZ interfaces
- Interface security levels
- Stateful inspection using Modular Policy Framework (MPF)
- Static routing
- NAT and Port Address Translation (PAT)
- DHCP services
- SSH management
- Local AAA authentication
- Static NAT for a DMZ server
- Access control for public-facing services

These configurations demonstrate how a dedicated firewall can provide traffic inspection, address translation, and security boundaries.

### 9. AAA Authentication & RADIUS

Configured AAA authentication policies with RADIUS server integration and local authentication fallback.

- Defined centralized authentication settings.
- Configured local administrative credentials as backup.
- Examined RADIUS server and client configuration.
- Applied authentication method lists.

**Simulation Note:** Cisco Packet Tracer imposed limitations on supported RADIUS CLI commands, so the configuration and verification followed the capabilities of the simulation environment.

### 10. Enterprise Network Services

Configured and tested several network services:

| Service | Function |
|---|---|
| DHCP | Dynamic IP address allocation |
| DNS | Domain-name resolution |
| FTP | Authenticated file transfer |
| HTTP | Internal/public web access |
| SMTP/POP3 | Email sending and receiving |
| Syslog | Centralized collection of network events |

Connectivity and service accessibility were verified using simulated client devices.

### 11. Network Monitoring & Logging

Implemented centralized Syslog monitoring for network events.

- Configured networking devices to forward logs to a Syslog server.
- Generated network events by changing interface states.
- Verified that Syslog messages were successfully collected.
- Configured device clocks and logging timestamps.
- Verified timestamped logging for troubleshooting and auditing.

## 🧪 Verification & Testing

Network functionality and security policies were validated using Cisco Packet Tracer simulation and CLI verification commands.

| Test | Result |
|---|---|
| IPv4 connectivity | Successful |
| OSPF neighbor establishment | Verified |
| SSH remote access | Successful |
| Site-to-site IPsec VPN | Encrypted/decrypted traffic verified |
| ZPF guest restrictions | Enforced |
| Extended ACL filtering | Denied traffic and hit counters verified |
| ASA interface and NAT configuration | Verified |
| DHCP address assignment | Successful |
| DNS and web services | Successful |
| FTP authentication | Successful |
| Email communication | Successful |
| Centralized Syslog collection | Successful |

RADIUS configuration was also explored, subject to Packet Tracer's command-support limitations.

### Example Verification Commands

```bash
show ip interface brief
show ip route
show ip ospf neighbor
show ip ospf interface
show crypto ipsec sa
show access-lists
show running-config
show nat
```

## 🛠️ Technologies & Skills

**Simulation Environment**
- Cisco Packet Tracer

**Routing & Networking**
- IPv4 Addressing
- Subnetting
- OSPF
- Static & Default Routing
- VLANs

**Network Security**
- IPsec VPN
- IKE / ISAKMP
- Zone-Based Policy Firewall
- Cisco ASA Firewall
- Extended ACLs
- SSH
- Port Security
- DHCP Snooping
- BPDU Guard
- NAT / PAT

**Authentication & Monitoring**
- AAA
- RADIUS
- Syslog
- Network Logging
- Traffic Inspection

**Network Services**
- DHCP
- DNS
- HTTP
- FTP
- SMTP / POP3

## 📁 Repository Contents

This repository includes:

- **Cisco Packet Tracer project (.pka):** The network simulation and configuration activity.
- **Network Security Project Report (.pdf):** Detailed implementation documentation, configuration screenshots, testing procedures, and verification results.
- **README.md:** Project overview and technical summary.

The report provides step-by-step evidence of the configurations and tests performed throughout the project.

## 🚀 How to Explore the Project

1. Install a compatible version of **Cisco Packet Tracer**.
2. Clone or download this repository.
3. Open the `.pka` project file using Cisco Packet Tracer.
4. Inspect the network topology and device configurations.
5. Review router, switch, and ASA settings through the CLI.
6. Reproduce the documented connectivity and security tests.
7. Refer to the PDF report for configuration explanations and verification screenshots.

**Note:** This is an academic network simulation, not a production infrastructure deployment. Certain security features use legacy or simulator-supported mechanisms.

## 🎯 Key Outcomes

- Designed and configured a multi-segment enterprise network simulation.
- Established and verified encrypted site-to-site VPN connectivity.
- Implemented layered security using ACLs, zone-based firewalls, and Cisco ASA policies.
- Applied access-layer security and network segmentation.
- Configured authenticated remote management and network services.
- Validated security policies through connectivity tests, CLI inspection, and traffic monitoring.
- Documented implementation steps and verification results.

## 🔮 Future Improvements

- Migrate the design to a platform supporting modern IKEv2 and stronger authentication algorithms.
- Implement automated network configuration validation.
- Introduce centralized security monitoring and alerting.
- Evaluate the architecture in a virtualized enterprise networking environment.

## 🏁 Conclusion

This project demonstrates practical experience in **enterprise networking, Cisco device configuration, secure routing, VPN technologies, firewall administration, network segmentation, and infrastructure monitoring**.

By integrating routing, encryption, access control, authentication, and network services, the project illustrates how multiple security mechanisms work together to protect an enterprise-style network.
