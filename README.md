# 🛰️ GNS3 Ansible Automation — Campus MPLS Network (with L3VPN)

This project demonstrates a **fully automated Campus-MPLS network** using **Ansible** and **GNS3**, implementing **MPLS Layer 3 VPN (L3VPN)** for inter-site connectivity.  
It provisions routers, switches, and PE/CE edge devices to simulate a service-provider-style backbone network with campus sites connected over an MPLS core.

> 🖼️ *Topology Screenshot*  
> *(Insert image here: e.g. `![Topology Screenshot](topology.png)`)*

---

## 📦 Prerequisites

Before starting, ensure you have:
- GNS3 installed and configured  
- Ansible environment set up on your automation PC  
- Correct device images for routers and switches  

You can find the full list of required GNS3 images here:  
👉 [`requirements.txt`](./requirements.txt)

---

## ⚙️ Setup

Because of GNS3 export limitations, **the Ansible PC requires manual setup before automation**.

1. On your Ansible PC, open the setup file:

       nano ansiblegns3.yml

2. Copy the contents from this repository file:  
   👉 [`ansiblegns3.yml`](./ansiblegns3.yml)

3. Save and exit (`Ctrl + O`, then `Ctrl + X`).

If you want to review the **initial configuration for all network devices**, it’s available here:  
👉 [Initial Configurations Folder](./initial-configs)

*(Note: These are already preconfigured except for the Ansible PC.)*

---

## 🚀 Automation Execution

Once setup is complete, you can execute the Ansible playbook to automate the network configuration.

    ansible-playbook ansiblegns3.yml

Ansible will automatically push all necessary configurations to your devices according to the definitions in `gns3_vars.yml` and inventory files.

This playbook configures:
- Core MPLS infrastructure  
- PE–P and P–P label distribution  
- VRF instances and RD/RT assignments for **L3VPN**  
- CE–PE BGP peering for customer routes  

---

## 🧪 Testing

To verify connectivity and configuration, you can perform a simple ping test between CE devices or across VRFs.

Example command:

    ping 10.0.1.2

If you want full reachability across the entire MPLS VPN, **add the following BGP configuration lines** on both **CE1** and **CE2**:

    router bgp <ASN>
      network <prefix> mask <subnet-mask>

*(Adjust `<ASN>`, `<prefix>`, and `<subnet-mask>` based on your lab’s addressing.)*

Refer to the credentials file here for all login information:  
👉 [`credentials.txt`](./credentials.txt)

> 📸 *Sample Ping Output Screenshot*  
> *(Insert image here: e.g. `![Ping Test Screenshot](ping-test.png)`)*

**L3VPN Verification:**  
You can verify VRF connectivity and label bindings using the following commands on PE routers:

    show ip vrf
    show ip route vrf <vrf-name>
    show mpls forwarding-table
    show bgp vpnv4 all summary

---

## 🧩 Recommended Additions

You may consider adding the following sections later (these are commonly found in GitHub READMEs):

- **📁 Repository Structure:** Brief explanation of folder contents  
- **📚 Documentation:** Link to detailed setup or topology guide  
- **🧰 Troubleshooting:** Common errors (e.g. permission or export issues)  
- **📜 License:** Specify license type (MIT, GPL, etc.)  
- **🤝 Contributing:** Instructions for collaborators or PRs  

---

## 🧠 Notes

- This project is designed for **educational and lab use** only.  
- Implements **MPLS Layer 3 VPN (L3VPN)** with Ansible-driven provisioning.  
- Always verify IP addressing, image names, and topology consistency before executing automation.  
- Tested with GNS3 version *(add version here)* and Ansible version *(add version here)*.

---

**Author:** *Your Name*  
📧 *Optional: your contact or GitH*
