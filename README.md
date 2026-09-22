\# Cisco Network Security Configuration

\## Overview

The project simulates the role of a Security Administrator responsible for securing an existing enterprise network infrastructure. The network includes multiple routers, Layer 2 switches, a Cisco ASA firewall, internal networks, a management network, a DMZ, servers and client devices.

The practical implementation covers a range of network security controls, including:

\- Layer 2 switch security

\- Secure SSH administration

\- AAA authentication

\- TACACS+

\- RADIUS

\- 802.1X authentication

\- VLAN and trunk security

\- Port security

\- NTP

\- SYSLOG

\- Cisco ASA firewall configuration

\- NAT and PAT

\- DMZ configuration

\- Static NAT

\- Access Control Lists (ACLs)

\- Zone-Based Policy Firewall

\- Secure management access

\- Network security verification

\## Network Topology

The simulated network contains:

\- Cisco routers

\- Cisco Layer 2 switches

\- Cisco ASA firewall

\- Internal LAN networks

\- Management network

\- DMZ

\- External network

\- Web/DNS server

\- Management services server

\- Mail server

\- Client PCs

The topology separates users, management systems, servers and publicly accessible services into different network segments.

\### Network Addressing

| Network | Purpose |

|---|---|

| \`192.168.1.0/24\` | Internal Edinburgh LAN |

| \`192.168.2.0/24\` | Internal user network |

| \`192.168.3.0/24\` | Management network |

| \`172.16.1.0/24\` | DMZ |

| \`10.3.3.0/24\` | Internal server network |

| \`209.165.200.224/29\` | External/public network |

| \`10.1.1.0/30\` | Router-to-router connection |

| \`10.1.1.4/30\` | Router-to-router connection |

\### Router Addressing

| Device | Interface | Address |

|---|---|---|

| R1 | G0/0 | \`209.165.200.234/29\` |

| R1 | G0/1 | \`10.1.1.1/30\` |

| R2 | G0/0 | \`10.1.1.2/30\` |

| R2 | G0/1 | \`10.3.3.1/24\` |

| R2 | G0/2 | \`10.1.1.5/30\` |

| R3 | G0/0 | \`192.168.2.1/24\` |

| R3 | G0/1 | \`192.168.3.1/24\` |

| R3 | G0/2 | \`10.1.1.6/30\` |

\### ASA Addressing

| Interface | Network | Address |

|---|---|---|

| G1/1 | INSIDE | \`192.168.1.1/24\` |

| G1/2 | OUTSIDE | \`209.165.200.233/29\` |

| G1/3 | DMZ | \`172.16.1.1/24\` |

\### Switch Management Addresses

| Device | Address |

|---|---|

| S1 | \`192.168.1.2/24\` |

| S2 | \`172.16.1.2/24\` |

| S3 | \`192.168.2.2/24\` |

| S4 | \`192.168.2.3/24\` |

| S5 | \`192.168.3.2/24\` |

\### Servers

| Server | Address | Purpose |

|---|---|---|

| WEB/DNS Server | \`10.3.3.10/24\` | Web and DNS services |

| DMZ Web Server | \`172.16.1.10/24\` | Public-facing web service |

| Mail Server | \`172.16.1.11/24\` | Mail services |

| Management Server | \`192.168.3.10/24\` | SYSLOG, NTP, TACACS+ and RADIUS |

\## Layer 2 Switch Security

The Layer 2 switches were secured against common access-layer and management security risks.

The configuration included:

\- Encrypted privileged EXEC credentials

\- Local administrator accounts

\- SSH version 2

\- Domain name configuration

\- RSA key generation

\- SSH timeout configuration

\- SSH authentication using the local database

\- Native VLAN configuration

\- Internal VLAN configuration

\- Trunk security

\- Port security

\- Sticky MAC address learning

\- Maximum MAC address limits

\- Port-security violation handling

\- PortFast

\- BPDU Guard

\- Shutdown of unused switch ports

\- 802.1X configuration

\### Port Security

Port security was configured on the required access ports.

The ports were configured for access mode and assigned to the required VLAN.

Dynamic sticky MAC address learning was enabled to associate connected devices with their switch ports.

A maximum of two MAC addresses was configured on the relevant port.

The violation action was configured to shut down the interface if the security policy was violated.

\### Spanning Tree Protection

PortFast and BPDU Guard were configured on the required access port.

This provides additional protection for edge ports by preventing unexpected spanning-tree participation.

\### Unused Ports

Unused switch interfaces were assigned to the designated unused/native VLAN and administratively shut down.

This reduces the number of active interfaces available for unauthorised network access.

\## Secure Router Administration

R1 was configured for secure remote administration.

The configuration included:

\- Enable secret

\- Local administrator account

\- SSH version 2

\- \`netsec.com\` domain

\- 1024-bit RSA keys

\- SSH timeout

\- Maximum SSH authentication retries

\- TACACS+ authentication

\- Local authentication fallback

\- Restricted VTY access

VTY access was restricted to the management network.

This means remote administration is controlled both by authentication and by the source network from which the connection originates.

\## AAA and TACACS+

AAA was configured to provide centralised authentication for administrative access.

The management server at:

\`\`\`text

192.168.3.10

was configured as the TACACS+ server.

The router was configured to use TACACS+ authentication with local authentication available as a fallback.

This demonstrates the use of centralised authentication for network device administration.

**TACACS+ Configuration**

The configured TACACS+ server was:

192.168.3.10

The project also included the required TACACS+ authentication credentials and shared key.

**Restricted VTY Access**

Remote VTY access on R1 was restricted to the management network:

192.168.3.0/24

This provides an additional access-control layer for remote administration.

Even with valid credentials, administrative access is restricted according to the configured source network.

**Privilege View**

A custom administrative view was configured for the required administrator account.

The view provides access to selected commands including:

-   show
-   Configuration commands
-   Debugging commands

This demonstrates the use of controlled administrative privileges rather than providing unrestricted access to every available command.

**NTP Configuration**

Network Time Protocol was configured on the required routers.

The management server:

192.168.3.10

was configured as the NTP server.

NTP authentication was also configured using the required authentication key.

Synchronised time is important for network security monitoring because events recorded across multiple devices can be correlated using consistent timestamps.

**NTP Verification**

NTP status and associations were verified using commands such as:

show ntp status

show ntp associations

**SYSLOG Configuration**

SYSLOG was configured on the required routers to send logging information to the central management server:

192.168.3.10

The logging level was configured to capture debugging-level messages as required.

SYSLOG provides centralised visibility of events occurring across the network infrastructure.

The configuration was tested by generating a network event and verifying that the corresponding log information was received by the SYSLOG server.

**Cisco ASA Firewall**

The Cisco ASA was configured to provide firewall functionality between the external network, internal network and DMZ.

The ASA interfaces were configured as:

G1/1 - INSIDE

G1/2 - OUTSIDE

G1/3 - DMZ

Security levels were assigned to the interfaces according to their role.

The ASA was also configured with:

-   Hostname
-   Domain name
-   Enable password
-   Local administrator account
-   Default route
-   NAT/PAT
-   DHCP
-   DNS configuration
-   SSH management
-   RSA keys
-   SSH timeout

**NAT and PAT**

Port Address Translation was configured to allow internal hosts to communicate through the external interface.

The internal network traffic is translated using the configured public address on the ASA outside interface.

This demonstrates the use of address translation between private internal addressing and the public-facing network.

**ASA Policy Inspection**

An ASA policy map was configured to inspect the required protocols.

The configuration included inspection for:

-   HTTP
-   HTTPS
-   DNS

The default global service policy was replaced with the configured security policy.

This demonstrates the use of Cisco ASA policy inspection for application-layer traffic.

**DHCP Configuration**

DHCP was configured on the ASA for the internal network.

The DHCP address pool was configured as:

192.168.1.10 - 192.168.1.100

The DNS server was configured as:

10.3.3.2

This allows internal clients to obtain their network configuration dynamically.

**Secure ASA Administration**

SSH access to the ASA was configured using:

-   RSA keys
-   SSH timeout
-   Local authentication
-   Restricted management access

The required internal hosts were permitted to administer the ASA remotely.

This prevents unrestricted SSH access to the firewall.

**DMZ Configuration**

A dedicated DMZ was configured on the ASA.

The DMZ interface uses:

172.16.1.1/24

with a security level of:

70

The DMZ separates externally accessible services from the internal LAN.

**DMZ Web Server**

The DMZ web server uses:

172.16.1.10/24

The server provides the web service that is made accessible through the firewall and static NAT configuration.

**Mail Server**

A mail server is also located within the DMZ:

172.16.1.11/24

**Static NAT**

Static NAT was configured for the DMZ web server.

**Private Address**

172.16.1.10

**Public Address**

209.165.200.235

This allows the DMZ web server to be accessed using its configured public address.

**DMZ Access Control List**

A named ACL was configured to control access to the DMZ web server.

The required public services were permitted:

-   HTTP
-   HTTPS
-   DNS

Traffic from external hosts was controlled through the configured ACL rather than allowing unrestricted access to the DMZ.

The web service was verified using:

https://web.netsec.com

**Zone-Based Policy Firewall**

R3 was configured with a Cisco Zone-Based Policy Firewall.

The interfaces were assigned to the following zones:

G0/0 - INSIDE

G0/1 - MANAGEMENT

G0/2 - OUTSIDE

The following security zones were created:

-   INSIDE
-   MANAGEMENT
-   OUTSIDE

Traffic policies were then created to control communication between these zones.

**User Network Security Policy**

The user network:

192.168.2.0/24

was configured with a policy allowing the required traffic towards the outside network.

The permitted services include:

-   HTTP
-   HTTPS
-   DNS

This demonstrates the use of zone-based policies to control traffic according to both source network and service.

**Management Network Security Policy**

The management network:

192.168.3.0/24

was configured to permit the required services towards the outside network.

These include:

-   HTTP
-   HTTPS
-   DNS
-   ICMP
-   SSH

Additional policies were configured to allow the management network to communicate with the required infrastructure services, including:

-   SYSLOG
-   NTP
-   TACACS+

**Self-Zone Protection**

The Zone-Based Policy Firewall was also configured to control traffic destined for the router itself.

ICMP traffic from the Internet towards the router's self-zone was restricted as required by the assessment.

This provides an additional layer of protection for the router and its own services.

**802.1X and RADIUS**

802.1X authentication was configured for the required access port on the Glasgow LAN.

The RADIUS server is located at:

192.168.3.10

The switch uses the RADIUS server to authenticate the connecting client.

**Authentication Process**

Client

   |

   v

Switch

   |

   | 802.1X

   v

RADIUS Server

192.168.3.10

   |

   v

Authentication Result

This demonstrates the use of network access control to authenticate devices before allowing network access.

**Verification and Testing**

The completed configuration was verified using Cisco Packet Tracer and the command-line interfaces available on the network devices.

Testing included:

-   SSH connectivity
-   Local authentication
-   TACACS+ authentication
-   Restricted VTY access
-   NTP synchronisation
-   SYSLOG message generation
-   DNS resolution
-   Web connectivity
-   NAT/PAT functionality
-   DMZ connectivity
-   Static NAT
-   ACL behaviour
-   Zone-Based Policy Firewall policies
-   Switch port security
-   Sticky MAC learning
-   802.1X authentication
-   RADIUS authentication

**Example Verification Commands**

show running-config

show ip interface brief

show ip route

show vlan brief

show interfaces trunk

show port-security

show port-security interface

show access-lists

show logging

show ntp status

show ntp associations

The assessment verification also included testing the web service, TACACS+ authentication, SYSLOG messages and 802.1X/RADIUS authentication.

**Security Concepts Demonstrated**

|
**Security Area**

 |

**Implementation**

 |
| --- | --- |
|

Secure management

 |

SSHv2

 |
|

Authentication

 |

Local authentication

 |
|

Centralised authentication

 |

TACACS+

 |
|

Network access control

 |

802.1X / RADIUS

 |
|

Layer 2 security

 |

Port security

 |
|

MAC control

 |

Sticky MAC addresses

 |
|

STP security

 |

BPDU Guard

 |
|

Edge-port protection

 |

PortFast

 |
|

Unused interface protection

 |

Shutdown / unused VLAN

 |
|

Firewalling

 |

Cisco ASA

 |
|

Traffic filtering

 |

ACLs

 |
|

Application inspection

 |

ASA policy inspection

 |
|

Address translation

 |

NAT/PAT

 |
|

Server isolation

 |

DMZ

 |
|

Public service publishing

 |

Static NAT

 |
|

Traffic segmentation

 |

Security zones

 |
|

Router firewalling

 |

Zone-Based Policy Firewall

 |
|

Centralised logging

 |

SYSLOG

 |
|

Time synchronisation

 |

NTP

 |
|

Network configuration

 |

DHCP

 |
|

Name resolution

 |

DNS

 |
|

Management restrictions

 |

VTY access control

 |

**Technologies Used**

-   Cisco Packet Tracer
-   Cisco IOS
-   Cisco ASA
-   IPv4
-   SSH
-   AAA
-   TACACS+
-   RADIUS
-   802.1X
-   VLANs
-   802.1Q trunking
-   Port Security
-   NAT
-   PAT
-   Access Control Lists
-   Zone-Based Policy Firewall
-   NTP
-   SYSLOG
-   DHCP
-   DNS
