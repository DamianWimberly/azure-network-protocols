<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
This tutorial guides you through the process of creating and configuring Virtual Machines (VMs) in Azure, observing network traffic, and implementing firewall rules to control traffic between VMs. The project includes using Wireshark to capture and filter traffic such as ICMP, SSH, DHCP, DNS, and RDP, as well as configuring Azure Network Security Groups (NSGs) to manage traffic.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Wireshark
- Remote Desktop
- Azure Network Security Groups (NSG)
- Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
  
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
*Set up the Resource Group and create both Windows 10 and Ubuntu VMs within the same Virtual Network/Subnet.*

- In Azure, navigate to **Resource Groups** → **Create**.  
    - **Name**: RG-Network-Actvities  
- Create a **Windows 10 Virtual Machine** in the previously created Resource Group.  
    - Allow it to create a new **Virtual Network** and **Subnet**.
- Create an **Ubuntu (Linux) VM** in the same Resource Group and **Virtual Network**.
    - Ensure the **Virtual Network** for this VM matches the one created with the Windows VM.
    - Authentication type: **Username/Password**.



### ***Part 2: Observing ICMP Traffic***

---

🔷***Set Up Remote Desktop and Wireshark***    
*Connect to the Windows 10 VM and set up Wireshark for packet capture.*  

- Use **Remote Desktop** to connect to your **Windows 10 VM**.
- Within the Windows 10 VM, install **Wireshark**.
    - Open Wireshark and start capturing packets.
    - Filter for **ICMP traffic** in Wireshark.

🔷***Ping the Ubuntu VM from the Windows 10 VM***  
*Test ICMP connectivity between the two VMs and observe traffic in Wireshark.*

- Retrieve the **private IP address** of the **Ubuntu VM**.
    - Azure > 
- From the **Windows 10 VM**, open **Command Prompt** or **PowerShell** and run:
    ```bash
    ping <Ubuntu-VM-private-IP>
    ```
3. Observe the ping requests and replies in **Wireshark**.

🔷***Ping a Public Website***  
*Observe external network traffic.*

1. From the **Windows 10 VM**, ping a public website (e.g., **google.com**) and observe the traffic in Wireshark:
    ```bash
    ping www.google.com
    ```



### ***Part 3: Configuring Firewall (Network Security Group)***

---

🔷***Block ICMP Traffic Using Network Security Group (NSG)***  
*Use Azure's NSG to block ICMP traffic and observe the results.*

1. In Azure, navigate to the **Network Security Group (NSG)** for your **Ubuntu VM**.
2. Disable incoming **ICMP traffic** by updating the NSG rules.
3. Back in the **Windows 10 VM**, observe the failed ping attempts in **Wireshark**.
4. Re-enable ICMP traffic in the **NSG** and verify successful ping responses.



### ***Part 4: Observing SSH, DHCP, DNS, and RDP Traffic***

---

🔷***Observe SSH Traffic***  
*Capture and analyze SSH traffic between the two VMs.*

1. Back in **Wireshark**, start a new capture and filter for **SSH traffic**.
2. From the **Windows 10 VM**, SSH into the **Ubuntu VM** using:
    ```bash
    ssh <username>@<Ubuntu-VM-private-IP>
    ```
3. Interact with the SSH session, then observe the traffic in **Wireshark**.

🔷***Observe DHCP Traffic***  
*Test DHCP traffic by renewing the IP address of the Windows 10 VM.*

1. In **Wireshark**, filter for **DHCP traffic**.
2. From the **Windows 10 VM**, renew the IP address:
    ```bash
    ipconfig /renew
    ```
3. Observe the DHCP traffic appearing in **Wireshark**.

🔷***Observe DNS Traffic***  
*Use nslookup to generate DNS traffic.*

1. In **Wireshark**, filter for **DNS traffic**.
2. From the **Windows 10 VM**, use **nslookup** to check DNS resolution for `google.com` and `disney.com`:
    ```bash
    nslookup google.com
    nslookup disney.com
    ```
3. Observe the DNS traffic in **Wireshark**.

🔷***Observe RDP Traffic***  
*Monitor Remote Desktop traffic and analyze its continuous nature.*

1. In **Wireshark**, filter for **RDP traffic** (tcp.port == 3389).
2. Observe the continuous stream of RDP traffic. This is because RDP continuously transmits data to maintain the live connection.


