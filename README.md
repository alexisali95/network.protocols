![Capture](https://github.com/user-attachments/assets/aa17d401-2f23-4636-868f-ddc693c6ff98)
![Capture2](https://github.com/user-attachments/assets/9ffb0115-4044-4770-a38a-1d5ba4bfecdd)

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

![ping 10 0 0 5 (1)](https://github.com/user-attachments/assets/c935132d-3dde-4a89-9547-70324b88fe01)

We can see the packets on Wireshark and inspect each packet and the data source that is being transferred from each ping. 

### Next section:

> RUN COMMAND: ping -t

![-t ping (2)](https://github.com/user-attachments/assets/650fd7c1-e2e6-4a42-80e3-b665315cf229)

This command will allow VM1 to continuously ping to VM2. VM1 will continue to ping VM2 until we manually create an inbound rule to stop it. While VM1 is pinging, we will go back to the Azure portal to add an inbound rule for VM2 to block ICMP traffic using its firewall. After adding a new inbound rule, VM1 will stop receiving echos from VM2. 

![deny rule (3)](https://github.com/user-attachments/assets/b5c553f6-9062-4000-b577-a33f59825980)

> Add > Destination port ranges: 8080 > Protocol: ICMPv4 > Action: Deny > Priority: 250 > Save

![5th timed out](https://github.com/user-attachments/assets/ac2c7d85-fc17-4a53-a077-c6a00edef3f7)

Observation: Notice how in the command line the request is timed out once we created a new inbound rule. 

Delete inbound rule

### Next section: 

We will use the SSH (Secure Shell) protocol. This protocol will allow VM1 to securely connect to VM2. The purpose of this protocol is to capture and analyze the network traffic tied to this connection. 

> RUN COMMAND: ssh azureuser2@10.0.0.5

![ssh key (4)](https://github.com/user-attachments/assets/37915fa9-c034-4848-be71-d25ea86706dc)


Observation: We can see that Wireshark begins to capture SSH packets.

### Next section:

We will use DHCP (Dynamic Host Configuration Protocol). This protocol is used to automatically assign IP addresses to devices on a network. This operates on ports 67 (server) and 68 (client).

> RUN COMMAND: ipconfig /renew

![DHCP ipconfig (5)](https://github.com/user-attachments/assets/b85e4f22-7828-42e3-b848-b46a7db502ca)

Observation: This shows how devices communicate with a DHCP server to get their network configurations.

### Next section

We will capture the DNS (Domain Name System) traffic. This protocol will help convert domain names (like google.com) into IP addresses.DNS works on port 53.

> RUN COMMAND: nslookup www.google.com

![dns look up (6)](https://github.com/user-attachments/assets/e746b331-b52c-4d8c-bc15-f49f0317de43)

Observation: You can see the actual exchange of DNS messages and understand how the system queries and receives responses for domain names.

### Last section:

We will use RDP (Remote Desktop Protocol) to filter and capture traffic on Wireshark inserting “tcp.port==339”, which is used by default for communication between the client and server.

![RDP traffic (7)](https://github.com/user-attachments/assets/398cd34e-9b94-436c-afe7-81e68c57aff1)

Observation: You can see wireshark capture the ongoing data exchange. This helps see real time remote desktop traffic.


Make sure to delete your resources to avoid cost because we are only paying for what we use.

Hope you enjoyed my tutorial :)
