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
*Create a Resource Group, then deploy both Windows 10 and Ubunto Server VMs. During the Windows 10 VM setup, create a Virtual Network and place both VMs in the same network*. 

- In Azure, navigate to **Resource Groups** → **Create**: 
    - **Name**: RG-Network-Activities
    - **Region**: (US) East US 2
<table>
  <tr>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 1 59 39 PM" src="https://github.com/user-attachments/assets/6b6eef59-f826-47b8-975d-23861e6cf013">
</td>
    

  <tr>
    <td>RG-Network-Activities</td>
  </tr>
</table>

- In Azure, navigate to **Virtual Machines** → **Create**:
  - **Resource Group**: RG-Network-Activites 
  - **Name**: Windows-VM
  - **Region**: (US) East US 2
  - **Image**: Windows 10 Pro (at least 2 vCPUs)
  - **Authentication Type**: select → **Password** (*the username and password are needed to RDP into the VM*)
  - Navigate to the **Networking** tab → **Create new**:    
      - Name: Project-VNnet
      - Subnet: Leave as *default*
<table>
  <tr>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 2 05 41 PM" src="https://github.com/user-attachments/assets/29808f0a-1f21-4772-a95b-b51be6775aea">
</td>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 2 06 45 PM" src="https://github.com/user-attachments/assets/3c9c1d79-87d2-4a53-a2ac-2e89ba4ce7a5">
</td>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 2 09 04 PM" src="https://github.com/user-attachments/assets/a355eead-312a-4d72-97ac-1d30d5518c42">
</td>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 2 12 11 PM" src="https://github.com/user-attachments/assets/cfae6617-379a-4a0d-8d13-f81ab2d19d16">
</td>

  <tr>
    <td>Name & Region</td>
    <td>Image Selection</td>
    <td>Username & Password </td>
    <td>Create VNet</td>
  </tr>
</table>

- In Azure, navigate to **Virtual Machines** → **Create**:
  - **Resource Group**: RG-Network-Activites
  - **Name**: Ubunto-VM
  - **Region**: (US) East US 2
  - **Image**: Ubunto Server 22.04 (at least 2 vCPUs)
  - **Authentication Type**: select → **Password** (*the username and password are needed to RDP into the VM*)
  - Navigate to the **Networking** tab:
      -  **Virtual Network**: Project-VNnet
      -  Subnet: Leave as *default*
<table>
  <tr>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 2 17 09 PM" src="https://github.com/user-attachments/assets/c2a9bc93-b8f4-4a64-9395-7d5685dc156e">
</td>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 2 17 35 PM" src="https://github.com/user-attachments/assets/af8f5375-4fdf-49a1-b100-b6a854700979">
</td>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 2 19 36 PM" src="https://github.com/user-attachments/assets/7a3f161d-0b68-418c-a724-2ffcdf8899d7">
</td>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 2 20 22 PM" src="https://github.com/user-attachments/assets/e858498f-22c5-45ab-bae2-c510c668ced7">
</td>

  <tr>
    <td>Name & Region</td>
    <td>Image Selection</td>
    <td>Username & Password </td>
    <td>Create VNet</td>
  </tr>
</table>


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
<table>
  <tr>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 2 25 21 PM" src="https://github.com/user-attachments/assets/3aa5210d-7476-4102-b76e-43e2e9f6381a">
</td>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 2 31 16 PM" src="https://github.com/user-attachments/assets/09cbb4bc-85f4-464c-bcd6-841b542b4f20">
</td>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 2 35 09 PM" src="https://github.com/user-attachments/assets/9f703a77-20c8-4db3-8f2f-f4afda58bbce">
</td>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 2 35 28 PM" src="https://github.com/user-attachments/assets/8ec4a300-0169-4fdd-b871-4f822d922964">
</td>

  <tr>
    <td>Windows VM Public IP</td>
    <td>Install Wireshark</td>
    <td>Start Capturing</td>
    <td>Filter: ICMP</td>
  </tr>
