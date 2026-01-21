Goal: Technical documentation and reproducibility. Tone: Concise, instructional, and structured.

Title: AWS Multi-VPC Transit Gateway Network Subtitle: A secure hub-and-spoke network topology using AWS Transit Gateway and Centralized Egress.


## Project Overview: This project demonstrates a scalable AWS network architecture designed to isolate private workloads while maintaining secure administrative access and internet connectivity for updates.

## Architecture Highlights
![Untitled Diagram](https://github.com/user-attachments/assets/ba657a81-d633-466c-a962-2ce6c20315d8)

Transit Gateway (TGW): Acts as the central hub connecting the Public and Private VPCs.

Centralized Egress: The Private VPC (20.0.0.0/16) has no Internet Gateway. All outbound traffic flows via TGW to the Public VPC's NAT Gateway.

Strict Routing:

Public VPC: Routes 20.0.0.0/16 traffic to the TGW.

Private VPC: Routes 0.0.0.0/0 (Internet) to the TGW.

Security Groups:

Private instances accept SSH (Port 22) only from the Public VPC CIDR (10.0.0.0/16), ensuring no direct public access.

## Deployment 

Create the Public VPC with Public and Private subnets.

Deploy IGW and NAT Gateway in the Public VPC.

Create the Private VPC with a Private subnet.

Provision the Transit Gateway and attach both VPCs.

Update Route Tables to direct traffic through the TGW as shown in the diagram.
