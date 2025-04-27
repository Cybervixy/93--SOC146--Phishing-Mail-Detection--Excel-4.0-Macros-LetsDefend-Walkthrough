# 93- SOC146- Phishing Mail Detection- Excel 4.0 Macros  LetsDefend Walkthrough

![image.png](image.png)

I saw an alert from my SIEM  system, this is an alert for a phishing email and my duty to investigate this email to determine if it is malicious or not. I   could see the severity as high and the **Date as June, 13, 2021 02:13 pm, this timestamp will be instrumental in our investigation.**

![image.png](image%201.png)

![image showing details about the alert](image%202.png)

image showing details about the alert

What rule triggered the alert?  The  presence of a Macros in a Document instead of a VBA  shows  some phishing going on. I also observed the  Server  address  {24}.{213}. {228}.{54}, source ip address as {trenton} {@} [tritowncomputers.com}, finally.  the Endpoint  address is for Lars@ledtsdefend.io.

### With this information ,  I  can begin my  investigation.

1. created a case

![image.png](image%203.png)

2. started a Playbook

![image.png](image%204.png)

3. observed the prompt from the playbook, ensuring i  didn't miss any step as seen below

![image.png](image%205.png)

![From our SIEM Monitoring bar, we see the source address as  follows:](image%202.png)

From our SIEM Monitoring bar, we see the source address as  follows:

SMTP Address :  {24}.{213}.{228}.{54} 
Source Address : trenton{@}tritowncomputers.com   - Sender Address
Destination Address : [lars@letsdefend.io](mailto:lars@letsdefend.io)       - Recipient Address

A quick look up at Mxtoolbox showed discrepancies in the IP. 

 

![image.png](image%206.png)

The Ip address is different from this  {24}.{213}. {228}.{54}

### Is the Email Content Suspicious?

I opened the Email security tool bar and searched for the sender address, i was a able to see the exact timestamp the email was sent Jun, 13, 2021 at 02:11pm by Trenton to Lars.

![image.png](image%207.png)

![image.png](image%208.png)

This confirmed that the email hard an attachments

I copied the url to VirusTotal,  flagged it as Malicious

![image.png](image%209.png)

But i needed more evidence, so i downloaded the file in a sandbox.

![note, you can copy the infected and paste it in the box if you could not type, like i did.](image%2010.png)

note, you can copy the infected and paste it in the box if you could not type, like i did.

After downloading, changed my directory and  i extracted the file hash using the terminal cmd.

![image.png](image%2011.png)

However, virustotal Flagged it as not malicious. 

![image.png](image%2012.png)

Could this be because of the nested compression zip.zip?

I decided to use 7zip a tool know to decompress such type of files

![image.png](image%2013.png)

![image.png](image%2014.png)

I went back to virusTotal to  check this new decompressed file hash. I was right!!!

![image.png](image%2015.png)

Many vendors flagged it as malicious.

next, I had to dive into the file,  unzip the file, cd into the directory and observed the file metadata,

i confirmed the date and time  updated, it targeted  windows OS, and it was an excel sheet containing Macros.

![image.png](image%2016.png)

> File: research-1646684671.xls
> 
> 
> **Type:** Composite Document File V2 (Microsoft Excel)
> 
> **Created:** June 5, 2015, 18:17:20
> 
> **Last Modified:** June 13, 2021, 09:24:55
> 
> **Author:** Amanda
> 
> **Macros:** Excel 4.0 macros detected (3 macros)
> 
> **Security:** No encryption
> 
> **Key Insight:**
> 
> - Likely repurposed for malicious use after 2015.
> - Excel 4.0 macros are commonly abused by attackers for evasion and execution of malicious payloads.

I got the confirmation i needed, next, i went to the endpoint tap, searched for lars, to get more information

![image.png](image%2017.png)

![image.png](image%2018.png)

![image.png](image%2019.png)

Lars indeed , used his computer within that time stamp. i had to contain him immediately

Finally, i have to confirm if lars opened the attachment,  there one place to find out, logs.

From the logs management tab,  i searched the Lars IP address.

![image.png](image%2020.png)

We have a match, clicked the drop down for more details

![image.png](image%2021.png)

There is a source  and destination IP  , which shows an established connection from a command and control server, via the url, which was also seen on virusTotal under the behavior tab.

### Its time for the playbook and the part you have been waiting for.

From the case management tab, lets continue from the case  i have already opened.

![image.png](image%2022.png)

![image.png](image%2023.png)

![image.png](image%2024.png)

![image.png](image%2025.png)

You can see that the device Action Showed  “Allowed” meaning, it delivered.

![image.png](image%2026.png)

![image.png](image%2027.png)

![image.png](image%2028.png)

![image.png](image%2029.png)

I had to add all the artifacts i found during investigation.

Next, i wrote an analyst note to enable anyone, understand my findings.

![image.png](image%2030.png)

![image.png](image%2031.png)

Confirmed the  and then closed the investigation

![image.png](image%2032.png)

![image.png](image%2033.png)

### Yes!!!

![image.png](image%2034.png)

**Conclusion:**

The analysis revealed that the Excel file `research-1646684671.xls` was created with Microsoft Excel in 2015 and last modified in 2021. It contains macros and may be linked to suspicious DLL files (`iroto.dll` and `iroto1.dll`). Several related artifacts, including IP addresses, file hashes, and a malicious URL, were identified.

Feel free to connect with me on LinkedIn: [https://www.linkedin.com/in/victoria-simon-9a4586194/](https://www.linkedin.com/in/victoria-simon-9a4586194/)
