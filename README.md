# Active Directory Lab
<h1>Creating Bulk Users Using Powershell</h1>

<h2> Project Description</h2>
In this first Active Directory project, I virtualized a Windows 2019 Domain Server and Windows 10 machine using VirtualBox. I then ran a powershell script to create 1000 users within Active Directory and configured the template accounts and the server domain settings, TCP/IP, DHCP and NAT settingsto hold certain properties.

I followed along using this <a href="https://youtu.be/MHsI8hJmggI?si=rB0beoWKQubHIXh8/">technical demo</a> by cybersecurity professional and Youtuber, Josh Madakor.


<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 
- <b>Oracle VirtualBox</b>

<h2>Environments Used </h2>

- <b>Windows 10</b> 64-bit
- <b>Windows 2019 Server</b>

<h2>Project Walk-through:</h2>

<p align="center">
1. Download and install Oracle Virtualbox. <br/>
<br />
<br />
2. Download Windows 10 ISO and Server 2019 ISO. <br/>
<br />
<br />
3. Create Server Domain Controller virtual machine.  <br/>
<br/>
  <img src="https://drive.google.com/file/d/1ekM2XdPrui1YJpBnQd4Te7idkXXYiZ2K/view?usp=drive_link" height="80%" width="80%" alt="Create Server VM"/>
  3a. Give it two network adapters/NIC's. One to connect to the internet (DHCP from home router) <br/>
  and the other for clients to connect to internally (private network).   <br/>
<img src=""/>
<br/>
  3b. Name Server.   <br/>
<br />
<br />
4. Install Active Directory and create the Domain. <br/>
<img src=""/>
<br />
<br />
5. Configure NAT and routing, so clients on private network can reach the internet through Domain Controller.  <br/>
<img src=""/>
<br />
<br />
6. Set up DHCP on Domain Controller, so Windows 10 machine can automatically get IP address assignment. <br/>
<img src=""/>
<br />
<br />
7. Run powershell script to automatically create 1000 users in Active Directory.  <br/>
<img src="https://i.imgur.com/K71yaM2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
8. Create Windows 10 machine and join it to the domain as Client1.  <br/>
<img src="https://i.imgur.com/K71yaM2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
9. Log in using user account.  <br/>
<img src="https://i.imgur.com/K71yaM2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />


<h2>Breakdown of the Script</h2>


<h2>Concluding Thoughts</h2>

Overall, this was a fun and challenging introductory project to learning Active Directory in a Windows Server environment. I was able to apply what I am currently learning in my Network+ studies regarding virtualization, IP addressing and Network Address Translation (NAT), as well as practicing basic powershell scripting and automation. I hope to utilize the skills I learned in this lab to aid me in how to create and manage user identities in both on-prem and cloud environments.
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
