<h1>Active Directory Home Lab</h1>

<h2>Description</h2>
This project simulates setting up an Active Directory (AD) environment with a Domain Controller (DC), internal networking, DHCP, and NAT configurations. The lab demonstrates creating an Active Directory forest, configuring a domain, setting up user roles, and connecting client machines to the domain for internal and external communication.
<br />

---

<h2>Table of Contents</h2>
1. [Lab Overview]<br />
2. [Prerequisites]<br />
3. [Setup Steps]<br />
   - [1. Configure the Domain Controller (DC) Server]<br />
   - [2. Rename the System]<br />
   - [3. Create the Domain and Configure AD DS]<br />
   - [4. Create a Dedicated Domain Admin Account]<br />
   - [5. Install and Configure RAS/NAT]<br />
   - [6. Setup DHCP Server]<br />
   - [7. Configure Internet Access (For Lab Use Only)]<br />
   - [8. Automate User Creation with PowerShell]<br />
   - [9. Configure a Virtual Client Machine]<br />
   - [10. Connect the Client to the Domain]<br />
4. [Testing]<br />
5. [Conclusion]<br />
6. [Resources]<br />

---

<h2 id="lab-overview">Lab Overview</h2>
This lab models a basic enterprise IT environment with:<br />
- A Domain Controller for centralized management<br />
- Network Address Translation (NAT) for internet access<br />
- A DHCP server for IP address allocation<br />
- Active Directory for managing users and computers<br />
- A virtual Windows 10 client connected to the domain<br />

