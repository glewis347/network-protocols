<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Network Traffic Analysis Between Azure Virtual Machines</h1>

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

- Observe ICMP Traffic
- Configure a Network Security Group
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

---

**1. Observe ICMP Traffic**
  
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







<br>
<br>

Once connected to the Windows 10 VM, I installed and launched Wireshark, started a packet capture, and applied the `ICMP` filter to display only `ICMP` traffic.  

<img width="1812" height="888" alt="image" src="https://github.com/user-attachments/assets/80508f41-8992-4105-828d-9798a3fb27ed" />
<img width="1886" height="915" alt="image" src="https://github.com/user-attachments/assets/eb959b0f-783c-411d-894a-cd462450668f" />

<br>
<br>

Applied the `ICMP` filter in Wireshark to only display ICMP traffic.

<img width="1549" height="910" alt="image" src="https://github.com/user-attachments/assets/323c53f5-4739-415c-8b1d-3341100f01c9" />

<br>
<br>

**Retrieved the private IP address of the Ubuntu VM (linux-vm) and attempted to ping it from within the Windows 10 VM**
1. Open the Ubuntu VM in Azure
2. Navigate to the **Overview/Properties tab**
3. Check under **Networking**
4. Locate Private IP address (**172.16.0.5**)

<br>
<br>

**Result**: You successfully found the Ubuntu VM's private IP address
  
<img width="1386" height="420" alt="Screenshot 2026-02-24 191138" src="https://github.com/user-attachments/assets/1ea9505d-eedc-4b10-9809-ca031e3f8875" />

<br>
<br>

Opened **Windows PowerShell** and `ping` the Ubuntu VM's private IP address (**172.16.0.5**) from within the Windows 10 VM.
While the ping was running on the Windows 10 VM, I observed the `ICMP` request and reply traffic in Wireshark.
> [!NOTE]
> Successful ICMP replies confirmed that communication between the Windows and Ubuntu virtual machines was working properly.
  
<img width="1902" height="936" alt="image" src="https://github.com/user-attachments/assets/8e2fffb7-5d4c-4480-bd8d-72703a0e6181" />


---

**2. Configuring a Firewall (Network Security Group)**
 
I initiated a continuous ping from the Windows 10 VM to the Ubuntu VM (Private IP address) using the `ping -t` command in PowerShell. This caused `ping` to run continuously, allowing me to analyze the traffic in real time in Wireshark.

<img width="624" height="555" alt="image" src="https://github.com/user-attachments/assets/fc9719cf-d706-4871-8813-325d3d813736" />

<br>
<br>

While the continuous ping was running, I configured the Network Security Group (NSG) associated with the Ubuntu VM to block inbound ICMP traffic.:

1. Navigate to the Ubuntu VM in Azure
2. Open **Network Settings**
3. Select the associated NSG **(Linux-VM-nsg)**
4. Click **+ Add inbound security rule**
5. Configure the rule to deny ICMP traffic:
- Source: Any
- Destination: Any
- Service: Custom
- Destination Port:*
- Protocol: ICMPv4
- Action: Deny
- Priority: 290
- Name: DenyInbound.
6. Click **Add**

<img width="1794" height="1087" alt="image" src="https://github.com/user-attachments/assets/6f38a1a0-cb20-4205-83f8-6a2371de62d4" />

<br>
<br>

> [!NOTE]
> All inbound `ICMP` (ping) traffic to your Ubuntu VM is now blocked, so ping requests from your Windows 10 VM will fail

<img width="877" height="548" alt="Screenshot 2026-02-24 193855" src="https://github.com/user-attachments/assets/76b693af-162f-46f1-aa03-07ab91a953f6" />

<br>
<br>

**Re-enabling `ICMP` traffic (Allow Ping again)**

1. Within Azure, go to Linux VM's Network Security Group
2. Locate the DenyInbound (ICMP) rule
3. Click the **delete** (trash icon) on the rule
4. Confirm by selecting **Yes** in the prompt

<img width="1490" height="576" alt="image" src="https://github.com/user-attachments/assets/0cc8f104-b0eb-459e-9a4b-233500259c2f" />

<br>
<br>

**Result**: After re-enabling `ICMP`, I returned to the Windows 10 VM and observed that the ping replies resumed, and Wireshark showed reply packets being received again.

> [!NOTE]
> This shows how NSGs function as a firewall by controlling inbound and outbound traffic, directly affecting connectivity between networked systems.

<img width="777" height="535" alt="image" src="https://github.com/user-attachments/assets/a5af13de-76ab-416c-b06b-df151b5c6501" />


---

**3. Observing SSH Traffic**

I opened Wireshark on the Windows 10 virtual machine and started a packet capture to observe network traffic. Applied the `SSH` filter to display only SSH-related packets.
  
