# SMQ05: AWS Multi-VPC Transit Gateway Architecture

![Untitled Diagram](https://github.com/user-attachments/assets/ba657a81-d633-466c-a962-2ce6c20315d8)

## 📋 Project Overview
**SMQ05** demonstrates a secure, scalable "Hub-and-Spoke" network topology on AWS. 

The primary goal of this architecture is to isolate sensitive private resources (in a Private VPC) from the public internet while strictly managing egress traffic through a centralized point (the Public VPC) using **AWS Transit Gateway (TGW)**.

## 🏗️ Architecture Details

### 1. Network Topology
The network consists of two distinct Virtual Private Clouds (VPCs) connected via a Transit Gateway.

| Component | CIDR Block | Description |
| :--- | :--- | :--- |
| **Public VPC (Hub)** | `10.0.0.0/16` | Hosts the Bastion Host and NAT Gateway. Handles all internet ingress/egress. |
| **Public Subnet** | `10.0.0.0/24` | Contains resources that require direct internet access. |
| **Private VPC (Spoke)**| `20.0.0.0/16` | Hosts secure workloads. No direct internet access (No IGW). |
| **Private Subnet** | `20.0.0.0/24` | Contains the Private EC2 instance. |

### 2. Traffic Flow & Routing
* **Internet Access (Ingress):** Traffic enters via the Internet Gateway (IGW) attached to the Public VPC.
* **Internal Communication:** The Public and Private VPCs communicate securely over the **Transit Gateway**.
* **Private Egress:** The Private EC2 instance accesses the internet (e.g., for OS updates) by routing traffic to the TGW, which forwards it to the **NAT Gateway** in the Public VPC.

### 3. Security & Hardening
* **Bastion Access:** The Private EC2 instance does not accept SSH connections from the open internet (`0.0.0.0/0`).
* **Security Groups:**
    * **Private EC2 SG:** Inbound Rule allows Port 22 (SSH) **ONLY** from `10.0.0.0/16` (The Public VPC range).
    * This ensures that the private instance can only be accessed by jumping through the Public EC2 (Bastion) first.

## 🚀 Deployment Steps

1.  **Create the Public VPC (Hub)**
    * Deploy IGW and attach to VPC.
    * Deploy Public Subnet (`10.0.0.0/24`).
    * Launch Public EC2 and NAT Gateway.
2.  **Create the Private VPC (Spoke)**
    * Deploy Private Subnet (`20.0.0.0/24`).
    * Launch Private EC2.
3.  **Configure Transit Gateway**
    * Create TGW.
    * Create TGW Attachments for both Public and Private VPCs.
4.  **Update Route Tables**
    * **Public RT:** Route `20.0.0.0/16` → Transit Gateway.
    * **Private RT:** Route `0.0.0.0/0` → Transit Gateway.
    * **TGW Route Table:** Propagate routes for both VPCs.

## 🛠️ Tech Stack
* **Cloud Provider:** AWS
* **Networking:** VPC, Transit Gateway, NAT Gateway, Route Tables
* **Compute:** EC2 (Amazon Linux 2)
* **Security:** Security Groups, Network ACLs

---
*Created by SMQ05*
