<h1>Ransomware Incident Response Lab IN PROGRESS</h1>



<h2>Description</h2> 
In this lab/CTF, my task was to investigate an Exchange server that the users could not connect to and a suspicious file appearing as a ransome note on their drive. After reviewing ransom note, it indicates the potential involvement of Conti ransomware. The lab provided several tools for analysis, such as Splunk logs, a Wireshark packet capture, and a Redline image of the affected system. Given my experience with Splunk and Wireshark, I focused on learning Redline by finding as much evidence with that tool as I could first before pivoting to Splunk logs to identify indicators of compromise.<br />
<br />

I started by opening the .mans file with Redline and conducted basic triage, identifying the operating system and a potentially suspicious user named "securityninja." Examining this user's Desktop and Documents folders, I found files with the .FEALC extension, a strong indication of ransomware. Checking the timeline logs during the incident, I found logs of the webshell attack on the Exchange server and the creation of a ransomware file under C:\Users\Administrator\Documents\cmd.exe. Knowing cmd.exe should reside in System32, this was clearly suspicious. Uploading the file hash to VirusTotal confirmed it as Conti ransomware. Continuing with Redline, I found evidence of when the encryption began, ransome note left in multiple folders and the attackers' attempts to delete logs. <br />
<br />
After reviewing copius logs in Redlien, I pivoted to Wireshark where I identified multiple public IP addresses related to trojan droppers used for malware delivery. However, this Wireshark did not prove very useful i nthis instance as much of the data transferred was encrytped. I then swtiched to Network Miner, another packet analysis tool, where I was to pinpoint the exact time the attackers first accessed the Exchange server.<br />
<br />

Finally, I turned to Splunk to get a clearer picture of the attack. I quickly found the ransomware binary and confirmed it matched the hash found in Redline. I discovered the commands used to create the "securityninja" user and that it was added to Remote Desktop Users group for persistence. Investigating CreateRemoteThread logs commonly used by threat actors, I found the attackers targeted lsass.exe, which holds authentication hashes, and unsecapp.exe, used for remote communication to webservers. I also uncovered a long, obfuscated PowerShell script spawned by C:\Windows\System32\inetsrv\w3wp.exe, likely used to download the ransomware or exploit vulnerabilities in Microsoft Exchange Servers.<br />

Once the attack chain and timeline was clear I coducted further research of Conti ransomware attacks on Exchange servers, I learned that the group leverages three different CVEs in their attack chain.
<br />


<h2>Languages and Utilities Used</h2>

- <b>Linux Terminal</b>
- <b>Network Miner</b>
- <b>Wireshark</b>
- <b>FireEye Redline</b>
- </b>Splunk/Search Processing Language (SPL)</b> 

<h2>Environments Used </h2>

- <b>Oracle Virtual Box</b>
- <b>Windows 10 </b>
- <b>Kali Linux</b>
<h2>Lab Overview</h2>

<p align="center">

The scenario of the attack and ransome note. DISCLAIMER: DO NOT attempt to connect to any addresses in the ransome note.<br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/cc27e2c5-b362-49fc-8396-3ea4836a75a8" height="50%" width="80%" alt="Ransomware Incident Response"/>
 <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/49dab900-24f2-415c-9a0f-3b28a33ce789" height="50%" width="80%" alt="Ransomware Incident Response"/>
  <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/e6bf0f6a-4eab-4423-9060-abcd1a420d75" height="50%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
I begun my investigation with Redline and when conducting the first part of the triaging process, examining the local user accounts, I noticed a suspicious user "securityninja" with Administrator privliges.<br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/31219591-feca-4c61-9191-08484281b42a" height="80%" width="80%" alt="Ransomware Incident Response"/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/9941f34c-c66a-4b78-ba07-a832d9c0b404" height="80%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
I then looked at Documents and Desktop for evidence of the ransomware and found what likely was multiple ransome notes and encrypted files. The file extention .FEALC appearing on many important files is an indicator of ransomware encryption. <br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/885dad2d-154e-4301-a48e-1df286f2f7a0" height="80%" width="80%" alt="Ransomware Incident Response"/>
 <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/1e68881c-2bb7-4a79-b9b5-b7d8bbbcf4f1" height="80%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