</table>

🔷***Ping the Ubunto Server VM from the Windows 10 VM***  
*Test ICMP connectivity between the two VMs and observe traffic in Wireshark.*

- Retrieve the **private IP address** of the **Ubunto Server VM**:
    - Azure Portal → Virtual Machines → Ubunto-VM → Overview → Network Settings 
- Within the **Windows 10 VM**, open **PowerShell** and run:
   
     `ping <Ubunto-VM-private-IP>`
    
- Observe the ping requests and replies in **Wireshark**.
<table>
  <tr>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 2 36 28 PM" src="https://github.com/user-attachments/assets/9a7d5084-c6e8-419f-92e5-215c04f47931">
</td>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 2 39 20 PM" src="https://github.com/user-attachments/assets/97189293-0e44-4cdb-a922-cde97b77897d">
</td>
    

  <tr>
    <td>Ubunto VM Private IP</td>
    <td>Ping & Observe ICMP Traffic</td>
  </tr>
</table>

🔷***Ping a Public Website***  
*Observe external network traffic.*

-  Within the **Windows 10 VM**, ping a public website (e.g., **google.com**) and observe the traffic in **Wireshark**:

    `ping www.google.com`
 
<table>
  <tr>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 2 41 02 PM" src="https://github.com/user-attachments/assets/1a03da11-9265-4959-86c9-a4394cefacbf">
</td>
    
  <tr>
    <td>Ping Google</td>
  </tr>
</table>


### ***Configuring Firewall (Network Security Group)***

---

🔷***Block ICMP Traffic Using Network Security Group (NSG)***  
*Use Azure's NSG to block ICMP traffic and observe the results.*

- Within the **Windows 10 VM**, open **Powershell** and run a perpetual/non-stop ping:  
       `ping <Ubuntu-VM-private-IP> -t`
<table>
  <tr>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 9 22 57 PM" src="https://github.com/user-attachments/assets/c97cb750-b9d3-4ba7-a46d-92e89cfb0323">
</td>
   
  <tr>
    <td>Perpetual Ping</td>
  </tr>
</table>

- In Azure, navigate to **Network Security Group (NSG)** for the **Ubuntu VM**:
    - Azure Portal → Virtual machines → Ubunto-VM → Networking → Network Settings → Ubunto-VM-nsg
    - Disable incoming **ICMP traffic** by updating the NSG rules:
      - Settings → Inbound security Rules → Add → From ANY source, *deny ICMPv4* w/ Priority: 290
<table>
  <tr>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 9 24 58 PM" src="https://github.com/user-attachments/assets/affdd5e3-1635-4eea-afcc-a009d3d0d079">
</td>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 9 44 29 PM" src="https://github.com/user-attachments/assets/43bfc3ce-51b0-4db5-902b-0a510b7e5697">
</td>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 9 44 59 PM" src="https://github.com/user-attachments/assets/85b2eb05-3c91-4d94-b119-a9ff3246103f">
</td>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 9 45 58 PM" src="https://github.com/user-attachments/assets/e37651f0-0d74-47bd-bd1c-aa969dbcdb62">
</td>

  <tr>
    <td>Ubunto-VM-nsg</td>
    <td>Add Security Rule</td>
    <td>Configure Security Rule</td>
    <td>Rule Overview</td>
  </tr>
</table>

- Back in the **Windows 10 VM**, observe the failed ping attempts in **Wireshark**.
- Re-enable ICMP traffic in the **NSG** and verify successful ping responses:
    - Delete the *deny ICMPv4* rule previoulsy created in Ubunto-VM-nsg
- *Stop* the **Wireshark** capture via the *Red Square* icon 
<table>
  <tr>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 9 47 50 PM" src="https://github.com/user-attachments/assets/23f889a5-acd7-4879-90cb-ecd4917cd377">