![Architecture Diagram](https://i.imgur.com/rEoYjIh.png)

---

<h2 id="prerequisites">Prerequisites</h2>
- A virtualized environment using VMware, VirtualBox, or Hyper-V<br />
- Access to Windows Server ISO and Windows 10 Pro ISO<br />
- Diagram for the network configuration (IP addressing, subnets, and routing)<br />
- Basic knowledge of Active Directory, DNS, DHCP, and NAT<br />

---

<h2 id="setup-steps">Setup Steps</h2>

<h3 id="1-configure-the-domain-controller-dc-server">1. Configure the Domain Controller (DC) Server</h3>
1. **Set up IP addressing**:<br />
   - Open network settings and go to **Change Adapter Options**.<br />
   - Identify the two network connections (one for internet, one for the internal network).<br />
   - Rename the internet connection (e.g., `Internet`).<br />
   - Assign a static IP to the internal connection:<br />
     - IP Address: `172.16.0.1`<br />
     - Subnet Mask: `255.255.255.0`<br />
     - DNS Server: `127.0.0.1` (loopback)<br />
     
![Network Config](https://i.imgur.com/bpYDQsQ.png)

2. **Enable internal and external communication**:
   - Ensure the internal network is configured correctly and NAT is set up later for routing.

---

<h3 id="2-rename-the-system">2. Rename the System</h3>
1. Rename the machine to `DC` for better organization.<br />
2. Right-click Start, go to **System**, and rename the system.<br />

---

<h3 id="3-create-the-domain-and-configure-ad-ds">3. Create the Domain and Configure AD DS</h3>
1. **Install Active Directory Domain Services**:<br />
   - Open **Server Manager** > **Add Roles and Features**.<br />
   - Select **Active Directory Domain Services (AD DS)** and install.<br />

![AD DS Download](https://i.imgur.com/0IClu5o.png)

2. **Promote the server to a Domain Controller**:
   - Click the flag in the top-right corner and select **Promote this server to a domain controller**.
   - Choose **Add a new forest** and set the Root Domain Name (e.g., `mydomain.com`).
   - Leave other settings as default except for the password.

3. **Verify the setup**:
   - Restart the server to complete the promotion.
   - Confirm AD DS and DNS roles appear in the Server Manager Dashboard.

![AD DS and DNS](https://i.imgur.com/3HqJyVt.png)

---

<h3 id="4-create-a-dedicated-domain-admin-account">4. Create a Dedicated Domain Admin Account</h3>
1. Open **Active Directory Users and Computers**.<br />

![Active Directory](https://i.imgur.com/E9XiYEB.png)

2. Create an Organizational Unit (OU) named `_ADMINS`.<br />

![Admin Folder](https://i.imgur.com/6ZrzuSu.png)

3. Add a new user in the `_ADMINS` OU:<br />
   - Assign the user to the **Domain Admins** group for elevated permissions.<br />

![Add to Domain](https://i.imgur.com/yCOvgyO.png)

![Show is Admin](https://i.imgur.com/Cna2zGM.png)

---

<h3 id="5-install-and-configure-rasnat">5. Install and Configure RAS/NAT</h3>
1. **Install Remote Access Services**:<br />
   - Go to **Add Roles and Features** > Select **Remote Access**.<br />
   - Include the **DirectAccess and VPN** and **Routing** services.<br />

![Roles](https://i.imgur.com/Hj0rhMI.png)

2. **Configure NAT**:
   - Open **Routing and Remote Access**.<br />
![Routing Wizard](https://i.imgur.com/ltiYkfV.png)<br />
   - Right-click the server > **Configure and Enable** > Select **Network Address Translation (NAT)**.<br />
![Select Internet](https://i.imgur.com/fPYVrv5.png)<br />
   - Assign the public interface (internet connection).<br />
  
![Show NAT Wizard](https://i.imgur.com/XpJkhZG.png)<br />

---

<h3 id="6-setup-dhcp-server">6. Setup DHCP Server</h3>
1. Install the DHCP role via **Add Roles and Features**.<br />

2. **Create a DHCP Scope**:<br />
![DHCP Range](https://i.imgur.com/BkGGCeC.png)<br />
   - Define a range of IPs (e.g., `172.16.0.100 - 172.16.0.200`).<br />
   - Use `172.16.0.1` as the default gateway.<br />
   - Activate the scope and ensure IPv4 is operational.<br />
![DHCP Config](https://i.imgur.com/lqk6eYF.png)<br />


---

<h3 id="7-configure-internet-access-for-lab-use-only">7. Configure Internet Access (For Lab Use Only)</h3>
1. Disable **IE Enhanced Security Configuration** via **Server Manager**.<br />
2. Enable NAT to allow internet access for lab purposes.

![DHCP Working](https://i.imgur.com/aqKRxEl.png)<br />

---

<h3 id="8-automate-user-creation-with-powershell">8. Automate User Creation with PowerShell</h3>
1. Download a PowerShell script for automated user creation:<br />
   - URL: [AD_PS GitHub Repository](https://github.com/joshmadakor1/AD_PS)<br />

![Script](https://i.imgur.com/oZiihrn.png)<br />

2. Run the script in PowerShell to create sample users in bulk.

![Script Complete](https://i.imgur.com/dAhgmHU.png)<br />

---

<h3 id="9-configure-a-virtual-client-machine">9. Configure a Virtual Client Machine</h3>
1. Set up a Windows 10 Pro VM.<br />
2. Connect it to the internal network.

![Connected](https://i.imgur.com/b28NAhU.png)<br />

![Connected](https://i.imgur.com/KUIZtG2.png)<br />

---

<h3 id="10-connect-the-client-to-the-domain">10. Connect the Client to the Domain</h3>
1. Assign an IP from the DHCP server.<br />
2. Join the domain via **System Properties**.<br />
3. Verify the client appears under Active Directory Computers.

![Verify](https://i.imgur.com/cgLWzhd.png)<br />

---

<h2 id="testing">Testing</h2>
1. Verify the client can:<br />
   - Access the internet via NAT.<br />
   - Communicate with the Domain Controller.<br />
   - Ping external sites (e.g., `google.com`).<br />

![Test](https://i.imgur.com/JevytG4.png)<br />

2. Ensure the domain admin account has full permissions.<br />
![Access](https://i.imgur.com/qpoDdj6.png)<br />
When the user logs into their account, they have access to the users specific permissions from the Domain.<br />
---

<h2 id="conclusion">Conclusion</h2>
This lab demonstrates setting up a basic Active Directory environment with DHCP, NAT, and a domain-connected client. It showcases skills in configuring Windows Server, managing AD DS, and automating tasks using PowerShell.

---

<h2 id="resources">Resources</h2>
- [Windows Server Documentation](https://docs.microsoft.com/en-us/windows-server/)
- [AD_PS GitHub Repository](https://github.com/joshmadakor1/AD_PS)
