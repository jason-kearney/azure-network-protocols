<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDP, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create our Virtual Machines
- Observe ICMP Traffic
- Configuring a Firewall/Network Security Group
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

<h2>Actions and Observations</h2>

![image](https://github.com/user-attachments/assets/57df51f8-1a6c-4008-b973-064cfcd31859)

<p>
First we need to create a resource group. Once you have Azure opened up, just search for resource groups and create one with whatever name you'd like. From there, search for virtual machines and create a Windows 10 VM and a Linux Ubuntu VM. Ensure that both VMs are part of the same resource group and Vnet and Subnet. When creating the Windows 10 VM first, just allow it to create a new Vnet and then just have the Linux VM use the same one.
</p>
<br />

![image](https://github.com/user-attachments/assets/e069128e-6907-4b80-a8c4-160b161d6c6d)

<p>
We're now going to observe some ICMP traffic. We're going to use Remote Desktop (Windows App) to open up our Windows VM. Log in using the username and password you set and download Wireshark. Once downloaded, open it up and filter the traffic for ICMP traffic. Open up Powershell and ping the private IP address of your Linux machine, which you can find by looking at its network information through the Azure GUI. Once you do that, you should see the traffic come up on Wireshark and you should be able to inspect it.
</p>
<br />

![image](https://github.com/user-attachments/assets/ff648122-23c3-4a17-9e27-3dcd46c6a744)

<p>
We're now going to configure a firewall to block inbound ICMP traffic to the Linux VM. But first, we're going to have our Windows VM constantly ping our Linux VM. Ping the Linux VM's private IP address with a -t at the end. Start a Wireshark capture and filter for ICMP traffic which you should constantly see coming in now. To set up the firewall, go to the Azure GUI, go to VMs, click on the Linux VM, Network Settings, click on the Network Security Group, settings, Inbound security rules, and add a new one. Change destination port ranges to *, Protocol to ICMPv4, Action to Deny, and Priority to 290. If you check the Wireshark capture now, you should only see requests. This means that the firewall is working as intended. Go back to Azure and delete the rule we just created. You should now start to see both requests and replies in Wireshark. You can now stop the constant pinging by hitting control c in Powershell.
</p>

![image](https://github.com/user-attachments/assets/06268507-33e0-45c3-b8a7-9865d9da529d)

<p>
We're now going to observe some SSH traffic. Start a new Wireshark capture and filter for SSH traffic. SSH into the Linux VM by typing in ssh username@private IP address, swapping in the private IP address for your Linux VM and whatever username you set. Type commands like pwd into the SSH connection. Look at Wireshark and observe the SSH traffic. Stop the SSH connection by typing in exit into Powershell.
</p>

![image](https://github.com/user-attachments/assets/a891a047-f69b-4920-a2ae-5bd1bb42a351)

<p>
We're now going to observe DHCP traffic. Start a new Wireshark capture and filter for DHCP traffic. We're going to attempt to issue our Windows VM a new IP address by opening Powershell as an admin and typing in ipconfig /renew. Observe the traffic in Wireshark.
</p>

![image](https://github.com/user-attachments/assets/e4814bdd-5f4c-488a-ba33-82918d3efab6)

<p>
We're now going to observe DNS traffic. Start a new Wireshark capture and filter for DNS traffic. Type in nslookup google.com to see what Google's IP addresses are. Observe the traffic in Wireshark.
</p>

![image](https://github.com/user-attachments/assets/b86a6421-0d9c-44eb-9ff4-1c769adcf8dd)

<p>
We're now going to observe RDP traffic. Start a new Wireshark capture and filter for tcp.port == 3389 traffic. Observe the traffic in Wireshark. We're getting constant traffic because we're using RDP to connect to our Windows VM and the server is constantly streaming a picture from the server to our local machine.
</p>

<p>
From here we can close our remote desktop connection and delete the resource group we created.
</p>
<br />