</td>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 9 51 02 PM" src="https://github.com/user-attachments/assets/c6b0c11a-8e52-408a-8d88-64b16f062dbe">
</td>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 9 52 07 PM" src="https://github.com/user-attachments/assets/9eb09de5-0777-4c55-bc30-6f3a2c92f7a0"></td>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 9 53 18 PM" src="https://github.com/user-attachments/assets/52539b30-8d48-4330-89b8-e077bbe230be">
</td>

  <tr>
    <td>ICMP Traffic Blocked</td>
    <td>Delete the Security Rule</td>
    <td>ICMP Re-Enabled</td>
    <td>Stop the Capture</td>
  </tr>
</table>


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
<table>
  <tr>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 10 02 42 PM" src="https://github.com/user-attachments/assets/0e1149a0-9781-4dc7-ac7f-e73b2eddb5f5">
</td>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 10 04 04 PM" src="https://github.com/user-attachments/assets/12f7b2b6-6b13-41a8-b41a-cd0f07007541">
</td>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 10 06 12 PM" src="https://github.com/user-attachments/assets/130b36b8-ea07-4df7-9ec8-2966edb8e689">
</td>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 10 07 21 PM" src="https://github.com/user-attachments/assets/2fbdf663-bb77-4035-a3a8-6e8dbb5cf0bf">
</td>

  <tr>
    <td>SSH into Ubunto-VM</td>
    <td>Verify SSH</td>
    <td>Filter: tcp.port ==22</td>
    <td>Exit</td>
  </tr>
</table>

🔷***Observe DHCP Traffic***  
*Test DHCP traffic by renewing the IP address of the Windows 10 VM.*

- In **Wireshark**, filter for **DHCP traffic**.
- Within the **Windows 10 VM**, renew the IP address via **Powershell**:
    
     `ipconfig /renew`
  
- Observe the DHCP traffic in **Wireshark**.
  - Observe the same trafffic filtering for `udp.port==67||udp.port==68`
<table>
  <tr>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 10 14 54 PM" src="https://github.com/user-attachments/assets/6a2b8c2f-5b40-4c40-8c4c-277612adc721">
</td>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 10 15 30 PM" src="https://github.com/user-attachments/assets/f6d08ad1-6b6e-4c1d-be5b-74bd9ab9467f">
</td>
   
  <tr>
    <td>Observe DHCP Traffic</td>
    <td>Filter:udp.port==67||udp.port==68 </td>
  </tr>
</table>

🔷***Observe DNS Traffic***  
*Use nslookup to generate DNS traffic.*

- In **Wireshark**, filter for **DNS traffic**.
- From the **Windows the VM**, run **nslookup** in **Powershell** to check DNS resolution for `google.com` and `disney.com`:
    
    `nslookup google.com`
    `nslookup disney.com`

- Observe the DNS traffic in **Wireshark**.
  - Observe the same trafffic filtering for `udp.port==53||tcp.port==53`
<table>
  <tr>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 10 21 36 PM" src="https://github.com/user-attachments/assets/98f1d8c6-1c70-4811-9435-7f2046b7ddc0"></td>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 10 21 55 PM" src="https://github.com/user-attachments/assets/32556695-3a51-4ad4-a27b-b6062d215be4"></td>
    
  <tr>
    <td>Observe DNS Traffic</td>
    <td>Filter:udp.port==53||tcp.port==53</td>
  </tr>
</table>


🔷***Observe RDP Traffic***  
*Monitor Remote Desktop traffic and analyze its continuous nature.*

- In **Wireshark**, filter for **RDP traffic**:  
    `tcp.port == 3389`
- Observe the continuous stream of RDP traffic. This is because RDP continuously transmits data to maintain the live connection.

<table>
  <tr>
    <td><img width="200" height="150" alt="Screenshot 2024-10-24 at 10 24 51 PM" src="https://github.com/user-attachments/assets/fb74af6a-86a4-4c79-979e-211257a5b06a">
</td>
    
  <tr>
    <td>Observe RDP Traffic</td>
    
  </tr>
</table>
