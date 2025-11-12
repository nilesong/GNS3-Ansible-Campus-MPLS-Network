# 🛰️ GNS3 Ansible Automation — Campus MPLS Network (with L3VPN)

This project demonstrates a **fully automated Campus-MPLS network** using **Ansible** and **GNS3**, implementing **MPLS Layer 3 VPN (L3VPN)** for inter-site connectivity.  
It provisions routers, switches, and PE/CE edge devices to simulate a service-provider-style backbone network with campus sites connected over an MPLS core.

<img width="2058" height="1038" alt="Topology" src="https://github.com/user-attachments/assets/7db38617-9159-4b79-83c7-5bb3092a3bac" />

---

## 📦 Prerequisites

Before starting, ensure you have:
- GNS3 installed and configured 
- GNS3 VM (VMWare) installed and configured 
- Network Automation appliance set up on your GNS3
- Correct device images for routers and switches  

You can find the full list of required GNS3 images here:  
👉 [`requirements.txt`](./requirements.txt)

---

## ⚙️ Setup

### Option 1 — For Ansible Automation
Import the following project file:
[`Campus-MPLS Network (For Ansible Automation).gns3project`](./Campus-MPLS%20Network%20(For%20Ansible%20Automation).gns3project)

Because of GNS3 export limitations, **the Ansible PCs are only partially preconfigured and requires manual setup before automation**.
Follow the steps below before executing the playbook.

1. Open and configure the **hosts** inventory file:

       nano hosts

   Copy the contents from this repository file:  
   Ansible-0: [`hosts`](./Ansible/Ansible-0/hosts)
   Ansible-1: [`hosts`](./Ansible/Ansible-1/hosts)
   Ansible-3: [`hosts`](./Ansible/Ansible-3/hosts)

2. Open and configure the **Ansible configuration file**:

       nano ansible.cfg

   Copy the contents from this repository file:  
   Ansible-0: [`ansible.cfg`](./Ansible/Ansible-0/ansible.cfg)
   Ansible-1: [`ansible.cfg`](./Ansible/Ansible-1/ansible.cfg)
   Ansible-3: [`ansible.cfg`](./Ansible/Ansible-3/ansible.cfg)

3. Open and configure the **main playbook file**:

       nano ansiblegns3.yml

   Copy the contents from this repository file:  
   Ansible-0: [`ansiblegns3.yml`](./Ansible/Ansible-0/playbooks/ansiblegns3.yml)
   Ansible-1: [`ansiblegns3.yml`](./Ansible/Ansible-1/playbooks/ansiblegns3.yml)
   Ansible-3: [`ansiblegns3.yml`](./Ansible/Ansible-3/playbooks/ansiblegns3.yml)

4. Open and configure the **variables file** for device information:

       nano gns3_vars.yml

   Copy the contents from this repository file:  
   Ansible-0: [`gns3_vars.yml`](./Ansible/Ansible-0/group_vars/gns3_vars.yml)
   Ansible-1: [`gns3_vars.yml`](./Ansible/Ansible-1/group_vars/gns3_vars.yml)
   Ansible-3: [`gns3_vars.yml`](./Ansible/Ansible-3/group_vars/gns3_vars.yml)

After completing these files, save and exit each (`Ctrl + O`, then `Ctrl + X`).

If you want to review the **initial configuration for all network devices**, it’s available here:  
👉 [Initial Configurations Folder](./Initial%20Config)

*(Note: These are already preconfigured except for the Ansible PCs)*

### Option 2 — Preconfigured and Ready for Testing
If you prefer a ready-to-use network without running Ansible, simply import:
[`Campus-MPLS Network (Configured).gns3project`](./Campus-MPLS%20Network%20(Configured).gns3project)

---

## 🚀 Automation Execution

> ⚠️ **Important:**  
> Execute the following command on **Ansible-0**, **Ansible-1**, and **Ansible-3** nodes only.

    ansible-playbook ansiblegns3.yml

---

### 🧠 What This Playbook Configures

This playbook builds a complete **Campus + MPLS** environment featuring both enterprise and service-provider elements.

#### 🏫 Campus Network Setup
- Three-tier design: **Access → Distribution → Core**
- **Distribution switches** run **HSRP** for VLAN SVI redundancy (load-balanced per VLAN)
- **Dist–Dist** links use **Layer 3 EtherChannel** interfaces (with IP addressing)
- **Dist–Dist**, **Dist–Core**, and **Core–Core** all run **OSPF** for internal routing
- **PCs** are segregated into VLANs for different sites:
  - **Site 1:**  
    - `VLAN100` → `10.1.100.x`  
    - `VLAN200` → `10.1.200.x`
  - **Site 2:**  
    - `VLAN100` → `10.2.100.x`  
    - `VLAN200` → `10.2.200.x`
  - **Site 3:**  
    - `VLAN100` → `10.3.100.x`  
    - `VLAN200` → `10.3.200.x`

#### 🌐 MPLS Core & VPN Setup
- Implements **MPLS L3VPN** for multi-site routing isolation  
- Autonomous Systems:
  - Site 1 & Site 2 → **AS65001**
  - Site 3 → **AS65002**
  - MPLS Core → **AS65000**
- Configures VRFs, Route Distinguishers (RDs), and Route Targets (RTs) for CE–PE separation  
- Automates BGP peering across MPLS and customer edges

---

## 💻 Testing & Additional Configurations

To verify network connectivity, perform the following **ping tests from `PC-V100-1`**.  
These confirm successful communication between VLANs and sites through the MPLS backbone.

    ping 10.1.200.11
    ping 10.1.100.12
    ping 10.1.200.12
    ping 10.2.100.11
    ping 10.2.200.11
    ping 10.2.100.12
    ping 10.2.200.12
    ping 10.3.100.11
    ping 10.3.200.11
    ping 10.3.100.12
    ping 10.3.200.12

<img width="872" height="163" alt="Ping" src="https://github.com/user-attachments/assets/98ef182d-6a98-4f9c-86f3-6c1b0219464f" />

If you want to enable **complete reachability across all network equipment** (routers and switches)  
in addition to PC connectivity, apply the **extra BGP configurations** on **CE1** and **CE2** as shown here:  
👉 [`CE BGP Additional.txt`](./CE%20BGP%20Additional.txt)

Use the following file for device access details when applying the configuration:  
👉 [`credentials.txt`](./CLI/credentials.txt)


---

## 🧠 Notes

- This project serves as a **demonstration of automated network provisioning and MPLS L3VPN deployment** using Ansible and GNS3.  
- Designed to showcase **infrastructure automation, multi-tier campus design, and service provider integration** in a simulated environment.  
- Implements **MPLS Layer 3 VPN (L3VPN)** with MP-BGP and OSPF configuration across campus and core networks.  
- Ensure all device images, IP addressing, and topology settings are verified before execution.  
- Tested with **GNS3 version 2.2.54** and **Ansible version 2.9.6**.


---

## 👤 Author

**Neil Ong**  

GitHub: [@nilesong](https://github.com/your-github-username)  
