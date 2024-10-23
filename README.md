<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
This tutorial guides you through the process of creating and configuring Virtual Machines (VMs) in Microsoft Azure, observing network traffic, and implementing firewall rules to control traffic between VMs. The project includes using Wireshark to capture and filter traffic such as ICMP, SSH, DHCP, DNS, and RDP, as well as configuring Azure Network Security Groups (NSGs) to manage traffic.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Wireshark
- Remote Desktop
- Azure Network Security Groups (NSG)
- Network Protocols (SSH, RDP, DNS, HTTP/S, ICMP)
  
<h2>Operating Systems Used </h2>

- Windows 10 
- Ubuntu 20.04 

<h2>High-Level Deployment and Configuration Steps</h2>

- Create Windows 10 and Ubuntu Virtual Machines
- Set up VMs within the same Virtual Network (VNet) and Subnet
- Capture and analyze ICMP, SSH, DHCP, DNS, and RDP traffic using Wireshark
- Configure Network Security Group to control ICMP traffic


<h2>Tutorial:</h2>

### ***Creating Virtual Machines***

---

🔷***Create Resource Group and Virtual Machines (VMs)***  
*Create a Resource Group, then deploy both Windows 10 and Ubuntu VMs. During the Windows VM setup, create a Virtual Network, and place both VMs in the same network*. 

- In Azure, navigate to **Resource Groups** → **Create**.  
    - **Name**: RG-Network-Actvities
    - **Region**: (US) East US 2
- In Azure, navigate to **Virtual Machines** → **Create**.
  - **Resource Group**: RG-Network_Activites 
  - **Name**: Windows-VM
  - **Region**: (US) East US 2
  - **Image**: Windows 10 Pro (at least 2 vCPUs)
  - **Authentication Type**: select → **Password** (*RDP into the VM with the username and password*)
  - Navigate to the **Networking** tab → **Create new**:    
      - Name: Project-VNnet
      - Subnet: Leave as *default*     
- In Azure, navigate to **Virtual Machines** → **Create**.
  - **Resource Group**: RG-Network_Activites
  - **Name**: Linux-VM
  - **Region**: (US) East US 2
  - **Image**: Ubunto Server 22.04 (at least 2 vCPUs)
  - **Authentication Type**: select → **Password** (*RDP into the VM with the username and password*)
  - Navigate to the **Networking** tab:
      -  **Virtual Network**: Project-VNnet
      -  Subnet: Leave as *default*



### ***Observing ICMP Traffic***

---

🔷***Set Up Remote Desktop and Wireshark***    
*Connect to the Windows 10 VM and set up Wireshark for packet capture.*  

- Retrieve the Windows 10 VM Public IP Address
    - Azure > Nwtwork Settings >   
- Use **Remote Desktop** to connect to your **Windows 10 VM**.
- Within the Windows 10 VM, install **Wireshark**.
    - Open Wireshark and start capturing packets.
    - Filter for **ICMP traffic** in Wireshark.

🔷***Ping the Ubuntu VM from the Windows 10 VM***  
*Test ICMP connectivity between the two VMs and observe traffic in Wireshark.*

- Retrieve the **private IP address** of the **Ubuntu VM**.
    - Azure > 
- From the **Windows 10 VM**, open **Command Prompt** or **PowerShell** and run:
   
     `ping <Ubuntu-VM-private-IP>`
    
3. Observe the ping requests and replies in **Wireshark**.

🔷***Ping a Public Website***  
*Observe external network traffic.*

1. From the **Windows 10 VM**, ping a public website (e.g., **google.com**) and observe the traffic in Wireshark:

    `ping www.google.com`




### ***Configuring Firewall (Network Security Group)***

---

🔷***Block ICMP Traffic Using Network Security Group (NSG)***  
*Use Azure's NSG to block ICMP traffic and observe the results.*

1. In Azure, navigate to the **Network Security Group (NSG)** for your **Ubuntu VM**.
2. Disable incoming **ICMP traffic** by updating the NSG rules.
3. Back in the **Windows 10 VM**, observe the failed ping attempts in **Wireshark**.
4. Re-enable ICMP traffic in the **NSG** and verify successful ping responses.



### ***Observing SSH, DHCP, DNS, and RDP Traffic***

---

🔷***Observe SSH Traffic***  
*Capture and analyze SSH traffic between the two VMs.*

1. Back in **Wireshark**, start a new capture and filter for **SSH traffic**.
2. From the **Windows 10 VM**, SSH into the **Ubuntu VM** using:
    
     `ssh <username>@<Ubuntu-VM-private-IP>`
    
3. Interact with the SSH session, then observe the traffic in **Wireshark**.

🔷***Observe DHCP Traffic***  
*Test DHCP traffic by renewing the IP address of the Windows 10 VM.*

1. In **Wireshark**, filter for **DHCP traffic**.
2. From the **Windows 10 VM**, renew the IP address:
    
     `ipconfig /renew`
   
3. Observe the DHCP traffic appearing in **Wireshark**.

🔷***Observe DNS Traffic***  
*Use nslookup to generate DNS traffic.*

1. In **Wireshark**, filter for **DNS traffic**.
2. From the **Windows 10 VM**, use **nslookup** to check DNS resolution for `google.com` and `disney.com`:
    
    `nslookup google.com`
    `nslookup disney.com`
 
3. Observe the DNS traffic in **Wireshark**.

🔷***Observe RDP Traffic***  
*Monitor Remote Desktop traffic and analyze its continuous nature.*

1. In **Wireshark**, filter for **RDP traffic** (tcp.port == 3389).
2. Observe the continuous stream of RDP traffic. This is because RDP continuously transmits data to maintain the live connection.


