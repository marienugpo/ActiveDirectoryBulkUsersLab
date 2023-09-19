# Active Directory Lab
<h1>Creating Bulk Users Using Powershell</h1>

<h2> Project Description</h2>
In this first Active Directory project, I virtualized a Windows 2019 Domain Server and Windows 10 machine using VirtualBox. I then configured the server domain settings, TCP/IP, DHCP and NAT settings to hold certain propertie and ran a powershell script to create 1000 users within Active Directory. Lastly, I created a virtual client and joined it to the domain. I followed along using this <a href="https://youtu.be/MHsI8hJmggI?si=rB0beoWKQubHIXh8/">technical demo</a> by cybersecurity professional and Youtuber, Josh Madakor.


<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 
- <b>Oracle VirtualBox</b>

<h2>Environments Used</h2>

- <b>Windows 10</b> 64-bit
- <b>Windows 2019 Server</b>

<h2>Overview</h2>

1. Create virtual machine for server (domain controller) using Virtualbox. Assign it two network adapters (internal and outbound). Assign IPv4 address to internal NIC.
2. Install Active Directory Domain Services and create domain. Create ADMIN organizational unit and admin account.
3. Install Remote Access Services (RAS) and configure NAT on Internet network adapter.
4. Set up DHCP and DHCP scope on domain controller.
5. Run powershell script to create 1000 users in AD.
6. Create Windows VM and join it to the domain as Client01.
7. Verify that client can connect to domain using ping or checking DHCP and AD settings on server side.

<h2>Project Walk-through:</h2>

<p align="center">
1. Download and install Oracle Virtualbox. <br/>
<br />
<br />
2. Download Windows 10 ISO and Server 2019 ISO. <br/>
<br />
<br />
3. Create Server Domain Controller virtual machine in VirtualBox and create Internet and Internal network adapters.  <br/>
<br/>
<img src="https://i.imgur.com/unMUvL9_d.jpg?maxwidth=520&shape=thumb&fidelity=high"/> <br/>
<img src="https://i.imgur.com/IIwtMQa_d.jpg?maxwidth=520&shape=thumb&fidelity=high"/> <br/>
<img src="https://i.imgur.com/BMyuEFc_d.jpg?maxwidth=520&shape=thumb&fidelity=high"/> <br/>
<br/>
<br />
4. Install Active Directory Domain Services and create domain. <br/>
<br/>
<img src="https://i.imgur.com/KPIyirQ_d.jpg?maxwidth=520&shape=thumb&fidelity=high"/> <br/>
<img src="https://i.imgur.com/HWmBBzt_d.jpg?maxwidth=520&shape=thumb&fidelity=high"/>
<br />
<br />
5. Create ADMINS organization and admin account. <br/>
<br/>
<img src="https://i.imgur.com/rwx9CaZ.png"/> <br/>
<img src="https://i.imgur.com/K3LjDPa.png"/>
<br />
<br />
6. Install Remote Access Services and configure NAT routing, so clients on private network can reach the internet through Domain Controller.  <br/>
<br/>
<img src="https://i.imgur.com/wyXPB84.png"/> <br/>
<img src="https://i.imgur.com/Ufn5oXY.png"/>
<br />
<br />
7. Set up DHCP on Domain Controller and set up DHCP scope, so Windows 10 machine can automatically get IP address assignment. <br/>
<br/>
<img src="https://i.imgur.com/tQsOnY5.png"/> <br/>
<img src="https://i.imgur.com/NW2CXGk.png"/>
<br />
<br />
8. Run powershell script to automatically create 1000 users in Active Directory (I break down this script in the next section).  <br/>
<br/>
<img src="https://i.imgur.com/QBEp6pB.png"/> <br/>
<img src="https://i.imgur.com/Cz0Ku4f.png"/>
<br />
<br />
9. Create Windows 10 machine and join it to the domain as Client01.  <br/>
<br/>
<img src="https://i.imgur.com/4TTRGMg.png"/>
<br />
<br />
10. You can now og in using a user account. Within the domain controller, the client shows up under Address Leases (in DHCP) and Computers (in AD). The domain can also be ping'd when logged into the client machine.  <br/>
<br/>
<img src="https://i.imgur.com/HMsGct3.png"/> <br/>
<br />
<br />


<h2>Breakdown of the Script</h2>

Full script:

    $PASSWORD_FOR_USERS   = "Password1"
    $USER_FIRST_LAST_LIST = Get-Content .\names.txt

    $password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force
    New-ADOrganizationalUnit -Name _USERS -ProtectedFromAccidentalDeletion $false

    foreach ($n in $USER_FIRST_LAST_LIST) {
        $first = $n.Split(" ")[0].ToLower()
        $last = $n.Split(" ")[1].ToLower()
        $username = "$($first.Substring(0,1))$($last)".ToLower()
        Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
    New-AdUser -AccountPassword $password `
               -GivenName $first `
               -Surname $last `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_USERS,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
               
Breakdown:
    
    $PASSWORD_FOR_USERS   = "Password1"
    $USER_FIRST_LAST_LIST = Get-Content .\names.txt
Sets "Password1" as password for all users. Pulls all content from text file with generated names into an array.
<br /> 

    $password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force
    New-ADOrganizationalUnit -Name _USERS -ProtectedFromAccidentalDeletion $false
Takes plain text password and creates a password object for it. Creates organizational unit named _USERS and protects account from accidental deletion.
<br /> 

    foreach ($n in $USER_FIRST_LAST_LIST) {
      $first = $n.Split(" ")[0].ToLower()
      $last = $n.Split(" ")[1].ToLower()
      $username = "$($first.Substring(0,1))$($last)".ToLower()
      Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
"$n" represents each user. Creates spaces between names by splitting them. Takes first character of name and adds it to the beginning of the last name to create a user name (ie. "mnugpo").
<br />     
 
    New-AdUser -AccountPassword $password `
               -GivenName $first `
               -Surname $last `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_USERS,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
Creates new user in Active Directory and enables account.
<br />  
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
