---

title: "Let's Defend SOC342 - CVE-2025-53770 SharePoint ToolShell Auth Bypass and RCE"
date: "2025-08-04 00:00:00"
categories: [Let's Defend Alerts, SOC Alert 324 Event ID 320 SharePoint ToolShell Auth Bypass and RCE]
tags: [Let's Defend Alerts]

---

# Overview of Alert

In this Let's Defend critical alert, "SharePoint01" server with the IP address of "172.16.20.17" has a critical zero-day vulnerability that allowed unauthorized attacker to send the POST request with the Source IP address of "107.191.58.76", which further exploit and target the "ToolPane.aspx" file in "SharePoint01" server with the large payload size which contains the malicious file "spinstall0.aspx" which then execute the malicious script using PowerShell tool. This vulnerability in the Microsoft SharePoint server was also reported as "CVE-2025-53770" on the NIST NVD (National Vulnerability Database) website.

Let's Defend SOC 342 Event ID 320 SharePoint ToolShell Auth Bypass and RCE:
![soc324alrt](assets/img/letsdefendSOC342/SOC_alert_342_event_id_320.png)


# Analyzing the Logs from Log Management Tab to check if a similar POST request was sent.

In this step, I first checked the logs from the Log Management tab in Let's Defend SIEM dashboard to check if the same POST request was sent to the other servers. I found that there was only a single event which shows that a POST request was received on the "SharePoint01" server with the destination IP address of "172.16.20.17" from the source IP address "107.191.58.76".

Analyzing the logs from the Log Management tab on Let's Defend SIEM dashboard:
![logmana](assets/img/letsdefendSOC342/mal_post_log_manag.png)


# Verifying the Source IP address of the Sender sending the POST request

To verify the Source IP address "107.191.58.76" of the sender, I used VirusTotal to first verify and check if the source IP address of the sender sending the POST request "/_layouts/15/ToolPane.aspx?DisplayMode=Edit&a=/ToolPane.aspx" to the destination IP address of "172.16.20.17" which belongs to "SharePoint01" on Let's defend was malicious or not. After analyzing the Source IP address, the IP address was malicious and used to exploit the vulnerability in Microsoft SharePoint on-premise servers.

Malicious Source IP address in VirusTotal:
![srcipmal](assets/img/letsdefendSOC342/mal_src_ip.png)


# Analyzing the EndPoint security of "SharePoint01" server on Let's Defend SIEM dashboard

In this step, from the EndPoint Security tab on Let's Defend, I analyzed the "SharePoint01" server Processes, Network Activity, Terminal history, and Browser History logs. From the Process logs, I found three suspicious processes, "powershell.exe", "csc.exe", and "cmd.exe" that were used to execute the malicious file "spinstall0.aspx" with the MD5 hash "92bb4ddb98eeaf11fc15bb32e71d0a63256a0ed826a03ba293ce3a8bf057a514" which contains the malicious script, that targeting "ToolPane.aspx" and exploit the vulnerability in "SharePoint01" server.  


Suspicious "powershell.exe" process found in Endpoint Security tab on Let's Defend SIEM dashboard:
![powshellproc](assets/img/letsdefendSOC342/Pow_shell_proc.png)

Suspicious "csc.exe" process found in Endpoint Security tab on Let's Defend SIEM dashboard:
![cscproc](assets/img/letsdefendSOC342/CSC_process.png)

Suspicious "cmd.exe" process found in Endpoint Security tab on Let's Defend SIEM dashboard:
![cmdproc](assets/img/letsdefendSOC342/cmd_proce_md5.png)

After further analyzing the cmd.exe process which contains the MD5 hash of the file, I then checked the MD5 hash of the file on VirusTotal, and confirmed that the file was malicious, and is used to exploit the vulnerability on the Microsoft SharePoint server with the file name "spinstall0.aspx" which contains the malicious script, and is also similar to the file name that I found on this process. 

Malicious "spinstall0.aspx" file that used to target "ToolPane.aspx" to exploit vulnerability in "SharePoint01" server:
![malfile](assets/img/letsdefendSOC342/mal_aspx_file_exe.png)


# Analyzing "CVE-2025-53770" related to Microsoft SharePoint Server

In this step, I analyzed the "CVE-2025-53770" on NIST NVD(National Vulnerability Database) and found that this vulnerability is critical with a score of "9.8", which is used to exploit the vulnerability in on-site Microsoft SharePoint servers that allows the unauthorized attacker to perform remote code execution over a network.

Analyzing "CVE-2025-53770" from NIST NVD(National Vulnerability Database) website:
![cvescore](assets/img/letsdefendSOC342/CVE_score.png)


# Adding the artifacts and notes related to this Alert to complete the Playbook Process:

In this final step, I gathered all the artifacts and added them to the Artifacts findings in the Let's Defend playbook section for documentation and finally added the note and closed the alert to complete the alert process. I was successfully able to complete this alert by understanding and answering each question correctly.

Adding Artifacts that I found from this alert:
![artfacts](assets/img/letsdefendSOC342/findings.png)

Successfully completed the alert process:
![alrtcompl](assets/img/letsdefendSOC342/succ_comp_alert.png)









