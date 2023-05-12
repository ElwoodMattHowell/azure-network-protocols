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

<p float="left">
  <img src="https://github.com/ElwoodMattHowell/images/blob/main/project4-step1-image1.png" height="40%" width="40%" alt="configure roles"/>
  <img src="https://github.com/ElwoodMattHowell/images/blob/main/project4-step1-image2.png" height="40%" width="30%" alt="configure roles">
   <img src="https://github.com/ElwoodMattHowell/images/blob/main/priject4-step1-image3.png" height="40%" width="25%" alt="configure rolese">
</p>


Our first step will be to create our resources in Azure.  We will create a Windows 10 virtual machine and a virtual machine running Ubuntu.  In the portal, click on _Virtual Machines_, _Create_, and _Azure virtual machine_.  Create a virtual machine using a Windows 10 image.  When your Windows 10 virtual machine has been created, begin the process again and create a virtual machine using an Ubuntu Server image.  For this virtual machine, be aure to use the same resource group and virtual network as the first virtual machine you created.  Once both machines have been created, use Microsoft Remote Desktop to connect to your Windows machine.  On your Windows virtual machine, open a web browser and navigate to https://www.wireshark.org/download.html.  Download and install Wireshark using all default settings. 
<br />

<p float="left">
  <img src="https://github.com/ElwoodMattHowell/images/blob/main/project4-step1-image1.png" height="40%" width="40%" alt="configure roles"/>
  <img src="https://github.com/ElwoodMattHowell/images/blob/main/project4-step1-image2.png" height="40%" width="30%" alt="configure roles">
   <img src="https://github.com/ElwoodMattHowell/images/blob/main/priject4-step1-image3.png" height="40%" width="25%" alt="configure rolese">
</p>


Now we will observe Internet Control Message Protocol(ICMP) traffic between our two virtual machines.  ICMP is a network layer protocol used by network devices to diagnose network communication issues.  It is the protocol used when you "ping" another device.  From our windows machine, we will first open Wireshark.  Double click on _Ethernet_.  You will immediately see traffic being displayed.  Where it says _Apply a display filter_ type in icmp (lowercase) and hit _Enter_.  The traffic you were seeing should stop as we are now filtering for only ICMP traffic.  Back in Azure, find the private IP address of your Ubunto virtual machine.  Once you have copied the private IP address, log back into you Windows machine.  Open PowerShell.  At the prompt type _ping -t < your Ubuntu private IP address >_(please enter your own Ubuntu private IP address and disregard the inequality signs).  You should begin to see Replies from the private IP address you entered.  If you look in Wireshark you will see continuous requests from the Windows IP address and replies from the Ubuntu IP address.  We will now attempt to blobk ICMP traffic to our Ubuntu machine.  With the continuos ping stillvrunning, open up your Azure portal again and type _Network Security Groups_ in the search bar.  There should be entries for both your Windows machine and your Ubuntu Machine.  Click on your Ubuntu machine. Click _Inbound security rules_ and then click _Add_.  In the _Protocol_ category, click the radio button next to _ICMP_ and in the _Actionb_ category click the radio button next to Deny.  Set the priority to anything lower than the lowest priority currently in use.  Ususally a priority of 200 will suffice.  Name the rule anything you like, but something discriptive along the lines of Stop_ICMP_Traffic would be good.
<br />

<p float="left">
  <img src="https://github.com/ElwoodMattHowell/images/blob/main/project4-step1-image1.png" height="40%" width="40%" alt="configure roles"/>
  <img src="https://github.com/ElwoodMattHowell/images/blob/main/project4-step1-image2.png" height="40%" width="30%" alt="configure roles">
   <img src="https://github.com/ElwoodMattHowell/images/blob/main/priject4-step1-image3.png" height="40%" width="25%" alt="configure rolese">
</p>


Our next observation will be of SSH traffic.  SSH, or Secure Shell, is a network communication protocol that allows two computers to communicate.  Back in your Windows virtual machine, open up Wireshark again and filter for ssh traffic.  You can type ssh in the filter or tcp.port == 22.  There should be no current traffic.  Open PowerShell again.  If your continuous ping from the last step is still running, press _control_ -_c_ to end the process.  We will now SSH into our Ubuntu machine.  At the prompt in PowerShell, type _ssh < your-Ubuntu-machine-username >@< your-Ubuntu-machine-private-IP-address >  (enter your own Ubuntu username and IP address, and disregard the inequality signs).  At the next prompt type _yes_ and then enter the password you created for your Ubuntu machine.  As a warning, nothing will show when you type in your password, just type it in and hit enter.  If it doesn’t work, try again.  We should now see a good deal of traffic in Wireshark.  We are logged into our Ubuntu machine and can enter Linux commands.  To close the connection type _exit_. 
<br />

<p float="left">
  <img src="https://github.com/ElwoodMattHowell/images/blob/main/project4-step1-image1.png" height="40%" width="40%" alt="configure roles"/>
  <img src="https://github.com/ElwoodMattHowell/images/blob/main/project4-step1-image2.png" height="40%" width="30%" alt="configure roles">
   <img src="https://github.com/ElwoodMattHowell/images/blob/main/priject4-step1-image3.png" height="40%" width="25%" alt="configure rolese">
</p>


Next up we will observe DHCP traffic.  Dynamic Host Configuration Protocol, or DHCP, is a client/server protocol that automatically provides an Internet Protocol host with its IP address and other related configuration information such as the subnet mask and default gateway.  We will observe this traffic by issuing our virtual machine a new IP address.  Back in Wireshark, filter by DHCP, tcp.port == 67, or tcp.port == 68.  Open Powershell.  At the command prompt, type _ipconfig /renew_.  Our virtual machine will be issued a new IP address and you will se DHCP traffic in Wireshark.
<br />

<p float="left">
  <img src="https://github.com/ElwoodMattHowell/images/blob/main/project4-step1-image1.png" height="40%" width="40%" alt="configure roles"/>
  <img src="https://github.com/ElwoodMattHowell/images/blob/main/project4-step1-image2.png" height="40%" width="30%" alt="configure roles">
   <img src="https://github.com/ElwoodMattHowell/images/blob/main/priject4-step1-image3.png" height="40%" width="25%" alt="configure rolese">
</p>


Now we will take a look at DNS traffic.  DNS, or the Domain Name System, translates human readable domain names to machine readable IP addresses.  Back in Wireshark, filter for DNS, or tcp.port == 53, traffic only.  Now open Powershell and using _nslookup_ find google.com and disney.com's IP addresses respectively.  At the command prompt, simply type `_nslookup www.disney.com_` and it will return the IP addresses associated with disney.com.
<br />

<p float="left">
  <img src="https://github.com/ElwoodMattHowell/images/blob/main/project4-step1-image1.png" height="40%" width="40%" alt="configure roles"/>
  <img src="https://github.com/ElwoodMattHowell/images/blob/main/project4-step1-image2.png" height="40%" width="30%" alt="configure roles">
   <img src="https://github.com/ElwoodMattHowell/images/blob/main/priject4-step1-image3.png" height="40%" width="25%" alt="configure rolese">
</p>


The final bit of traffic we will look at will be RDP traffic.  Remote desktop protocol (RDP) is a secure network communications protocol developed by Microsoft. It enables network administrators to remotely diagnose problems that individual users encounter and gives users remote access to their physical work desktop computers.  The port number for RDP is 3389.  In Wireshark, filter by RDP or by port number.  You should immediately be spammed by traffic.  The reason?  We are using RDP to connect to our virtual machine.
<br />
