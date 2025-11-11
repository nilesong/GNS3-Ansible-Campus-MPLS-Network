# 🛰️ GNS3 Ansible Automation — Campus MPLS Network

This project demonstrates a **fully automated Campus-MPLS network** using **Ansible** and **GNS3**.  
It provisions routers, switches, and core MPLS components automatically — ideal for network automation labs and testing environments.

> 🖼️ *Topology Screenshot*  
> *(Insert image here: e.g. `![Topology Screenshot](topology.png)`)*

---

## 📦 Prerequisites

Before starting, ensure you have:
- GNS3 installed and configured
- GNS3 VM (VMWare) installed and configured
- Network Automation appliance environment set up on your GNS3
- Correct device images for routers and switches

You can find the full list of required GNS3 images here:  
👉 [`requirements.txt`](./requirements.txt)

---

## ⚙️ Setup

Because of GNS3 export limitations, **the Ansible PCs requires manual setup before automation**.

1. On Ansible-0, Ansible-1 & Ansible-3, open the setup file:

   ```bash
   nano ansiblegns3.yml
