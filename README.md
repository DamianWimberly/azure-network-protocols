-<p align="center">
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
*Create a Resource Group, then deploy both Windows 10 and Ubunto Server VMs. During the Windows 10 VM setup, create a Virtual Network and place both VMs in the same network*. 

- In Azure, navigate to **Resource Groups** → **Create**: 
    - **Name**: RG-Network-Actvities
    - **Region**: (US) East US 2
- In Azure, navigate to **Virtual Machines** → **Create**:
  - **Resource Group**: RG-Network-Activites 
  - **Name**: Windows-VM
  - **Region**: (US) East US 2
  - **Image**: Windows 10 Pro (at least 2 vCPUs)
  - **Authentication Type**: select → **Password** (*the username and password are needed to RDP into the VM*)
  - Navigate to the **Networking** tab → **Create new**:    
      - Name: Project-VNnet
      - Subnet: Leave as *default*     
- In Azure, navigate to **Virtual Machines** → **Create**:
  - **Resource Group**: RG-Network-Activites
  - **Name**: Ubunto-VM
  - **Region**: (US) East US 2
  - **Image**: Ubunto Server 22.04 (at least 2 vCPUs)
  - **Authentication Type**: select → **Password** (*the username and password are needed to RDP into the VM*)
  - Navigate to the **Networking** tab:
      -  **Virtual Network**: Project-VNnet
      -  Subnet: Leave as *default*



### ***Observing ICMP Traffic***

---

🔷***Remote Desktop and Wireshark***    
*Connect to the Windows 10 VM via RDP and set up Wireshark to view traffic between the VMs.*  

- Retrieve the **public IP address** of the **Windows 10 VM**:
    - Azure Portal → Virtual Machines → Windows-VM → Overview → Network Settings   
- Use **Remote Desktop** to connect and log in to the **Windows 10 VM**.
- Within the **Windows 10 VM**, download and install **Wireshark**:
    - [link](https://your-link-here) → Windows x64 Installer → Install
      - Open **Wireshark** and start capturing packets via the *Shark Fin* icon 
      - Filter for **ICMP traffic** in Wireshark.

🔷***Ping the Ubunto Server VM from the Windows 10 VM***  
*Test ICMP connectivity between the two VMs and observe traffic in Wireshark.*

- Retrieve the **private IP address** of the **Ubunto Server VM**:
    - Azure Portal → Virtual Machines → Ubunto-VM → Overview → Network Settings 
- Within the **Windows 10 VM**, open **PowerShell** and run:
   
     `ping <Ubunto-VM-private-IP>`
    
- Observe the ping requests and replies in **Wireshark**.

🔷***Ping a Public Website***  
*Observe external network traffic.*

-  Within the **Windows 10 VM**, ping a public website (e.g., **google.com**) and observe the traffic in **Wireshark**:

    `ping www.google.com`
 



### ***Configuring Firewall (Network Security Group)***

---

🔷***Block ICMP Traffic Using Network Security Group (NSG)***  
*Use Azure's NSG to block ICMP traffic and observe the results.*

- Within the **Windows 10 VM**, open **Powershell** and run a perpetual/non-stop ping:  
       `ping <Ubuntu-VM-private-IP> -t`  
- In Azure, navigate to **Network Security Group (NSG)** for the **Ubuntu VM**:
    - Azure Portal → Virtual machines → Ubunto-VM → Networking → Network Settings → Ubunto-VM-nsg
    - Disable incoming **ICMP traffic** by updating the NSG rules:
      - Settings → Inbound security Rules → Add → From ANY source, *deny ICMPv4* w/ Priority: 290
- Back in the **Windows 10 VM**, observe the failed ping attempts in **Wireshark**.
- Re-enable ICMP traffic in the **NSG** and verify successful ping responses:
    - Delete the *deny ICMPv4* rule previoulsy created in Ubunto-VM-nsg
- *Stop* the **Wireshark** capture via the *Red Square* icon 



### ***Observing SSH, DHCP, DNS, and RDP Traffic***

---

🔷***Observe SSH Traffic***  
*Capture and analyze SSH traffic between the two VMs.*

- Open Wireshark in the **Windows 10 VM**:
    - In **Wireshark**, start a new capture and filter for **SSH traffic**.
- From the **Windows 10 VM**, SSH into the **Ubuntu Server VM** via **Powershell**:  
    `ssh <username>@<Ubuntu-VM-private-IP>`
   - Use the **Ubunto VM** password
   - Observe the *prompt change*
   - Run `hostname` to confirm connection to the VM  
- Interact with the SSH session, then observe the traffic in **Wireshark**.
  - Use `tcp.port ==22` to observe the SSH traffic 
  - Run `exit` to end SSH connection

🔷***Observe DHCP Traffic***  
*Test DHCP traffic by renewing the IP address of the Windows 10 VM.*

- In **Wireshark**, filter for **DHCP traffic**.
- Within the **Windows 10 VM**, renew the IP address via **Powershell**:
    
     `ipconfig /renew`
  
- Observe the DHCP traffic in **Wireshark**.
  - Observe the same trafffic filtering for `udp.port==67||udp.port==68`

🔷***Observe DNS Traffic***  
*Use nslookup to generate DNS traffic.*

- In **Wireshark**, filter for **DNS traffic**.
- From the **Windows the VM**, run **nslookup** in **Powershell** to check DNS resolution for `google.com` and `disney.com`:
    
    `nslookup google.com`
    `nslookup disney.com`

- Observe the DNS traffic in **Wireshark**.
  - Observe the same trafffic filtering for `udp.port==53||tcp.port==53`
   

🔷***Observe RDP Traffic***  
*Monitor Remote Desktop traffic and analyze its continuous nature.*

- In **Wireshark**, filter for **RDP traffic**:  
    `tcp.port == 3389`
- Observe the continuous stream of RDP traffic. This is because RDP continuously transmits data to maintain the live connection.


