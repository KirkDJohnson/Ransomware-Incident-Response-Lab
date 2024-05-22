<h1>Ransomware Incident Response Lab IN PROGRESS</h1>



<h2>Description</h2> In this lab/CTF, my task was to investigate an exchange server that the admins could not connect to and a weird file on thier drive. I nthe scernio the ransomnote was shown as part of the setup so it was initally clear that the ransomware Conti was invovled in the incident. Intrestingly, this lab walk-through provided a considerable amount of tools to analyze the incident such as logs Splunk, a packet capture from wireshark, and a Redline image of the effected system. Due to me having experinece using Splunk and Wireshark, I wanted to take the time and learn Redline so I tried to uncover as much I could with it before eventually pivoting to the Splunk logs to finish finding the indicaotrs of compormise. So the first thing I did was open the .mans with Redline and begun conduciting basic traiging such as finding out the Operating System and quickly came accross a potenteial suspicious user named "securityninja". 

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
Opened the imaged computer in Redline<br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/31219591-feca-4c61-9191-08484281b42a" height="80%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
The scenario of the attack and ransomnote, DISCLAIMER: DO NOT attempt to connect to addresses in the ransome note.<br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/cc27e2c5-b362-49fc-8396-3ea4836a75a8" height="50%" width="50%" alt="Ransomware Incident Response"/>
 <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/49dab900-24f2-415c-9a0f-3b28a33ce789" height="50%" width="50%" alt="Ransomware Incident Response"/>
  <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/e6bf0f6a-4eab-4423-9060-abcd1a420d75" height="50%" width="50%" alt="Ransomware Incident Response"/>
<br />
<br />
During my intital general investigation with Redline examining the local user accounts, I noticed a suspicious user "securityninja" with Administrator privliges.<br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/9941f34c-c66a-4b78-ba07-a832d9c0b404" height="80%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
I then looked at the Documents and Desktop for evidence of the ransomware and found what likely was multiple ransomnotes and encrypted files.<br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/885dad2d-154e-4301-a48e-1df286f2f7a0" height="80%" width="80%" alt="Ransomware Incident Response"/>
 <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/1e68881c-2bb7-4a79-b9b5-b7d8bbbcf4f1" height="80%" width="80%" alt="Ransomware Incident Response"/>
 <img src="" height="80%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
After it was clear from the inital triaging ransomware activity occured on the device, so I begeun further exploring the host to find initial access, the ransomware itself ect. Due to me being inexperienced with Redline, the bulk of my investigations revolved around using the timeline tab and scrolling through the thousands of logs looking for anything that exhibits malicious/suspicious behavior and I came accross a webshell accessing the MS exchange server likely showing the inital breech  by the attackers.<br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/6d885b79-96c7-488f-8dd5-74b2bc65e666" height="80%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
I came accross the log where the attacker added the suspicious user "securityninja" likely for persistance after the attack.<br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/2a4e36eb-b136-4562-8e57-276bea7184f6" height="80%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
Further down in the logs, I came accross a file creation that seemed suspicous as it was cmd.exe within the Documents folder and upon further analysis this was confirmed to be the ransomware. I obtained the hash in Redline and 62 vendors on VirtusTotal confirmed the file to be Conti ransomware.<br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/06a77c20-da11-4f9e-aa77-f2e1867b4638" height="50%" width="50%" alt="Ransomware Incident Response"/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/4e8265e1-c59a-44b7-b665-3fa921466282" height="50%" width="50%" alt="Ransomware Incident Response"/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/46dbbee8-f6bb-491a-834c-2f780305d687" height="50%" width="50%" alt="Ransomware Incident Response"/>
<br />
<br />
I had already discovered the file extension of the encypted files early in my investigation when looking at the User's desktop and docuemnts so I filterd for it and discovered that well before the bulk encryption of the host and exhange server, the attackers targeted different log instances both on the host and the exchange server<br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/4da249db-89e7-4692-93cf-2c9d55edd0bb" height="50%" width="50%" alt="Ransomware Incident Response"/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/04e046e3-bed6-4fd6-b2fc-b4357981687d" height="50%" width="50%" alt="Ransomware Incident Response"/>
<img src="(https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/c355f325-73a7-404f-b843-9bc1c83a55f4" height="50%" width="50%" alt="Ransomware Incident Response"/>
<br />
<br />
This behavior was also seen with the ransomnotes "readme.txt" files, as they first appeared on the exchange server well before the being created en-mass on the host machine <br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/6cbcefc9-a92f-4d23-be86-7b000fe7132b" height="50%" width="50%" alt="Ransomware Incident Response"/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/c9fa2b84-585b-4e2d-86f4-b14c82957235" height="50%" width="50%" alt="Ransomware Incident Response"/>
<br />
<br />
As I was running out of options in Redline I decided to switch to the packet capture in Wireshark to see if I can find any further evidence of inital access or Indicators of Compromise. However, the bulk of the traffic captured by Wireshark was encrypted LDAP traffic so I did not find much useful inforamtion other than confirming the majority of traffic was between private IP on the same network but public IP addresses I found communicating to the host to downloaded further malware from were had malcious realtions.<br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/892dd3ba-948b-42fe-82f8-44046f048781" height="50%" width="50%" alt="Ransomware Incident Response"/>
 <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/56b15a44-8e6a-4fae-9da3-bfc2a80927b2" height="50%" width="50%" alt="Ransomware Incident Response"/>
 <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/fd0749ec-7cd9-4256-81a2-a70482208a1f" height="50%" width="50%" alt="Ransomware Incident Response"/>
 <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/7393942e-30bc-44fc-84d4-68dc9a101106" height="50%" width="50%" alt="Ransomware Incident Response"/>
 <img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/140e8a7d-51c6-4425-9314-61a64392366d" height="50%" width="50%" alt="Ransomware Incident Response"/>
<br />
<br />
Once again I was running into deadends so I decided to try Nwtwork Miner which is also a packet analysis tool. I had to first convert the pcapng file to a standard pcap file and then opened it in Network Miner. Within Netork Miner I could onyl find oen useful piece of inforamtion which was the inittial connection the threat actors made the exchange server which helps build the timeline of the attack <br/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/1745c454-dc95-4c11-9eb0-5ce21dec9e60" height="50%" width="50%" alt="Ransomware Incident Response"/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/5d50f619-761d-43c5-bac6-18ef6fca6bf3" height="50%" width="50%" alt="Ransomware Incident Response"/>
<img src="https://github.com/KirkDJohnson/Ransomware-Incident-Response-Lab/assets/164972007/feb67f0b-6d0b-4dbe-a4e4-aff38cd8bd3b" height="50%" width="50%" alt="Ransomware Incident Response"/>
<br />
<br />
Text<br/>
<img src="" height="80%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
Text<br/>
<img src="" height="80%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
Text<br/>
<img src="" height="80%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
Text<br/>
<img src="" height="80%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
Text<br/>
<img src="" height="80%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />
Text<br/>
<img src="" height="80%" width="80%" alt="Ransomware Incident Response"/>
<br />
<br />




<h2>Thoughts</h2>
text
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
