![Capture](https://github.com/user-attachments/assets/b198136f-96cf-43e0-8693-33ca9587af4c)
![Capture2](https://github.com/user-attachments/assets/68fa5dc1-0d2f-408b-b7bf-eac0c7fc2fb2)


# Implementing Network Security Groups (NSGs) and Network Protocols

This tutorial will help us observe how to inspect network traffic between two azure virtual machines using Wireshark and implement Network Security Groups. 

## Environments and Applications Used

* Azure Portal
* Remote Desktop
* Command-Line Tools
* Network Protocols (ICMP, SSH, DHCP, DNS, RDP)
* Wireshark (Network Analyzer)

## Operating Systems Used

* Windows 10
* Ubuntu Server 20.24

## Step By Step Tutorial   

Before we begin we need to create a resource group, virtual network and 2 virtual machines in the Azure portal. Both NSGs for each virtual machine will automatically be made but double check to make sure the resources are there.

- Images used for VMs: Windows 10 Pro (VM1) and Ubuntu Server (Linux) [VM2]

> Remotely access VM1 > Go to the internet browser and search “download wireshark”
> Download wireshark > go to “downloads” in file explorer > install and run the application

The first network protocol we will use is ICMP (Internet Control Message Protocol). ICMP is a network layer protocol used to diagnose communication issues in networks. Its purpose is to determine whether data reaches its destination and helps in error reporting.In order to ping VM1 to VM2, we will need the private IP address of VM2.

> OPEN POWERSHELL >
> RUN COMMAND: ping 10.0.0.5

![ping 10 0 0 5 (1)](https://github.com/user-attachments/assets/ccb0522a-c02c-42fc-af31-db58f6a98036)


We can see the packets on Wireshark and inspect each packet and the data source that is being transferred from each ping. 

### Next section:

> RUN COMMAND: ping -t

![-t ping (2)](https://github.com/user-attachments/assets/76cd01c4-d6ea-490b-8e9d-39b8e6bd96be)

This command will allow VM1 to continuously ping to VM2. VM1 will continue to ping VM2 until we manually create an inbound rule to stop it. While VM1 is pinging, we will go back to the Azure portal to add an inbound rule for VM2 to block ICMP traffic using its firewall. After adding a new inbound rule, VM1 will stop receiving echos from VM2. 

![deny rule (3)](https://github.com/user-attachments/assets/8ce29a35-3c34-43d0-811a-76d3bf981aa4)


> Add > Destination port ranges: 8080 > Protocol: ICMPv4 > Action: Deny > Priority: 250 > Save

![5th timed out](https://github.com/user-attachments/assets/34b32323-590b-4aef-8637-47ee51798a1c)


Observation: Notice how in the command line the request is timed out once we created a new inbound rule. 

Delete inbound rule

### Next section: 

We will use the SSH (Secure Shell) protocol. This protocol will allow VM1 to securely connect to VM2. The purpose of this protocol is to capture and analyze the network traffic tied to this connection. 

> RUN COMMAND: ssh azureuser2@10.0.0.5

![ssh key (4)](https://github.com/user-attachments/assets/5925a30f-f4a8-4d88-8790-9e871b2f1dbf)

Observation: We can see that Wireshark begins to capture SSH packets.

### Next section:

We will use DHCP (Dynamic Host Configuration Protocol). This protocol is used to automatically assign IP addresses to devices on a network. This operates on ports 67 (server) and 68 (client).

> RUN COMMAND: ipconfig /renew

![DHCP ipconfig (5)](https://github.com/user-attachments/assets/4891ea77-045c-48d0-a67b-a541ac69aaee)


Observation: This shows how devices communicate with a DHCP server to get their network configurations.

### Next section

We will capture the DNS (Domain Name System) traffic. This protocol will help convert domain names (like google.com) into IP addresses. DNS works on port 53.

> RUN COMMAND: nslookup www.google.com

![dns look up (6)](https://github.com/user-attachments/assets/2b03b2c4-5675-4b8b-b074-524461425ca5)


Observation: You can see the actual exchange of DNS messages and understand how the system queries and receives responses for domain names.

### Last section:

We will use RDP (Remote Desktop Protocol) to filter and capture traffic on Wireshark inserting “tcp.port==339”, which is used by default for communication between the client and server.

![RDP traffic (7)](https://github.com/user-attachments/assets/dfde4336-5cb4-4a5b-80d1-b3a7016d60fd)


Observation: You can see wireshark capture the ongoing data exchange. This helps see real time remote desktop traffic.


Make sure to delete your resources to avoid cost because we are only paying for what we use.

Hope you enjoyed my tutorial :)
