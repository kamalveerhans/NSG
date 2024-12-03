<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04


<h2>Actions and Observations</h2>

<p>
Log in to the Azure Portal and create a Resource Group. Deploy a Windows 10 VM, allowing it to create a new Virtual Network (VNet) and Subnet. Next, deploy an Linux (Ubuntu) VM in the same Resource Group, VNet, and Subnet, using a username/password for authentication. Use Remote Desktop to connect to the Windows VM. Once you're in, open a web browser and download Wireshark from the following link: http://www.wireshark.org/download.html. Once you get Wireshark up and running, you're going to filter for ICMP (Internet Control Message Protocol). Retrieve the Linux (Ubuntu) VM's private IP, then ping it from the Windows VM while filtering for ICMP traffic in Wireshark
</p>
<p>
<img src="https://imgur.com/saGn1Gq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Next we will configure the Network Security Group (NSG) of the Linux (Ubuntu) VM to manage ICMP traffic. Start a continuous ping from the Windows 10 VM to the Linux (Ubuntu) VM, to do this we will use "-t" option with the "ping" command and the private IP address of the Linux Virtual Machine. Then, go to the Azure Portal, open the Network Security Group (NSG) associated with the Ubuntu VM, and disable inbound ICMP traffic by adding a deny rule. As a result the ping from the Windows VM failed, and no ICMP replies were captured in Wireshark. Next, re-enable ICMP traffic in the NSG by modifying the rule to allow ICMP traffic. Once the changes are applied, the ping responses resumed, and could see the ICMP traffic again in Wireshark. Finally stop the ping activity by pressing Ctrl + C in the Command Prompt.
</p>
<p>
<img src="https://github.com/user-attachments/assets/43fff794-d96a-49fb-ac4b-0991c14224dd" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/QUmOEgG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />


<p>
We then set up a filter for SSH (Secure Shell) traffic on the Windows 10 VM to monitor activity during the connection to the Linux VM. Since SSH operates solely through the command line, we used the command ssh username@10.0.0.5 in the terminal. Once the connection was established, Wireshark started capturing and displaying SSH packets, showcasing the encrypted data exchange between the systems.
</p>
<p>
<img src="https://imgur.com/eTgy1D9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<p>
While monitoring SSH traffic, we executed several Linux commands on the Ubuntu VM to generate activity and observe the resulting packets captured in Wireshark. The commands used included:


  * id (to display user information)
  * hostname (to show the system's hostname)
  * pwd (to print the current working directory)
  * touch <filename> (to create a new file)
  * ls (to list files in the directory).
</p>
<p>
<img src="https://imgur.com/NqcGn2Z.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<p>
In Wireshark, apply a filter for DHCP traffic. On the Windows 10 VM, open PowerShell as an administrator and run the command ipconfig /renew to request a new IP address for the VM. This action generates DHCP traffic, including the DHCP Discover, Offer, Request, and Acknowledgment packets, which can be observed in Wireshark. These packets demonstrate the process of obtaining a new IP address from the DHCP server.
</p>
<p>
<img src="https://imgur.com/s2mXema.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<p>
In Wireshark, apply a filter for DNS traffic. On the Windows 10 VM, open the command line and use the nslookup command to query the IP addresses of google.com. Observe the DNS query and response packets in Wireshark, which display the domain names being resolved to their corresponding IP addresses.
</p>
<p>
<img src="https://imgur.com/TKIVeSG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<p>
Finally, we'll filter RDP (Remote Desktop Protocol) traffic by entering tcp.port==3389. Notice the constant stream of traffic, even without user activity. This happens because RDP continuously transmits data to maintain the live desktop stream, ensuring real-time updates on the client side.
</p>
<p>
<img src="https://imgur.com/SFyCMqM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
