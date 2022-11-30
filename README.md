<h1>Active Directory Home Lab</h1>

 

<h2>Description</h2>
In this lab, we will use Oracle VM VirtualBox to set up a virtual lab running Active Directory with NAT on a domain controller. Including a basic group policy to give users administrative rights. The project also consists of Powershell scripting to build and develop over 1,000 users to simulate a real working environment. The DHCP implementation is also used to route traffic from an external client to the NIC of the main domain.

<br />


<h2>Languages and Utilities Used</h2>
- <b>Active Directory</b> 
- <b>PowerShell</b> 
- <b>CMD</b>

<h2>Environments Used </h2>

- <b>Oracle VM VirtualBox 7.0</b> 
- <b>Windows 10(21H2) </b> 
- <b>Microsoft Server 2019</b> 

<h2>Program walk-through:</h2>

<p align="center">
Network Diagram that will be guiding this project <br/>
<img src="https://i.imgur.com/6Nqb9kJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Virtual machines hosting domain controllers require two network adapters. One using NAT from the home router's host IP address, and another using the internal network adapter so that the DC can communicate with other virtual machines. Use intnet for the internal network.  <br/>
<img src="https://i.imgur.com/68VPD15.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<img src="https://i.imgur.com/KJKsB5T.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<b>First we find out which one of these adapters is connected to home router's NAT</b> <br/>
<img src="https://i.imgur.com/JE7zCDh.jpg" height="80%" width="80%" alt="Configuring the Network"/>
<br />
<br />
<img src="https://i.imgur.com/cZjYxf9.png" height="80%" width="80%" alt="Configuring the Network"/>
<br />
<br />
<b>To make things easier, I renamed the adapters to identify them for DHCP configuration later.</b> <br/>
<img src="https://i.imgur.com/Lw8rJI4.png" height="80%" width="80%" alt="Configuring the Network"/>
<br />
<br />
<b>Now  configure the internal network adapter and assign an IP address shown in the diagram.  You don't need to specify a default gateway because the domain controller is the gateway. When you install Active Directory, DNS is installed, so you only need to set a loopback address. </b> <br/>
<img src="https://i.imgur.com/zHSWyN8.png" height="80%" width="80%" alt="Configuring the Network"/>
<br />
<br />
<b>At this point you can also rename the PC to something more simple, I chose to go with DC for Domain Controller</b> <br/>
<img src="https://i.imgur.com/TklrxzC.jpg" height="80%" width="80%" alt="Renaming the PC"/>
<br />
<br />
<b>Once your system is up and running again, go to your Server Manager and add features and roles to start downloading and installing Active Directory.</b> <br/>
<img src="https://i.imgur.com/GOqCTV8.png" height="80%" width="80%" alt="Renaming the PC"/>

<br />
<br />
<p align="center">
<b>Remember to finish the deployment configuration, as this is where you (actually) create your domain, for this case i'll be using "mydomain.com" </b> <br/>
</p>
<img src="https://i.imgur.com/Juv6rLY.png" height="80%" width="80%" alt="Renaming the PC"/>
<img src="https://i.imgur.com/kSw8g69.png" height="80%" width="80%" alt="Renaming the PC"/>
<br />
<br />
<p align="center">
<b>After your domain is created, the next time your VM restarts, you should be seeing your domain at the beginning of the User's name. This wil indicate that the domain was created successfully. </b> <br/>
</p>
<img src="https://i.imgur.com/BmLk2Gc.jpg" height="80%" width="80%" alt="Renaming the PC"/>
<br />
<br />
<p align="center">
<b>Now instead of using the built in Admin account, I will create a dedicated domain Admin account </b> <br/>
</p>
<img src="https://i.imgur.com/8NqwiG0.png" height="80%" width="80%" alt="Renaming the PC"/>
<img src="https://i.imgur.com/vDXLG9h.png" height="80%" width="80%" alt="Renaming the PC"/>
<img src="https://i.imgur.com/dNZ5HQ0.png" height="80%" width="80%" alt="Renaming the PC"/>
<br />
<br />

<br />
<br />
<p align="center">
<b>Now you need to install and configure RAS/NAT to allow your Windows 10 client machines to access the internet over your internal network through your domain controller.</b> <br/>
</p>
<img src="https://i.imgur.com/HGL1rph.png" height="80%" width="80%" alt="Renaming the PC"/>
<img src="https://i.imgur.com/P5xzIGH.png" height="80%" width="80%" alt="Renaming the PC"/>


<br />
<br />
<p align="center">
<b>In order to configure Remote Access, remember to select the Adapter that has acess to the public INTERNET</b> <br/>
</p>
<img src="https://i.imgur.com/94gG3wd.png" height="80%" width="80%" alt="Renaming the PC"/>
<img src="https://i.imgur.com/px6fsPG.png" height="80%" width="80%" alt="Renaming the PC"/>
<img src="https://i.imgur.com/cWRdX3c.png" height="80%" width="80%" alt="Renaming the PC"/>

<br />
<br />
<p align="center">
<b>After installing and configuring remote access, install a DHCP server. This will give your Windows 10 client an IP address and allow you to browse the web.</b> <br/>
</p>
<img src="https://i.imgur.com/En3Z3Cx.png" height="80%" width="80%" alt="Renaming the PC"/>
<img src="https://i.imgur.com/vBZCbuX.png" height="80%" width="80%" alt="Renaming the PC"/>


