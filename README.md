<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Network Traffic Examination Between Azure Virtual Machines</h1>

## Project Overview
 
In this project I will be using Wireshark to analyze network traffic between Azure Virtual Machines. Wireshark is an open-source network protocol analyzer that inspects network traffic in real time. I will use this to inspect and observe data flowing across a network at the packet level using various protocols such as ICMP, SSH, DHCP, DNS, RDP. This will provide a good understanding of how Virtual Machines communicate in a network. This project also entails working with Azure Network Security Groups (NSG) to manage and control network traffic.

## 🔗 Related Project: Azure VM Setup
 
The Virtual Machines used in this project were created and configured in a previous project. Two Virtual Machines (Windows-VM and Linux-VM) were created and setup within the same Resource Group and Virtual Network.
You can view that project here:  
👉 https://github.com/glewis347/azure-setup

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute services)
- Windows App (for remote connection)
- Powershell
- Different Network Protocols (SSH, RDP, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (22H2)
- Ubuntu Server 24.04

<h2>High Level Steps</h2>

- Send a ping to the Linux VM in the Vnet and to a public IP address. Use Wireshark to observe the ICMP traffic.
- Send perpetual pings to the Linux VM and observe ICMP traffic. Configure the Linux VM's Network Security Group to disable incoming ICMP traffic and observe what happens in Wireshark.
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

---

**1. Observing ICMP Traffic**
  
In the Azure platform, select "Virtual machines" in the menu on the left, then select "Windows-VM". Copy the Public IP address as shown in the image below. Ensure that the Virtual Machines are turned on within Azure before continuing.

<img width="1713" height="858" alt="1" src="https://github.com/user-attachments/assets/81211ce2-9d05-41d3-9c51-f2f9da3bb91b" />

Open the Windows app and select the "+" sign, then "Add PC":

<img width="1268" height="437" alt="2" src="https://github.com/user-attachments/assets/e0108d10-9ea1-4f6b-92e1-db686d1d5dae" />

In the "PC Name" field, enter the IP Address of the Windows VM. In the "Friendly name" field, enter "Windows-VM" then select "Add":

<img width="1164" height="711" alt="image" src="https://github.com/user-attachments/assets/f7061d45-1437-41e2-a275-99357a493e1e" />

Under "Saved Devices" double click the Window-VM tile and enter the login credentials for this VM. Select "Continue"

<img width="1165" height="542" alt="image" src="https://github.com/user-attachments/assets/ef53e5c2-e642-449b-aeb2-ac67edf08a3e" />

Select "Continue" again:

<img width="1172" height="491" alt="image" src="https://github.com/user-attachments/assets/07f57d45-198f-49c3-8511-082a9e4d7fca" />

You should now be able to see the desktop of the Windows VM as shown below:

<img width="1279" height="718" alt="6" src="https://github.com/user-attachments/assets/99f13baf-3994-48bf-a94d-aaae44c7d37b" />

In a web browser in Windows-VM, navigate to https://www.wireshark.org/download.html and download/install Wireshark in Windows-VM:

<img width="1278" height="715" alt="7" src="https://github.com/user-attachments/assets/a30f77b3-55f0-413f-b0ad-1ff3607bcc40" />

Once installed, open Wireshark and start packet capture. Select "Ethernet" then click the Shark Fin in the menu bar as shown below:

<img width="428" height="371" alt="8" src="https://github.com/user-attachments/assets/55feac85-6ff0-4103-ba02-b24d82ff3494" />

Observe rows of active traffic being displayed. This means there is communication occurring on the back end:

<img width="973" height="637" alt="9" src="https://github.com/user-attachments/assets/0def5ad5-96cf-402d-8c14-a46dd73b2f99" />

Filter for ICMP traffic and observe the change. Apply a display filter by typing "ICMP" and notice the change. The display window is blank as there are currently no packets using ICMP. 

<img width="975" height="639" alt="10" src="https://github.com/user-attachments/assets/0349f65d-a73f-4f3f-ab44-e5eadee27738" />

Ping uses ICMP. I will obtain the Linux-VM private IP address and ping it from within the Windows-VM and observe the traffic in Wireshark. To ping the Linux-VM we will use Powershell. The command line for this is "ping 10.0.0.5". Notice that the ping was successful as there were 4 packets sent and received, with 0% loss:

<img width="855" height="292" alt="11" src="https://github.com/user-attachments/assets/4c49491f-0ae9-4508-921a-87dc327f6712" />

Examine the traffic in Wireshark. Observe that for every packet that was sent, Wiresharks logs a request and a reply, creating 8 lines of observable ICMP traffic:

<img width="1141" height="455" alt="12" src="https://github.com/user-attachments/assets/469fa172-f93a-4d47-8f0f-5f0ba623e9ea" />

Ping a public website (www.google.com) and observe the results in Wireshark. Observe that each of the 8 lines of ICMP traffic has the IP address for www.google.com

<img width="446" height="186" alt="13" src="https://github.com/user-attachments/assets/ba9f9d99-0a65-42e7-8a49-d35590675df8" />
<img width="1141" height="463" alt="14" src="https://github.com/user-attachments/assets/b7039ed7-b500-4b4a-bd40-94b929dec137" />

Ping the Linux-VM using a perpetual (non-stop) ping from the Windows-VM. In Powershell, enter the command "ping 10.0.0.5 -t". Observe the continuous lines of ICMP in Wireshark:

<img width="452" height="532" alt="Screenshot 2026-06-08 at 12 00 58 PM" src="https://github.com/user-attachments/assets/7e96ad77-d04c-4c2e-958d-1b0abf6de080" />
<img width="1145" height="488" alt="Screenshot 2026-06-08 at 12 03 19 PM" src="https://github.com/user-attachments/assets/e34eab38-f6b8-4879-ac8c-9df3497e1107" />


**2. Configuring a Network Security Group**
Now configure the Linux-VM's Network Security Group to disable incoming (inbound) ICMP traffic.
Within Azure's Virtual Machine page, select "Linux-VM" and in the menu on the left select "Networking" then "Network settings". Navigate to the Rules section and select "+ Create port rule" -> "Inbound port rule":

<img width="1674" height="730" alt="1" src="https://github.com/user-attachments/assets/124ef52d-a727-42fd-90c3-48b706bf9129" />

Configure the rule to deny ICMP traffic with the below settings:
- Source: Any
- Destination: Any
- Service: Custom
- Destination Port:*
- Protocol: ICMPv4
- Action: Deny
- Priority: 290
- Name: DenyInboundICMP
Select "Add"

<img width="577" height="814" alt="2" src="https://github.com/user-attachments/assets/e2e991bb-d245-40a7-a0b6-ee2afc64ab95" />

All incoming ICMP traffic to the Linux-VM is now blocked. Observe what happens in Powershell and Wireshark:
In Powershell, there is now a message of "Request timed out." In Wireshark, each row of new traffic now says "request" and "no response found!". Contrast this with previous rows of traffic where we observed alternating rows of "request" and "reply", which is absent here as we disabled inbound ICMP traffic to the Linux-VM.

<img width="857" height="400" alt="Screenshot 2026-06-08 at 12 06 10 PM" src="https://github.com/user-attachments/assets/b9d19f31-a788-475b-a3dc-b0038a882070" />
<img width="1144" height="473" alt="Screenshot 2026-06-08 at 12 06 26 PM" src="https://github.com/user-attachments/assets/b382a839-9311-44f0-987c-97418760c4c8" />

Re-enable inbound ICMP traffic on the Linux-VM and observe what happens in Powershell and Wireshark:
In Powershell, the perpetual pings have now resumed with a reply from the Linux-VM. In Wireshark, we are now observing alternating rows of "request" and "reply" as a result of enabling inbound ICMP traffic.

<img width="853" height="385" alt="3" src="https://github.com/user-attachments/assets/6e59a649-de33-4d79-9e6a-12edb79011a9" />
<img width="1141" height="378" alt="4" src="https://github.com/user-attachments/assets/24d749a2-7eb7-4642-b826-0661c14af9a8" />


**3. Observing SSH Traffic**
In Wireshark, apply a filter to display only SSH packets. Type "SSH" in the filter bar, or you can also type "tcp.port==22" (SSH uses TCP on port 22)
<img width="1142" height="259" alt="image" src="https://github.com/user-attachments/assets/34be1e8d-a7d8-4899-ac68-7a2e961ec2f5" />

From the Windows-VM, open Powershell and initiate a SSH connection to the Linux-VM using the command: **ssh labuser@10.0.0.5** 
Enter the password for Linux-VM to connect, and observe the SSH packet capture in Wireshark:
<img width="551" height="638" alt="image" src="https://github.com/user-attachments/assets/dbc3d7b5-9fb9-4f8e-bb9d-f889127f0ee5" />
<img width="1143" height="500" alt="image" src="https://github.com/user-attachments/assets/b094445c-0918-4cb8-b043-2fd3ab3b5283" />

Enter basic commands in Powershell (example: hostname) and observe the packets in Wireshark, which confirms that communication between both VMs is active. Type "exit" and press (Enter) in Powershell to close the SSH connection.
<img width="517" height="287" alt="Screenshot 2026-06-08 at 1 11 39 PM" src="https://github.com/user-attachments/assets/abd69c20-2963-4853-8e7b-44952f8a4ee3" />


**4. Observing DHCP Traffic**
In Wireshark apply a filter to display only DHCP related traffic. Type "DHCP" in the filter bar, or you can also type "udp.port==67 || udp.port==68" (DHCP uses UDP on ports 67 & 68)
<img width="1141" height="256" alt="image" src="https://github.com/user-attachments/assets/7aa7b4d9-ce0a-466a-af33-ef90d6eb0b8c" />

Using Powershell in Windows-VM, enter the command "ipconfig /renew" to request a new IP address from the DHCP server. Observe the traffic in Wireshark and notice the DHCP traffic between the VM and DHCP server during the IP assignment process.
<img width="857" height="298" alt="Screenshot 2026-06-08 at 1 29 07 PM" src="https://github.com/user-attachments/assets/9e39365c-65eb-4fc6-ae63-f7ea31d7e144" />
<img width="1141" height="466" alt="Screenshot 2026-06-08 at 1 29 46 PM" src="https://github.com/user-attachments/assets/2724cbf6-0bf0-4913-955e-941050ffd169" />


**5. Observing DNS Traffic**
In Wireshark apply a filter to display only DNS related traffic. Type "DNS" in the filter bar, or you can also type "udp.port==53 || tcp.port==53" (DNS uses UDP & TCP on port 53)
Using Powershell in Windows-VM, enter the command "nslookup www.google.com" to query the DNS server for IP address of google.com. Observe the DNS traffic in Wireshark:
<img width="470" height="453" alt="Screenshot 2026-06-08 at 3 30 26 PM" src="https://github.com/user-attachments/assets/c8eb95f4-4591-4601-be8e-1417fc7ff7cc" />
<img width="1277" height="484" alt="Screenshot 2026-06-08 at 3 30 51 PM" src="https://github.com/user-attachments/assets/a2ae7145-782e-414b-986a-1d37646db7ff" />


**6. Observing RDP Traffic** 
In Wireshark apply a filter to display only RDP related traffic. Type "tcp.port==3389" (RDP uses TCP on port 3389)
Immediately notice that there is continuous traffic being displayed in Wireshark. This happens because RDP constantly sends data during a live remote session. Any changes on the remote system, such as a screen update or mouse/cursor movement, is shown in real time. 
<img width="1280" height="483" alt="image" src="https://github.com/user-attachments/assets/f2f6a3d1-7ed5-4752-ae6c-51e05502a461" />
