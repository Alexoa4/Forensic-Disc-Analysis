
<h1>Cyber Defense Project 2</h1>
<h2> Project Summary </h2>
<p>In this project, I will be investigating Ramnit Malware. Ramnit was initially detected in early 2010 and later evolved into W32.Ramnit. B, is a worm known for spreading through removable drives. Notably, its operators have continuously upgraded the threat by integrating modules from the leaked source code of the Zeus banking Trojan in May 2011. Presently, Ramnit concentrates on information theft, specifically targeting passwords and online banking login credentials. The malware also installs remote access tools to establish and maintain backdoor connectivity. The Ramnit botnet is estimated to have compromised approximately 350,000 computers worldwide.</p>


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
- Prepare SBAR Report.

<h2>Investigate an Incident Using a SIEM</h2>
<p>We are to identify whether the  email received by AT-USA employee is benign or has malicious intent, then use the SIEM to validate whatever hypotheses we might be able to glean from the threat.</p>
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

<b>Profiling the Log Source:</p></b>

<img src="https://i.imgur.com/TikET9r.png" height="80%" width="80%" alt=""/>

<h2>Hunting down information on Malware using OSNIT</h2>

<b>Getting a bird's-eye view of the malware sample using Virus Total </b><br>

<img src="https://i.imgur.com/tzGy1Ys.png" height="80%" width="80%" alt=""/>


<b> Using a Sandbox (Any.Run) to perform dynamic/hybrid analysis on Ramnit Malware Payload, bilo400.exe </b><br>

<img src="https://i.imgur.com/glNSO83.png" height="80%" width="80%" alt=""/>

<b> Dig in deeper using threat intelligence </b><br>
<p> Once I discovered the type of malware variant I was dealing with, I researched the internet to see what others had written about it. At this time I came across two whitepapers about the Ramnit malware. <br>
For instance, google search provided a 2015 whitepaper from Symantec as well as a late 2017 reverse engineering write-up from CERT.PL. as linked below:</p>
 <a href=“https://cert.pl/en/posts/2017/09/ramnit-in-depth-analysis/”>CERT.PL</a>
 <a href=“https://informationsecurity.report/Resources/Whitepapers/b201d876-c5df-486d-975e-2dc08eb85f02_W32.Ramnit%20analysis.pdf”>Symantec Whitepaper</a> 

 
<img src="https://i.imgur.com/GWWwSDO.png" height="80%" width="80%" alt=""/>
<img src="https://i.imgur.com/QjVXq3U.png" height="80%" width="80%" alt=""/>
<img src="https://i.imgur.com/XJEZSq9.png" height="80%" width="80%" alt=""/>




<h2>Examine a Compromised Host's Memory with Volatility and Atom</h3>

<img src="https://i.imgur.com/6rvtSdZ.png" height="80%" width="80%" alt=""/>

 <b>Investigate Process (with pslist, psscan, and pstree) <b/><br>
 
<img src="https://i.imgur.com/yydvbwp.png" height="80%" width="80%" alt=""/>

<b>Inspect Network Activity (with netscan) </b><br>

<img src="" height="80%" width="80%" alt=""/>

<b>Identify Ramnit Injected DLLs: </b><br>

There are three injected DLLs involved with Ramnit: rmnsoft.dll, modules.dll, and hooker.dll.


<h2>Conduct a Forensic Disk Examination with autopsy</h2>


 Getting Started with Autopsy: Explore the Filesystem <br>


<img src="https://i.imgur.com/DGOe5OY.png" height="80%" width="80%" alt=""/>


Getting  started with Registry Explorer: Extracting Hives in Temp Folder <br>


<img src="https://i.imgur.com/I01xPoW.png" height="80%" width="80%" alt=""/>





<h2>Tools and Technologies Used</h2>
- Volatility and Atom <br>
- Splunk <br>
- Autopsy<br>
- Registry Explorer
- Amazon WorkSpaces as Virtual Machine 