<img width="746" height="484" alt="image" src="https://github.com/user-attachments/assets/41e9dc17-1c21-4f78-861f-1005bffa1545" />

<br>
<br>

From the Windows 10 VM, I initiated an SSH connection to the Ubuntu VM using **PowerShell** with the following command: (**`ssh labuser@172.16.0.5`**). After entering the correct credentials, I successfully connected to the Ubuntu virtual machine.
  
<img width="1221" height="566" alt="Screenshot 2026-02-24 202054" src="https://github.com/user-attachments/assets/09d8b753-4c66-44f3-bea7-886af2b5f5cd" />

<br>
<br>

Once connected, I executed basic commands such as `whoami` and `pwd` within the SSH session. During this time, I observed continuous SSH traffic in Wireshark, confirming that communication between the two systems was active and encrypted. After finishing observing the traffic, I typed `exit` and pressed Enter to close the `SSH` connection.

<img width="1406" height="826" alt="Screenshot 2026-02-24 201620" src="https://github.com/user-attachments/assets/198ba6a0-08b9-4067-b40b-2fecfcac71fe" />


---

**4. Observing DHCP Traffic**

On the Windows 10 VM, I returned to Wireshark and applied a `DHCP` filter to display only DHCP-related traffic

<img width="960" height="682" alt="Screenshot 2026-02-24 204137" src="https://github.com/user-attachments/assets/5dad524f-8a8f-469f-b5ed-71913330f7e6" />

<br>
<br>

After setting the filter, I opened **Windows PowerShell** within the Windows 10 VM and entered the command `ipconfig /renew` to request a new IP address from the DHCP server. While the command was running, `DHCP` traffic appeared in Wireshark (The capture showed the exchange between the client and the DHCP server during the IP assignment process).

> [!NOTE]
> DHCP dynamically assigns IP addresses and network configuration settings, enabling devices to join a network without manual configuration.

<img width="1242" height="577" alt="Screenshot 2026-02-24 204753" src="https://github.com/user-attachments/assets/41814628-72d3-45a9-a9b2-7b0b100df463" />

---

**5. Observing DNS Traffic**

In the Windows 10 VM, I applied a `DNS` filter to view only DNS-related traffic.

<img width="879" height="533" alt="Screenshot 2026-02-24 205139" src="https://github.com/user-attachments/assets/e3ce49c9-4e38-4c8e-8a22-62dd867e4f02" />

<br>
<br>

**After applying the filter:**
- Open **Windows PowerShell**
- Run the command: `nslookup google.com`
- The DNS server returns IP addresses for **google.com**

- Observe the DNS traffic being shown in Wireshark:

<img width="473" height="342" alt="Screenshot 2026-02-24 205818" src="https://github.com/user-attachments/assets/f6252881-980c-4e05-8a90-52f74e00f078" />
<img width="1538" height="865" alt="Screenshot 2026-02-24 205837" src="https://github.com/user-attachments/assets/005ce102-9125-4095-b074-77e40418dfdb" />

<br>
<br>

- Open **Windows PowerShell**
- Run the command: `nslookup disney.com`
- The DNS server returns IP addresses for **disney.com**

- Observe the DNS traffic being shown in Wireshark:

> [!NOTE]
> This demonstrates how DNS is used to translate domain names into IP addresses so computers can communicate with each other over the network.

<img width="470" height="353" alt="Screenshot 2026-02-24 205918" src="https://github.com/user-attachments/assets/80bbe9cc-2918-4ee6-b4fc-846b94ddaed2" />
<img width="1427" height="984" alt="Screenshot 2026-02-24 205946" src="https://github.com/user-attachments/assets/1673974f-cbb5-407d-b476-4fbb2a0d26b4" />

---

**6. Observing RDP Traffic** 

I returned to Wireshark and applied the filter `tcp.port == 3389` to view Remote Desktop Protocol `RDP` traffic. After applying the filter, I observed continuous network traffic between my computer and the Windows 10 VM (This traffic remained active even when I was not typing or clicking anything). 

> [!NOTE]
> This happens because `RDP` constantly sends data to maintain the remote session and display a live view of the remote computer screen. The continuous transmission ensures that any changes on the remote system, such as mouse movements or screen updates, are shown in real time. 

<img width="1151" height="725" alt="image" src="https://github.com/user-attachments/assets/aa932325-279d-4d0e-80fa-2eade25e89de" />


<h2>Purpose</h2>

The purpose of this project is to develop a deeper understanding of how network traffic flows within the Azure environment and how security configurations help control and protect that traffic. Within the Azure-based systems, this project allows us to analyze how virtual machines communicate with each other between virtual networks and how security tools, such as Network Security Groups, regulate access and data transmission. This also helps us build practical knowledge of how various protocols function and how network activity can be monitored and observed. Overall, the project provides foundational experience that is essential for effectively managing, securing, and maintaining cloud-based network infrastructures in Microsoft Azure.
