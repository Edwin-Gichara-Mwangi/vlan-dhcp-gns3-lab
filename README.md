# 🖧 VLAN & DHCP Network Lab in GNS3

## 📌 Project Overview
This project demonstrates how to configure:
- VLAN segmentation (VLAN 10 for Staff, VLAN 20 for HR)
- Router-on-a-Stick inter-VLAN routing
- DHCP pools on the router for automatic IP assignment
- Switchport configurations (access & trunk)

The lab simulates a **small office network** where PCs in different departments are separated by VLANs but can still communicate through a router.

---
## 🖼️ Network Topology
![Topology]
<img width="1012" height="598" alt="Screenshot from 2025-09-18 19-47-38" src="https://github.com/user-attachments/assets/ad763e9b-32aa-4d9d-b632-0e33dad5e0ef" />
*Components:**
- **Router:** Provides inter-VLAN routing + DHCP
- **Switch:** VLAN creation, trunk + access ports
- **PCs:** Test hosts that receive IPs via DHCP

## ⚙️ Configurations

### 🔹 Router
```bash
! VLAN 10 DHCP Pool
ip dhcp pool STAFF
 network 192.168.1.0 255.255.255.128
 default-router 192.168.1.1
 dns-server 8.8.8.8

! VLAN 20 DHCP Pool
ip dhcp pool HR DEPARTMENT
 network 192.168.1.128 255.255.255.128
 default-router 192.168.1.129
 dns-server 8.8.8.8

! Router Sub-interfaces
interface FastEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.1.1 255.255.255.128

interface FastEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.1.129 255.255.255.128
```

### Switch
```bash
! VLAN Creation
vlan 10
 name STAFF
vlan 20
 name HR-DEPARTMENT

! Access Ports
interface range Ethernet0/0 - 1/0
 switchport mode access
 switchport access vlan 10

interface range Ethernet1/1 - 2/1
 switchport mode access
 switchport access vlan 20

! Trunk Port
interface Ethernet2/2
 switchport mode trunk
 switchport trunk allowed vlan 10,20
```
### How to Run

Clone this repository.

Open vlan-dhcp-lab.gns3project.zip in GNS3.
https://github.com/Edwin-Gichara-Mwangi/vlan-dhcp-gns3-lab/releases/download/v1.0.0/GNS3.NETWORK.PROJECT.zip

Start the router, switch, and PCs.

On the PCs, request an IP via DHCP:
PC1> sudo udhcp -i eth0
PC1> show ip



