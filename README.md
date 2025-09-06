# 🚀 InterVLAN Routing using Router-on-a-Stick

This project demonstrates how to configure **InterVLAN Routing** in Cisco Packet Tracer using a **Router-on-a-Stick** setup.

---

## 📘 Concept

- VLANs logically segment a network into smaller broadcast domains.  
- A **router** (or Layer 3 switch) is needed to allow communication between VLANs.  
- With **Router-on-a-Stick**, a single physical router interface is divided into multiple **sub-interfaces**, each representing a VLAN.  
- A **trunk link** between the switch and router carries traffic from multiple VLANs using **802.1Q encapsulation**.

---

## 🖥️ Topology

- **Router:** Cisco 2621XM  
- **Switches:** 2 × Cisco 2950-24  
- **PCs:** 4 end devices in 2 VLANs  

| VLAN | Subnet            | Gateway       | Devices         |
|------|------------------|---------------|----------------|
| 10   | 192.168.2.0/24   | 192.168.2.1   | PC0, PC2       |
| 20   | 192.168.3.0/24   | 192.168.3.1   | PC1, PC3       |

---

## ⚙️ Configuration Steps

### 1️⃣ Router (Router-on-a-Stick)

```bash
enable
configure terminal

! VLAN 10 sub-interface
interface fastEthernet 0/0.10
 encapsulation dot1Q 10
 ip address 192.168.2.1 255.255.255.0

! VLAN 20 sub-interface
interface fastEthernet 0/0.20
 encapsulation dot1Q 20
 ip address 192.168.3.1 255.255.255.0

! Enable main interface
interface fastEthernet 0/0
 no shutdown
###  2️⃣ Switch (VLAN and Trunk Setup)
enable
configure terminal

! Create VLANs
vlan 10
 name SALES
vlan 20
 name HR

! Assign ports to VLANs
interface fastEthernet 0/2
 switchport mode access
 switchport access vlan 10

interface fastEthernet 0/3
 switchport mode access
 switchport access vlan 20

! Trunk link to router
interface fastEthernet 0/1
 switchport mode trunk

! Trunk link to another switch
interface fastEthernet 0/24
 switchport mode trunk
```
## ✅ Verification Commands

### 🔹 On Router
```bash
show ip interface brief
ping 192.168.2.2
ping 192.168.3.2
```
### 🔹 On Switch
```bash
show vlan brief
show interfaces trunk
```