<br />
<br />
<p align="center">
<b>Configure DHCP and set scopes here. The overall purpose of DHCP is to allow computers on a network to be automatically assigned IP addresses. The range we are going to create will assign IP addresses in the range 172.16.0.100-200, so DHCP can effectively assign 100 IP addresses. Leases are also created so if you only have 100 IP addresses in that range, new devices can be assigned IP addresses on your network periodically. For example, if this is a coffee shop that offers WiFi and the average time he spends in that coffee shop is two hours, it doesn't make sense to rent an IP address to him for 20 days. Doing so effectively prevents anyone else from using it. If this is a coffee shop, one would recommend setting the lease period to less than 4 hours and offering a longer range. Although this is a Lab setting, it's good to keep in mind.</b> <br/>
</p>
<img src="https://i.imgur.com/80dRSlx.png" height="80%" width="80%" alt="Renaming the PC"/>
<img src="https://i.imgur.com/9v4CPOU.png" height="80%" width="80%" alt="Renaming the PC"/>
<img src="https://i.imgur.com/aKcIanH.png" height="80%" width="80%" alt="Renaming the PC"/>
<img src="https://i.imgur.com/Q4qY9L6.png" height="80%" width="80%" alt="Renaming the PC"/>
<img src="https://i.imgur.com/P3IYgrO.png" height="80%" width="80%" alt="Renaming the PC"/>
<br />
<br />
<br />
<br />
<p align="center">
<b>Now that both Active Directory and my Domain Controller are configured, Here's how you can use the Powershell script provided to create a thousand user accounts.</b> <br/>
</p>
<img src="https://i.imgur.com/eKpjnVz.png" height="80%" width="80%" alt="Using Powershell to create 1000 user accounts in bulk"/>
<p align="center">
<b>Also remember to set the execution policy to unrestricted, otherwise the code won't allow you you to run the script. Hence typing "set-ExecutionPolicy unrestricted" into Windows Powershell beforehand </b> <br/>
<img src="https://i.imgur.com/Q4xwIzE.png" height="80%" width="80%" alt="Using Powershell to create 1000 user accounts in bulk"/>
<br />
<br />
<p align="center">
<b>As shown in my diagram, CLIENT1 refers to a Windows 10 Virtual Machine that will serve as an office/employee computer to test the Active Directory. </b> <br/>
</p>
<img src="https://i.imgur.com/5iCyRk8.png" height="80%" width="80%" alt="Setting up new Virtual Machine"/>
<br />
<br />
<p align="center">
<b>I have my network adapter set to the Internal network of the Virtual Machine so it cannot connect to the internet on my local network. Hence, making it only able to connect to the internet only if it is assigned an IP by the server VM's Domain Controller. See the illustration at the beginning. You need to change the network adapter to be in the same internal network as the domain controller.</b>  <br/>
</p>
<img src="https://i.imgur.com/sfzOLS5.png" height="80%" width="80%" alt="Configuring the VM Network Adapter"/>
<br />
<br />
<p align="center">
<b>Now you just need to rename the CLIENT1 PC, and add it to the mydomain.com domain with Administrative credentials</b>  <br/>
</p>
<img src="https://i.imgur.com/6FTPk42.png" height="80%" width="80%" alt="Configuring the Client VM"/>
<img src="https://i.imgur.com/VkisQz7.png" height="80%" width="80%" alt="Configuring the Client VM"/>
<br />
<br />
<p align="center">
<b>Domain member confirmation</b>  <br/>
</p>
<img src="https://i.imgur.com/euBgIXf.jpg" height="80%" width="80%" alt="Configuring the Client VM"/>
<br />
<br />
<p align="center">
<b>I log into a user account I created from the Powershell script to test if everything is configured correctly. Instead of logging into the user account created when I made the virtual machine, I try to log into a user created account in MYDOMAIN</b>  <br/>
</p>
<img src="https://i.imgur.com/DPwmPuw.png" height="80%" width="80%" alt="Testing The Environment"/>
<br />
<br />
<p align="center">
<b>Running command promt to see if the client VM is getting the IP address properly assigned by the DC. We can see that I was properly leased an IP address by the domain controller (circled red) and when I ping the domain, it works (circled yellow)</b>  <br/>
</p>
<img src="https://i.imgur.com/QBWuCS9.jpg" height="80%" width="80%" alt="Testing The Environment"/>
<br />
<br />
<br />
<br />
<p align="center">
<b>Just to be completely sure, you could always go back into your VM to check leased addresses, this is where you would see which IP's are being used or not used.</b>  <br/>
</p>
<img src="https://i.imgur.com/qvNKa7v.jpg" height="80%" width="80%" alt="Checking leased addresses"/>
<br />
<br />
<p align="center">
<b>Go into the Computers folder to confirm the connection between CLIENT1 and the Domain, this gives some perspective on how most Offices and Schools manage their own Active Directory. r</b>  <br/>
</p>
<img src="https://i.imgur.com/A2dMovv.jpg" height="80%" width="80%" alt="Checking the computers in Active Dirctory"/>
<br />
<br />
<p align="center">
<b>A thousand new user's created all with Powershell and Windows Server 2019, Active Directory has never been more easy. </b>  <br/>
</p>
<img src="https://i.imgur.com/POpjnf9.gif" height="80%" width="80%" alt="Checking the Users created by Powershell"/>
<br />
<br />
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
