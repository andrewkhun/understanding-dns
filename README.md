<p align="center">
<img src="https://imgur.com/RJc1AiF.png" height = 70% width = 70%/>
</p>

<h1>Building Intuition for DNS</h1>
This lab showcases the practical application of DNS and provides guidance on its configuration. DNS, or Domain Name System, serves as a naming database facilitating the translation of internet domain names into IP addresses. It plays a crucial role in mapping user-friendly website names to the corresponding IP addresses used by computers to access those sites. As we configure and set up the system, hands-on exercises involving client and domain controller virtual machines will enhance our understanding of DNS.

</br>Please note, that this lab uses the Active Directory Setup lab as a pre-requisite.

  - [Active Directory Deployed in the Cloud (Azure)](https://github.com/andrewkhun/configure-ad)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute
- Remote Desktop
- Active Directory Domain Services
- Command Prompt

<h2>Operating Systems Used</h2>

- Windows 10 (21H2)
- Windows Server 2022

<h2>High-Level Overview</h2>

- A-Record Exercise
- Local DNS Cache Exercise
- CNAME Record Exercise


<h2>A-Record Exercise</h2>
<p>

An A-record, or Address Record, is a type of DNS (Domain Name System) record that links a domain name to its corresponding IPv4 address. It helps translate human-readable domain names (like w<span>ww.</span>example.com) into machine-readable IP addresses (such as 192.168.1.1), allowing computers to locate and connect to each other on the internet.
</p>
<p>
First, connect to both the DC-1 and Client-1 VMs through the Remote Desktop Connection application as your domain admin account (mydomain.com\jane_admin). In Client-1 open up the Command Prompt and ping 'mainframe'.
</p>
<p>
<img src="https://imgur.com/LZRlUoX.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
You will notice that it will fail because it cannot find any host named 'mainframe'. This is because as Client-1 attempts to ping 'mainframe', it will check its local cache for a hostname of 'mainframe', not find it, then continue to search it's Host file, then it's DNS server for any records. However, since we currently do not have any records with the name 'mainframe', the ping will fail.

To create an A-record with the name 'mainframe', go to the DC-1 VM and open the DNS manager from the Server Manager Application. Then within the DNS manager, right-click and select "New Host (A or AAAA). 
</p>
<p>
<img src="https://imgur.com/7pPMD1Y.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://imgur.com/q1O1mkB.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
Within the New Host window, input the name of the A-Record (mainframe), then input the local address of the Domain Controller (DC-1) and click Add host.
</p>
<p>
<img src="https://imgur.com/EeEosHl.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
Head back into Client-1 and try to ping 'mainframe' once again. You will notice the ping now goes through as we have created an A-Record that contains the hostname of 'mainframe' and an address that is associated with the name. If you input the command 'nslookup mainframe' you can see the information associated with the 'mainframe' including the domain and IP address.
</p>
<p>
<img src="https://imgur.com/ICaOy2J.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
