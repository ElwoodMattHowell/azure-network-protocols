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

<h2>High-Level Steps</h2>

- Create Resources in Azure and install Wireshark
- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

<h2>Actions and Observations</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>


Our first step will be to create our resources in Azure.  We will create a Windows 10 virtual machine and a virtual machine running Ubuntu.  In the portal, click on _Virtual Machines_, _Create_, and _Azure virtual machine_.  Create a virtual machine using a Windows 10 image.  When your Windows 10 virtual machine has been created, begin the process again and create a virtual machine using an Ubuntu Server image.  For this virtual machine, be aure to use the same resource group and virtual network as the first virtual machine you created.  Once both machines have been created, use Microsoft Remote Desktop to connect to your Windows machine.  On your Windows virtual machine, open a web browser and navigate to https://www.wireshark.org/download.html.  Download and install Wireshark using all default settings. 
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>


Now we will observe Internet Control Message Protocol(ICMP) traffic between our two virtual machines.  ICMP is a network layer protocol used by network devices to diagnose network communication issues.  It is the protocol used when you "ping" another device.  From our windows machine, we will first open Wireshark.  Double click on _Ethernet_.  You will immediately see traffic being displayed.  Where it says _Apply a display filter_ type in icmp (lowercase) and hit _Enter_.  The traffic you were seeing should stop as we are now filtering for only ICMP traffic.  Back in Azure, find the private IP address of your Ubunto virtual machine.  Once you have copied the private IP address, log back into you Windows machine.  Open PowerShell.  At the prompt type _ping -t < your Ubuntu private IP address >_(please enter your own Ubuntu private IP address and disregard the inequality signs).  You should begin to see Replies from the private IP address you entered.  If you look in Wireshark you will see continuous requests from the Windows IP address and replies from the Ubuntu IP address.  We will now attempt to blobk ICMP traffic to our Ubuntu machine.  With the continuos ping stillvrunning, open up your Azure portal again and type _Network Security Groups_ in the search bar.  There should be entries for both your Windows machine and your Ubuntu Machine.  Click on your Ubuntu machine. Click _Inbound security rules_ and then click _Add_.  In the _Protocol_ category, click the radio button next to _ICMP_ and in the _Actionb_ category click the radio button next to Deny.  Set the priority 
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
