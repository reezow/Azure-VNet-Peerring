# Azure-VNet-Peerring
## Introduction
Virtual Network Peering allows you to connect two or more Virtual Networks in Azure.

Imagine that you have a virtual machine in VNet01. This VM needs to see another VM that is deployed to another Azure VNet. You can simply create VNet peering between these two VNets, allowing these VMs to communicate with each other.

In this project (using the Azure Portal), I created two virtual networks (VNets) in Azure, connected them using VNet peering, deployed a virtual machine (VM) into each VNet, and established communication between the VMs using the `ping` command.

## Prerequisites
- Preconfigured Azure lab with a subscription and resource group
- Azure CLI installed

## Step-by-Step Process
### Step 1: Create Virtual Networks
1. vnet-1:
    - Address space: 10.0.0.0/16
    - Subnet: subnet-1 with address space 10.0.1.0/24
    - Enabled Azure Bastion with hostname 'bastion' and public IP 'public-ip-bastion'

2. vnet-2:
    - Address space: 10.1.0.0/16
    - Subnet: subnet1 with address space 10.1.0.0/24
    - Skipped Bastion deployment (After the network peer, you can connect to both virtual machines with the same Bastion deployment.)

### Step 2: Configure VNet Peering
- Created a two-way network peer between vnet-1 and vnet-2.
    - added a peering under vnet-1 with the following parameters:
        - Remote VNet Summary:
            - Peering link name --> vnet2-to-vnet1
            -  Virtual Network --> vnet-2
            -  Allowed 'vnet-2 to receive forwarded traffic to vnet-1'
        - Local VNet Summary:
            - Peering link name --> vnet1-to-vnet2
            - Allowed 'vnet-1 to receive forwarded traffic to vnet-2'
  

### Step 3: Deploy Virtual Machines in each vnet:
1. vm-1:
   - Deployed in vnet-1
   - Ubuntu server with password authentication
   - No public inbound ports
   - No public IP
   - Network Security Group (NSG): (new) nsg-1
  
2. vm-2:
   - Deployed in vnet-2
   - Ubuntu server with password authentication
   - No public inbound ports
   - No public IP
   - Network Security Group (NSG): (new) nsg-2

### Step 4: Establish Communication
1. Connected to vm-1 through Bastion and pinged the target IP of vm-2:
   > ping -c 4 10.1.0.4
    - ping: This is a network utility used to test the reachability of a host on an IP network. It sends Internet Control Message Protocol (ICMP) Echo Request packets to the target host and waits for an Echo Reply.
    - -c 4: This option specifies the number of ICMP Echo Request packets to send. In this case, it sends 4 packets.
    - 10.1.0.4: This is the IP address of the target VM you are trying to reach.
      
- Verified connectivity with a successful reply:
  
        azureuser@vm-1:~$ ping -c 4 10.1.0.4
        PING 10.1.0.4 (10.1.0.4) 56(84) bytes of data.
        64 bytes from 10.1.0.4: icmp_seq=1 ttl=64 time=2.29 ms
        64 bytes from 10.1.0.4: icmp_seq=2 ttl=64 time=1.06 ms
        64 bytes from 10.1.0.4: icmp_seq=3 ttl=64 time=1.30 ms
        64 bytes from 10.1.0.4: icmp_seq=4 ttl=64 time=0.998 ms
        --- 10.1.0.4 ping statistics ---
        4 packets transmitted, 4 received, 0% packet loss, time 3004ms
        rtt min/avg/max/mdev = 0.998/1.411/2.292/0.520 ms
        azureuser@vm-1:~$


2. Connected to vm-2 through Bastion and pinged the target IP of vm-1:
   > ping -c 4 10.0.0.4
     - ping: This is a network utility used to test the reachability of a host on an IP network. It sends Internet Control Message Protocol (ICMP) Echo Request packets to the target host and waits for an Echo Reply.
     - -c 4: This option specifies the number of ICMP Echo Request packets to send. In this case, it sends 4 packets.
     - 10.0.0.4: This is the IP address of the target VM you are trying to reach.

- Verified connectivity with a successful reply:

        azureuser@vm-2:~$ ping -c 4 10.0.0.4
        PING 10.0.0.4 (10.0.0.4) 56(84) bytes of data.
        64 bytes from 10.0.0.4: icmp_seq=1 ttl=64 time=1.87 ms
        64 bytes from 10.0.0.4: icmp_seq=2 ttl=64 time=0.831 ms
        64 bytes from 10.0.0.4: icmp_seq=3 ttl=64 time=11.2 ms
        64 bytes from 10.0.0.4: icmp_seq=4 ttl=64 time=1.08 ms
        --- 10.0.0.4 ping statistics ---
        4 packets transmitted, 4 received, 0% packet loss, time 3036ms
        rtt min/avg/max/mdev = 0.831/3.744/11.199/4.320 ms
        azureuser@vm-2~$:


## Conclusion
By following these steps, I successfully created two VNets, peered them, deployed VMs, and verified connectivity between the VMs. This project helped me understand VNet peering and inter-VM communication in Azure.