After it was clear from the inital triaging, ransomware activity occured on the device. I then begun further exploring the host to find initial access, indicators of attack, and the ransomware itself. Due to me being inexperienced with Redline, the bulk of my investigations revolved around using the timeline tab and scrolling through the thousands of logs looking for anything that exhibits malicious/suspicious behavior and I came accross a webshell accessing the MS exchange server likely showing the inital breech by the attackers.<br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/6d885b79-96c7-488f-8dd5-74b2bc65e666" height="80%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
I came accross the log where the attacker added the suspicious user "securityninja" likely for persistance after the attack.<br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/2a4e36eb-b136-4562-8e57-276bea7184f6" height="80%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
Further down in the logs, I came accross a file creation that seemed suspicous as it was cmd.exe within the Documents folder and upon further analysis this was confirmed to be the ransomware. I obtained the hash in Redline and 62 vendors on VirtusTotal confirmed the file to be Conti ransomware.<br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/06a77c20-da11-4f9e-aa77-f2e1867b4638" height="50%" width="80%" alt="Ransomware Incident Response"/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/4e8265e1-c59a-44b7-b665-3fa921466282" height="50%" width="80%" alt="Ransomware Incident Response"/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/46dbbee8-f6bb-491a-834c-2f780305d687" height="50%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
I had already discovered the file extension of the encypted files earlier in my investigation when looking at the User's desktop and docuemnts. When filtering the file extension I  and discovered that well before the bulk encryption of the host and exhange server, the attackers targeted different log instances both on the host and the exchange server.<br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/4da249db-89e7-4692-93cf-2c9d55edd0bb" height="50%" width="80%" alt="Ransomware Incident Response"/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/04e046e3-bed6-4fd6-b2fc-b4357981687d" height="50%" width="80%" alt="Ransomware Incident Response"/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/c355f325-73a7-404f-b843-9bc1c83a55f4" height="50%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
This behavior was also seen with the ransomnotes "readme.txt" files, as they first appeared on the exchange server well before the being created en masse on the host machine. <br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/6cbcefc9-a92f-4d23-be86-7b000fe7132b" height="50%" width="80%" alt="Ransomware Incident Response"/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/c9fa2b84-585b-4e2d-86f4-b14c82957235" height="50%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
As I was running out of options in Redline I decided to switch to the packet capture in Wireshark to see if I can find any further evidence of inital access or Indicators of Compromise. However, the bulk of the traffic captured by Wireshark was encrypted LDAP traffic so I did not find much useful inforamtion other than confirming the majority of traffic was between private IP addresses on the same network. However, I discovered the few public IP addresses communciating with the host had considerable malicious realtions wihtin VirusTotal despite the IP itself being clean.<br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/892dd3ba-948b-42fe-82f8-44046f048781" height="50%" width="80%" alt="Ransomware Incident Response"/>
 <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/56b15a44-8e6a-4fae-9da3-bfc2a80927b2" height="50%" width="80%" alt="Ransomware Incident Response"/>
 <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/fd0749ec-7cd9-4256-81a2-a70482208a1f" height="50%" width="80%" alt="Ransomware Incident Response"/>
 <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/7393942e-30bc-44fc-84d4-68dc9a101106" height="50%" width="80%" alt="Ransomware Incident Response"/>
 <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/140e8a7d-51c6-4425-9314-61a64392366d" height="50%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
I then tried my luck Network Miner which is also a packet analysis tool. I had to first convert the .pcapng file to a standard .pcap file and then opened it in Network Miner. Within Netork Miner I found one useful piece of inforamtion which was the initial connection the threat actors made the exchange server which helps build the timeline of the attack. <br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/1745c454-dc95-4c11-9eb0-5ce21dec9e60" height="50%" width="80%" alt="Ransomware Incident Response"/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/5d50f619-761d-43c5-bac6-18ef6fca6bf3" height="50%" width="80%" alt="Ransomware Incident Response"/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/feb67f0b-6d0b-4dbe-a4e4-aff38cd8bd3b" height="50%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
Finally, I decided to use the Splunk instance to review the incident as this is the tool that I am most experienced with. The first Search Procssing Lnaguage (SPL) query I made was exmaining the Images which in Splunk are locations of binaries. I immediatly found what was likely the ransomware that I found in Redline, a cmd.exe within the User\Documents folder. I knew it was suspicous becasue cmd.exe should be only in C:\Windows\System32 and seeing it another direcotry warrants further investigation. <br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/a2fb3b01-f6cf-4efe-a9fe-97d5ce40c4b8" height="50%" width="80%" alt="Ransomware Incident Response"/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/647b995c-3d6c-47df-9e03-92d13a527445" height="50%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
I then added the suspicious image to the filter and under extract new fields, I extracted the hashes field so I can confirm whether this is the ransomware I found in Redline. Upon uploading the sha256 hash to VirusTotal it was confirmed to be Conti Ransomware.<br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/c747ad2d-b27f-4089-a113-c4bb87abbf00" height="50%" width="80%" alt="Ransomware Incident Response"/>
 <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/d0817c5d-50b9-43c8-97fb-2aad2f470540" height="50%" width="80%" alt="Ransomware Incident Response"/>
 <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/897ee06a-d997-4d2b-a31f-6c1e0332341c" height="50%" width="80%" alt="Ransomware Incident Response"/>
 <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/c99074fb-283d-44d0-b944-4fb728f77d04" height="50%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
