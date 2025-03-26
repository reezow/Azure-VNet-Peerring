# Azure-VNet-Peerring
## Introduction
In this project, I created two virtual networks (VNets) in Azure, connected them using VNet peering, deployed a virtual machine (VM) into each VNet, and established communication between the VMs using the `ping` command.

## Prerequisites
- Preconfigured Azure lab with a subscription and resource group
- Azure CLI installed

## Step-by-Step Process
### Step 1: Create Virtual Networks
1. vnet-1:
    - Address space: 10.0.0.0/16
    - Subnet: subnet-1 with address space 10.0.1.0/24
    - Enabled Azure Bastion with hostname bastion and public IP public-ip-bastion

2. vnet-2:
    - Address space: 10.1.0.0/16
    - Subnet: subnet1 with address space 10.1.0.0/24
    - Skipped Bastion deployment (After the network peer, you can connect to both virtual machines with the same Bastion deployment.)

### Step 2: Configure VNet Peering
- Created a two-way network peer between vnet-1 and vnet-2.

### Step 3: Deploy Virtual Machines in each vnet:
1. vm-1:
   - Deployed in vnet-1
   - Ubuntu server with password authentication
   - No public inbound ports
   - No public IP
   - Network Security Group (NSG): nsg-1
  
2. vm-2:
   - Deployed in vnet-2
   - Ubuntu server with password authentication
   - No public inbound ports
   - No public IP
   - Network Security Group (NSG): nsg-2

### Step 4: Establish Communication
1. Connect to vm-1 through Bastion
