<p align="center">
<img src="https://i.imgur.com/WZYhzEU.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in Microsoft Azure</h1>
In this lab I deployed Active Directory services, experimented with DNS resolution, and experimented with Fileshares & Permissions.<br />

<h2>Technology Utilized</h2>

- Microsoft Azure (Virtual Machine/Cloud Computing)
- Remote Desktop
- Active Directory Domain Services
- Windows Command Line

<h2>Operating Systems</h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Overview of Tasks</h2>

- Provision a Windows Server 2022 machine and Windows 10 Pro machine in the same Azure Resource Group. 
- Ensure network connectivity between the Server machine and Client machine.
- Set up and deploy Active Directory Services.
- Connect the Client to the Domain.
- Create Organizational Units (OU) and Users in AD Users and Computers
- Experiment with DNS resolution with the Client.
- Experiment with Fileshares and Permissions in the AD environment.

<h2>Deploy and Configure</h2>

<p>
<img src="https://i.imgur.com/QsLb2Sm.png" height="80%" width="80%" alt="Machine Deployment"/>
</p>
<p>
I created a Resource Group in Microsoft Azure. I then deployed a Windows 2022 Server machine and a Windows 10 Pro machine inside of it.
  While provisioning machines, I ensured both were on the same virtual network and subnet. I then configured the server machine's IPv4 address
  to be static. Once both machines were provisioned, I connected to both of them with RDP (Remote Desktop Protocol) from my host computer. 
  I then ensured there was network connectivity between the two machines, by pinging the server from the client machine (Windows 10 Pro).
  In order for the client's pings to be successful, I configured the server's Firewall to allow incoming ICMP(v4) connections.
  
</p>
<br />

<p>
<img src="https://i.imgur.com/Bpg43ib.png" height="80%" width="80%" alt="Deploying AD"/>
</p>
<p>
Next I turned the server machine into a Domain Controller. I installed Active Directory Domain Services onto the server from the Server Manger
  dashboard. During this process I set up a new forest, "mydomain.com", and promoted the server as a Domain Controller. I then configured the client machine's NIC to use the server machine's IPv4 address as the DNS server it used. 
</p>
<br />

<p>
<img src="https://i.imgur.com/xdeu5Y4.png" height="80%" width="80%" alt="AD Users and Computers"/>
</p>
<p>
Next I created an admin account and normal user accounts with Active Directory Users and Computers. I created two new Organization Units (OU), Employees and Admins.
  I created a new user and granted them Admin permissions. I then logged out of the DC and logged back in under this account. I then created other, normal, users. I then joined the client machine to my newly created domain. I verified the client machine was joined to the domain by viewing it in Active Directory Users and Computers on the DC machine. I then logged into the client machine as the admin user and enabled remote desktop for domain users. I verified RD was enabled by logging into the client machine as one of the normal users I created.
</p>
<br />

<h2>Experimenting with DNS</h2>

<p>
<img src="https://i.imgur.com/pAQnvQC.png" width="80%" alt="DNS Manager"/>
</p>
<p>
I experimented with DNS with the client and server(DC) machines. I tried to ping a random word (mainframe) from the client machine. I observed the failure. 
  I then went into the server machine's DNS Manager and created a new host within the domain folder. The new host's name was the word "mainframe" and the IPv4 address for this host was set to the DC's IPv4 address. I then pinged the word mainframe from the client again and observed successful pings. The pings returned from the DC's IPv4 address.
</p>
<br />

<p>
<img src="https://i.imgur.com/jeFCZJr.png" height="80%" width="80%" alt="Failed and Successful Ping"/>
</p>
<p>
I then expiremented with manipulating my custom host's address and the client's DNS table. I changed the "mainframe" host's IPv4 address to "8.8.8.8". I then pinged the word mainframe from the client. I observed the output of the ping to return from the DC's address. This demonstrated the client machine did not request resolution from the DC, as the previous address for mainframe was stored in its own DNS table. I verfied the client's saved address for mainframe by looking at its DNS table. This was done from the command line (ipconfig /displaydns). I then flushed the client's DNS table from the command line (ipconfig /flushdns). I pinged the word mainframe again and observed the output to return from 8.8.8.8, demonstrating the client asked the DC for resolution. I also experimented with Alias resolution by adding a random word to DNS Manager and having it resolve to a specific URL.
</p>
<br />

<p>
<img src="https://i.imgur.com/U86t78h.png" height="80%" width="80%" alt="DNS Display and Flush"/>
</p>
<p>

</p>
<br />

<h2>Experimenting with Fileshares and Permissions</h2>

<p>
<img src="https://i.imgur.com/5sNTkxo.png" height="80%" width="80%" alt="Advanced Security Settings"/>
</p>
<p>
I experimented with fileshares and permissions on my domain. While logged into the DC as an admin, I created four different files on the C:\ drive. I then assigned the different folders with varying permissions. One granted Domain Users read permission, one granted Domain Users read and write permissions, one provided only Admins full permissions, and the last provided a specific group permissions to read and write. I also made sure to share these folders across the domain. 
</p>
<br />

<p>
<img src="https://i.imgur.com/C7yL02O.png" height="80%" width="80%" alt="Assign Permissions"/>
</p>
<p>
I then logged into the client as a normal Domain User. I then navigated to the domain folder "\\dc-1\" and tried accessing the different folders. I observed this user to only be able to read from one folder and to read and write to another folder. The user could not access the other two folders.
</p>
<br />

<p>
<img src="https://i.imgur.com/sZnmYkp.png" height="80%" width="80%" alt="Restricted Access"/>
</p>
<p>
I then added the normal Domain User to the specific group that was allowed to access the last folder. The user was the able to successfully read and write to this folder.
</p>
<br />
