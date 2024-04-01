
<h1>Cyber Defense Project 2</h1>
<h2> Project Summary </h2>
<p>In this project, I will be investigating Ramnit Malware. Ramnit was initially detected in early 2010 and later evolved into W32.Ramnit. B is a worm known for spreading through removable drives. Notably, its operators have continuously upgraded the threat by integrating modules from the leaked source code of the Zeus banking Trojan in May 2011. Presently, Ramnit concentrates on information theft, specifically targeting passwords and online banking login credentials. The malware also installs remote access tools to establish and maintain backdoor connectivity. The Ramnit botnet is estimated to have compromised approximately 350,000 computers worldwide.</p>


<h3>Project Scenario</h3>
Re: Closing incident reports
- I noticed that you were just handed the job of closing out a bunch of old incident reports, and I think this might be another chance for you to get familiar with some of the tasks the senior members of the team do daily.
- Take a look at this interesting report from an investigation that Boots, another analyst on our team, led at the end of 2017. That investigation was centered around a strange email that the CFO of AT-USA received that he flagged as suspicious. Even though that email did seem a bit unusual, the URL linked to a legitimate website that is frequently visited by IT staff at AT-USA and there was no email attachment, so after a short investigation, Boots determined that it was a false alarm.

I think this is a good opportunity for you to take a pretty straightforward case and follow in Boots's footsteps to try and replicate his findings. You should still have access to Splunk, so take a look at the details and see what you can discover. Even though this incident has a resolution, try to treat it like you do not know the outcome. You never know if you will end up noticing something that others overlooked!

I've attached the SBAR report that Boots submitted regarding the incident below. Take a look at his findings as well as the originally received e-mail to begin your investigation.

 



<h2>Project Roadmap</h2>
- Investigate an Incident Using a SIEM <br>
- Hunting down information on Malware using OSNIT <br>
- Examine a Compromised Host's Memory <br>
- Conduct a Forensic Disk Examination <br>
- Write a comprehensive SBAR Report.

<h2>Investigate an Incident Using a SIEM</h2>
<p>We are to identify whether the  email received by an AT-USA employee is benign or has malicious intent, then use the SIEM to validate whatever hypotheses we might be able to glean from the threat.</p>
<b><p>Starter Pivots: </p></b> <br>
- TimeStamps <br>
- Email Address <br>
- Internal Domain <br>
- Username formatting <br> 
- URLs <br>
- IP Address.

<b>Initial Access Method:</p></b>
Since the Email has no attachments, & no sneaky malicious URLs—it appears that the methods of attack are limited to web-based threats like forms of drive-by compromise that target a user's browser.<br>
Also since only one foreign domain is cited in the email, we search for all web traffic in the SIEM associated with the www[.]ciso[.]guide domain. <br>

