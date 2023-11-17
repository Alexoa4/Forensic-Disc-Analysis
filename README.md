
<h1>Cyber Defense Project 2</h1>


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
<b></b><p>Starter Pivots: </p> <br>
- TimeStamps <br>
- Email Address <br>
- Internal Domain <br>
- Username formatting <br> 
- URLs <br>
- IP Address.
<p>Initial Access Method:</p>
- Since the Email has no attachments, & no sneaky malicious URLs—it appears that the methods of attack are limited to web-based threats like forms of drive-by compromise that target a user's browser.
- Since only one foreign domain is cited in the email, we search for all web traffic in the SIEM associated with the www[.]ciso[.]guide domain. <br>

<p>Profling the Log Source: </p>

<img src="" height="80%" width="80%" alt=""/>

<h2>Hunting down information on Malware using OSNIT</h2>

Getting a bird's-eye view of the malware sample using Virus Total <br>
<img src="" height="80%" width="80%" alt=""/>

 Use a Sandbox to perform hybrid analysis <br>
<img src="" height="80%" width="80%" alt=""/>

 Dig in deeper using threat intelligence <br>
<img src="" height="80%" width="80%" alt=""/>

Applying OSINT to the investigation <br>
<img src="" height="80%" width="80%" alt=""/>




<h2>Examine a Compromised Host's Memory with Volatility and Atom</h3>

 Investigate Process (with pslist, psscan, and pstree) <br>
<img src="" height="80%" width="80%" alt=""/>

inspect Network Activity (with netscan) <br>
<img src="" height="80%" width="80%" alt=""/>

Identify Ramnit Injected DLLs: <br>

There are three injected DLLs involved with Ramnit: rmnsoft.dll, modules.dll, and hooker.dll.


<h2>Conduct a Forensic Disk Examination with autopsy</h2>
- Get started with Autopsy <br>
- Explore the Filesystem <br>
- Investigate within Autopsy <br>
-Extract and Analyze Registry Hiv using Registry Explorer.

<h2>Tools and Technologies Used</h2>
- Volatility and Atom <br>
- Splunk <br>
- Autopsy<br>
- Registry Explorer
- Amazon WorkSpaces as Virtual Machine 
