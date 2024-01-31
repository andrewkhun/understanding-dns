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
You will notice that it will fail because it cannot find any host named 'mainframe'. This is because as Client-1 attempts to ping 'mainframe', it will check its local cache for a hostname of 'mainframe', not find it, then continue to search its Host file, then its DNS server for any records. However, since we currently do not have any records with the name 'mainframe', the ping will fail.

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



<h2>Local DNS Cache</h2>
<p>
For this step, we will change the IP address of the A-Record in the previous exercise and observe Client-1's behavior as we try to ping 'mainframe' once again. Head into the DC-1 VM and edit the mainframe A-record properties and change the IP address to 8.8.8.8. Once changed, head back into Client-1 and ping mainframe from the command prompt once more.
</p>
<p>
<img src="https://imgur.com/7tuixz3.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
You will see after pinging mainframe, although we have changed the IP address associated with it, Client-1 is still receiving a response from the previous IP address that was associated with mainframe. This is because the old IP address is locally cached and stored on the VM. It doesn't need to communicate with the DNS server since it already has a cached record of mainframe.  We can view this if we input the command ipconfig /displaydns in Client-1's command prompt. You will see that the previous IP address is still associated with mainframe.
</p>
<p>
<img src="https://imgur.com/13TTJiY.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
To update the cache on Client-1, input the command ipconfig /flushdns to empty the cache. If you are unable to execute the command, re-open Command Prompt as Administrator. Once the command has been executed, ping mainframe once again and you will see an updated ping attempt to the new IP address. If you input the command ipconfig /displaydns, you can see the updated IP address associated with the A-Record.
</p>
<p>
<img src="https://imgur.com/JVR4dbE.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>







<h2>CNAME Record Exercise</h2>
<p>
A CNAME (Canonical Name) is a type of DNS record used to alias one domain name to another. It is often used to make one domain an alias of another, allowing users to access a website or resource using different domain names.

To make a CNAME record the process is very similar to creating an A-Record. Head back into DC-1 and inside the DNS Manager, right-click and select New Alias (CNAME)...
</p>
<p>
<img src="https://imgur.com/p8hmdTv.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
Name the new Alias name 'search' and for FQDN type in w<span>ww.</span>google.com and click OK. This will associate the name 'search' with w<span>ww.</span>google.com. 

Head back into Client-1 and input the command 'ping search'.
</p>
<p>
<img src="https://imgur.com/hBuxE9C.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
You can see the ping command went through to find the 'search' alias which points to w<span>ww.</span>google.com. If we perform the nslookup search command, we can see the DNS information associated with 'search'. 
</p>