<b><h2>Profiling the Log Source:</h2></p></b>
![Firewall](https://github.com/Alexoa4/Forensic-Disc-Analysis/assets/105945708/21a9848f-330a-425f-8433-f264766358bb) <br>

<h2>Hunting down information on Malware using OSNIT</h2>

<b>Getting a bird's-eye view of the malware sample using Virus Total </b><br>

![VT](https://github.com/Alexoa4/Forensic-Disc-Analysis/assets/105945708/fb461a6c-95c9-449d-9230-a9372b96056e)


<b> <h2>Using a Sandbox (Any.Run) to perform dynamic/hybrid analysis on Ramnit Malware Payload, bilo400.exe </h2></b><br>


![any run](https://github.com/Alexoa4/Forensic-Disc-Analysis/assets/105945708/2dc5d425-0286-41b8-a105-cff692d3a414)<br>

<b> <h3>Review the Ramnit IOCs from ANY.RUN sandbox</h3><br>
<p>In many instances Suspicious network activity can reveal a lot about a malware attack. Suspicious source IPs and suspicious destination IPs are easy to identify with external research. At this juncture, we start our investigation with an IP address IOC—the most suspicious IP address found during the original Splunk investigation was 194.87.109.183.

Starting with the memory image and the netscan memory module, we observed a connection from IP 194.87.109.183 to Daniel Pc. Also, the review of the ANY.RUN sandbox page showed there was a process after a reboot:</p>

<b> Digging deeper using threat intelligence </b><br>
<p> Once I discovered the type of malware variant I was dealing with, I researched the internet to see what others had written about it. At this time I came across two whitepapers about the Ramnit malware. <br>
For instance, google search provided a 2015 whitepaper from Symantec as well as a late 2017 reverse engineering write-up from CERT.PL. as linked below:</p> <br>

<b> <h2>The links below lead to the two whitepapers on Ramnit Malware by Symantec and Mitchat Praszmo</h2>
 -[Cert.PL](https://cert.pl/en/posts/2017/09/ramnit-in-depth-analysis/). <br>
 -[Symantec Whitepaper](https://informationsecurity.report/Resources/Whitepapers/b201d876-c5df-486d-975e-2dc08eb85f02_W32.Ramnit%20analysis.pdf)<br>

 <h3>Excerpts of the whitepapers are shown below:</h3> <br>

 ![CERT](https://github.com/Alexoa4/Forensic-Disc-Analysis/assets/105945708/316fad52-f267-4e0e-b236-671feee0083b)<br>
 
![F1](https://github.com/Alexoa4/Forensic-Disc-Analysis/assets/105945708/ed6a897b-3d6a-43bc-8039-874c8b267716)






<h2>Examine a Compromised Host's Memory with Volatility and Atom</h2><br>

<h2>Ramnit Malware Footprint</h2>

 <b>Investigate Process (with pslist, psscan, and pstree) <b/><br>
 <p>In this step, I walked through an investigation of the memory images that focus on suspicious processes. I started by examining the Process list (pslist) which is usually the first place to look carefully after verifying the image profile. Once I  ran pslist, I checked the svchost.exe process since it is a popular hiding place for malware due to how easy it is to secretly add an extra svchost.exe or two without raising eyebrows. Also since svchost.exe has only one legitimate parent: services.exe (PID 500), I looked for a different parent process for svchost.exe. Our memory image shows two svchost.exe (PIDs 4104, 2612) with a PPID that is NOT services.exe (PID 500): At this point, I had to confirm the two rogue svchost.exe's mysterious parent process, (PID 4652) from the SIEM</p> <br>
 
 ![pstree](https://github.com/Alexoa4/Forensic-Disc-Analysis/assets/105945708/51e5e442-cc5e-4a4d-8ee9-7f42c275af95)
![evidence](https://github.com/Alexoa4/Forensic-Disc-Analysis/assets/105945708/26871396-320c-4b8e-a307-859d88b1c134)

<h3>index=sysmon Computer="Daniel-PC" ProcessId=4652</h3>
<p>At last, I can connect the Unknown process (PID 4652) to a familiar IOC: obommhdf.exe (PID 3764) by looking at its parent_process and ParentProcessId as shown in the images below.</p>

![Splunk logs](https://github.com/Alexoa4/Forensic-Disc-Analysis/assets/105945708/aedbfae0-4929-43e3-8579-1d9b8da0ee44)
![sysmon log](https://github.com/Alexoa4/Forensic-Disc-Analysis/assets/105945708/854d68a0-9051-48e1-93ce-23757e4456d3)

<p>In conclusion, by pivoting between the two log sources, we can now construct what happened:
obommhdf.exe (PID 3764) spawned a second instance of the Ramnit executable: xwgrttjl.exe (PID 4652).
xwgrttjl.exe (PID 4652) then spawned the two rogue processes without the PPID of services.exe (PID 500)!) svchost.exe's: svchost.exe (PID 4104) and svchost.exe (PID 2612).
xwgrttjl.exe (PID 4652) then exited the process list, having served its function.
obommhdf.exe (PID 3764), svchost.exe (PID 4104), and svchost.exe (PID 2612) remain running.</p>



<h2>What just happened?.... In Summary </h2>
<p>1. I began this investigation in a memory module where we picked up new IOCs: two rogue svchost.exe processes and a mysterious parent process (PPID) that was done and gone by the time we captured our memory image. 

2. I then moved into Splunk to corroborate those new IOCs. Once inside Splunk, I found our new IOCs and the file name for the mysterious parent process. I also found the PPID for our mysterious parent process in Splunk.

3. Going back to Volatility, I connected all this new information back to an old IOC from our original Splunk investigation (obommhdf.exe).

By going back and forth between Volatility and Splunk, I can corroborate old IOCs, discover new IOCs, corroborate familiar connections between IOCs, and discover new connections between IOCs. </p>




<h2>Tools and Technologies Used</h2>
- Volatility and Atom <br>
- Splunk <br>
- Autopsy<br>
- Registry Explorer
- Amazon WorkSpaces as Virtual Machine 
