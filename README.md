# MyDFIR-SOC-Challenge

This project consisted of creating a SOC environment linking multiple virutal machines with elastic agents and a ticketing system.

MYDFIR 30 DAY CHALLENGE



DAY 1:

Created a logical diagram for the SOC network using Draw.io.

![Logical Diagram](https://i.imgur.com/qRBTVTG.png)


DAY 2:

Created an account on vultr.com and created virtual machines.  
Created an ELK server running Ubuntu 22.04.  
Installed and configured elastic search on the ELK server.
Added firewall group to the ELK server on vultr.com to accept SSH connections only from my IP address.


DAY 3:

Installed and configured Kibana on the ELK server.
Edited the firewall settings for the ELK server to allow TCP connections over any port for my IP address only.
Logged into Elastic, generated encryption keys, and added them to the ELK server.


DAY 4:

Created a new virtual machine running Windows Standard 2022 x64.
Moved the Windows and Ubuntu servers out of the VPC for security.
Edited the logical diagram to reflect this as shown below.

![Logical Diagram UPDATED](https://i.imgur.com/IWIT0Gv.png)

This resulted in the Windows and Ubuntu servers existing outside the virtual private cloud, which means the Windows and Ubuntu servers will not have the same private IP address.  So, if an attacker was able to compromise either server it would be isolated from everything else.

Now the Windows server is exposed to the internet via remote desktop protocol (RDP).  




DAY 5:

Created a new virtual machine called “Fleet Server” running Ubuntu 22.04.
Added Fleet server in elastic.
Installed elastic agent on Fleet server.
Installed elastic agent on Windows server machine.


DAY 6:

Installed and configured Sysmon on the Windows server and confirmed it is generating events in the Windows event viewer.


DAY 7:

Set up Elastic so both Sysmon and Windows Defender logs are now being ingested into the Elastic Search instance.


DAY 8:

Created a new virtual machine running Ubuntu 24.04.  We can now see failed authentication attempts in real-time.


DAY 9: 

Installed elastic agent onto SSH server and can now view events within the elastic search instance.


DAY 10:

Created alerts and dashboards in Kibana.  Also created SSH network maps showing both failed and successful SSH authentications across the world based on geolocation.  

![Failed and Successful SSH Authentications](https://i.imgur.com/4o8LQYR.png)

DAY 11:

Created alerts and dashboards in Kibana.  Also created RDP network maps showing both failed and successful RDP authentications across the world based on geolocation.  
![Failed and Successful RDP Authentications](https://i.imgur.com/9wUO8SU.png)

DAY 12:

Created a table layout for both RDP and SSH brute force attempts that correlate to each map.  These tables show the top 10 usernames, source IPs, source countries, and number of records.

![Table for SSH](https://i.imgur.com/dpwscyU.png)
![Table for RDP](https://i.imgur.com/7lYDeZk.png)

DAY 13:

Created an attack diagram in Draw.io as shown below.  This details each phase of the C2 attack.  Phase 1 is initial access where we use our Kali Linux machine to perform an RDP brute force attack against the Windows server.  Once we have a successful authentication, we move on to Phase 2.  This phase will perform some discovery commands which are (whoami, ipconfig, net user, and net group).  Phase 3 is defense evasion with an RDP-established session we will then disable Windows Defender on the Windows server.  Phase 4 is execution.  We will use Powershell invoke expression to download Mythic agent from our C2 server.  We then execute the Mythic agent and move to Phase 5 which is command and control.  With a successfully established C2 session, we move to Phase 6 where we perform exfiltration by downloading a password file from our Windows server using the existing C2 session.

![Attack Diagram](https://i.imgur.com/TSSPucQ.png)

DAY 14:

Created a new virtual machine running Ubuntu 22.04 as the Mythic server.  Then SSH into the new VM and installed docker-compose.  Cloned the GitHub repository for Mythic and installed docker.  Created a new firewall rule for the Mythic server that allows connection over any TCP port from my IP only.  Added another rule specifically to allow the Windows server and Linux server IPs to connect.

Installed a Kali Linux virtual machine on the home system 


DAY 15: 

Used crowbar tool via Kali Linux to perform a brute force attack against the Windows server.  After obtaining the username and password for the Windows server, logged in using xfreerdp on a Linux machine.  Then performed commands whoami, ipconfig, net user, and net group via the command prompt.  Disabled Windows Defender.  Using the mythic server, installed the mythic agent “Apollo”.  Installed mythic profile for “HTTP”.  Generated a new payload with the Windows server as the target.  Downloaded the payload on the mythic server.  Successfully exfiltrated the password file from the Windows server.


DAY 16:

Created an alert and dashboard for the Mythic C2 Apollo agent along with the processes that are created and processes initiated.


DAY 17:

Created a new virtual machine running Windows Standard 2022 x64.  Added a firewall group to keep the new VM secure.  In the VM, installed and configured XAMPP.  Edited the configuration file to match the public IP address of the VM.   Changed the phpMyAdmin config file to change the local host to the public IP address.  Using Windows Defender, created a new rule to allow inbound connections over ports 80 and 443.  Then installed and configured osTicket version 1.18.1.  Now have a fully functioning ticketing system.


DAY 18:

Successfully integrated osTicket with ELK and sent alerts into the ticketing system.  Added a new API key using the private IP of the ELK server since both instances exist in the same VPC.  Copied the API key from osTicket into Elastic using webhook connector.  

![Ticketing System](https://i.imgur.com/k5CLP8C.png)

DAY 19:

Investigated SSH brute force attacks.  Looking at a recent alert, investigated the IP address using abuseipdb.com.  This revealed the IP is known to perform brute-force attacks.  Checked the IP in Kibana and found it affects other users.  Found no successful logons.  If the attack had been successful, the investigation would have continued.  Followed the same investigation patterns for both RDP and Mythic servers.  Triaged alerts in osTicket.  


DAY 20: 

Installed and configured Elastic Defend.  Selected traditional endpoints and completed EDR.  Then added this to the Windows policy agent.  Created a malware prevention alert and set up a response action to isolate the host.  
![image](https://github.com/user-attachments/assets/9a3fdc25-8837-4311-b768-cc771c9a4a6f)