Now that I confirmed the Image of the ransomware, I used the field "TargetFilename" and discovered the the ransome note "readme.txt" was saved to multiple locations on the host. <br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/096dbf3f-48db-4dbb-8991-9fa4972b7f87" height="80%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
Looking at my notes for the investigation so far, I noticed the account "securityninja" so I begun searching for evidence of the user creation in Splunk. I filterd for "EventCode=4720" which is the Windows Event Log for user account creation and was successful. I then made SPL query for "securityninja" with the CommandLine and ParentCommandLine field and saw the commands used to create, give administrator privileges, and add the user to "Remote Desktop Users" for persistance. <br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/8de546a2-4c5b-431e-81e7-bcbf032b40ae" height="50%" width="80%" alt="Ransomware Incident Response"/>
 <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/82b10c03-da6b-4989-ab7d-8413a0707f9c" height="50%" width="80%" alt="Ransomware Incident Response"/>
 <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/f461a61f-c68a-4c64-806f-e87f02e564c4" height="50%" width="80%" alt="Ransomware Incident Response"/>
 <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/21392712-2e52-4aa7-a84a-59f344299807" height="50%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
Since the logs in Splunk had Sysmon logs, I knew that EventCode 8 is avery useful search for threat hunting as it logs CreateRemoteThread which threat actors use which is whether a malicious process creates a thread in another usually clean process. Two very interesting binaries were targeted, "C:\Windows\System32\lsass.exe" and "C:\Windows\System32\unsecapp.exe". I know that lsass.exe is a common target for threat actors as it contains credential authentication hashes. Unsecapp.exe on the other hand, after some researtch I found out it is part of Windows Management Instrumentation and is used to "adapt the communication between the software of your computer and the remote server".<br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/32f8268e-ac95-4089-91ca-2f05fd32815f" height="80%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
Lastly in Splunk, as a shot in the dark because the attack targeted Windows machines, I filtered to see if the the attackers used Powershell and cleared out all the Splunk Forwarder logs. I uncovered something clearly malicious. The log showed the process w3wp.exe which I found out is an Internet Information Service responsible for handling requests sent to a Web Server for a specific application pool, spawned a long and heavily obsufcated Powershell script on the exchange server.<br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/941ba7c9-19da-4604-a871-3ffd5a2eba7e" height="50%" width="80%" alt="Ransomware Incident Response"/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/a8223054-da2f-45f2-9be8-7aef7dc33ef5" height="50%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
With ample evidence of the attack now obtained, I conducted further research about the Exchange Server attack with Conti Ransomware and came accross an article detailing that the group leverages three vulnerabilites (CVEs) against Microsoft Exchange Server's in their attack chain. One CVE to Bypass security measures, one for Privlige Escalation, and one for Remote Code Execution.<br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/7f8ebf6d-58d4-45ab-83e3-4e41d803e88c" height="50%" width="80%" alt="Ransomware Incident Response"/>
 <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/e2ed6d9d-0ffc-4416-9c6b-d307c6ffd62e" height="50%" width="80%" alt="Ransomware Incident Response"/>
 <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/fe4ba104-7013-48d5-bdfe-a1e548cf10ae" height="50%" width="80%" alt="Ransomware Incident Response"/>
 <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/8296f53a-1f04-45fa-8b5e-836c0c2e47f0" height="50%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />



<h2>Thoughts</h2>

ChatGPT
This lab/CTF was the most complex I've done thus far, as the task had very little direction and many possible avenues and tools to use. I challenged myself to use Redline to answer as many questions as possible, which provided significant hands-on experience but took considerably longer compared to using Splunk to find the same and more evidence of the attack. Additionally, I spent a lot of time sifting through packets in Wireshark without finding much. It was frustrating not having hints on where to look or which tool to use, but it taught me to be thorough. This experience highlighted that no single tool is best for everything, although I had the most success with Splunk, possibly due to my previous experience with it. The logs I examined felt like they were from a real attack rather than a lab setup for security researchers, due to the complexity of the attack and the multiple angles the attackers used to target the Exchange server and the host machine. My key takeaways from this lab include the observation that ransomware attacks often involve multiple IP addresses for establishing command and control servers, downloading additional malware, or enumerating the target. This lab also improved my research skills, exposing me to many Windows processes I was previously unaware of. Being able to quickly research and understand how attackers leverage these services is a crucial skill for a security analyst.
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
