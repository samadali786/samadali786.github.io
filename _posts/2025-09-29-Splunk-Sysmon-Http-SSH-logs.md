---

title: "How to Install Sysmon and Ingest Sysmon Logs to Splunk and How to Perform HTTP and SSH Log Analysis in Splunk"
date: "2025-09-29 00:00:00"
categories: [Log Analysis, How to Install Sysmon and Ingest Sysmon Logs to Splunk and How to Perform HTTP and SSH Log Analysis in Splunk]
tags: [Log Analysis]

---

# Overview

In this log analysis activity, I downloaded and installed Sysmon driver using PowerShell and then using Microsoft Event Viewer tools to convert Sysmon logs into ".evtx" format and then ingest the logs in Splunk Enterprise in Windows 10 VM. In the second part of this activity, I performed HTTP and SSH Log Analysis tasks by filtering and extracting the useful information from the logs using SPL (Search Processing Language) queries and Intresting fileds in Splunk. 

# Installing Sysmon Driver and Sysmon config file in Windows 10 VM

I first downloaded the latest Sysmon driver version (v15.15) from Microsoft Sysinternals Sysmon, then I extracted the "Sysmon.zip" folder which consists of "Sysmon.exe", "Sysmon64.exe", and "Sysmon64a.exe" executable files. I used "Sysmon64.exe" which is a 64-bit version that is compatible with my Windows 10 VM, to install the Sysmon driver using Microsoft PowerShell tool by using this command "./Sysmon64.exe -i". After installing the Sysmon driver, I downloaded the "sysmonconfig-export.xml" file from GitHub "https://github.com/SwiftOnSecurity/sysmon-config" that is used to capture high signal events and make the logs more manageable for analysis. To install this config file in Splunk, I used this command " ./sysmon64.exe -c sysmonconfig-export.xml" in Microsoft PowerShell tool.

Downloaded the Sysmon driver from Microsoft Sysinternals:
![down_sysdriv](assets/img/SplunkLogAnalysis/down_sysmo_Mic.png)

Installing the Sysmon driver from the Microsoft PowerShell tool:
![inst_sysdriv](assets/img/SplunkLogAnalysis/inst_sys_pow.png)

Installing the sysmonconfig-export.xml config file in Sysmon using Microsoft PowerShell tool:
![inst_sysconf](assets/img/SplunkLogAnalysis/inst_sys_conf_file.png)

# Converting Sysmon logs to .evtx fromat from Microsoft Event Viewer

In this step, I opened up the Microsoft Event Viewer from the console tree. I expanded "Applications and Service Logs", then "Microsoft", "Windows", and then opened the "Sysmon" folder. Inside the folder, there was a file named "Operational" which consisted of all the logs that were captured by Sysmon. I right-clicked that file and selected "Save All Evnet As" and then gave the name and selected the 'Save as' type" as ".evtx" and saved that Sysmon log file on my Windows 10 VM desktop.

From Microsoft Event Viewer Application and Service, Microsoft, and Windows: 
![evtv_mswin](assets/img/SplunkLogAnalysis/Event_view_app_mic_wind_1.png)

From Microsoft Windows, and Sysmon folder:
![evtx_winsys](assets/img/SplunkLogAnalysis/save_sys_log_2.png)

Converting Sysmon "Operational" logs into .evtx format:
![evtx_sysoper](assets/img/SplunkLogAnalysis/save_sys_log_evtx_3.png)

# Ingesting Sysmon Logs into Splunk Enterprise using the Upload option

In this step, I ingest the Sysmon Logs to Splunk Enterprise by using the upload option from Splunk. First I logged to Splunk Enterprise, then, from the settings tab, I selected "Add Data" and then selected the option "Upload" and then uploaded the Sysmon log file, "Sysmon.evtx" file that I saved from Microsoft Event Viewer. After that, I set the source type to binary and then select next for the "input settings" then review, done and "Start Searching". Sysmon logs are now successfully ingested into Splunk for log analysis.   

Splunk "Add Data" option from the Settings tab in Splunk:
![add_data](assets/img/SplunkLogAnalysis/Add_data_spl.png)

Select the "Uplaod" option in Splunk:
![upl_data](assets/img/SplunkLogAnalysis/upload_data_1.png)

Upload the "sysmon.evtx" file to Splunk:
![upl_syslog](assets/img/SplunkLogAnalysis/upload_sys_log_evtx.png)

Set the "Source type" to binary in Splunk:
![set_srcbin](assets/img/SplunkLogAnalysis/set_src_bin.png)

Select the "Start Searching" option to analyze Sysmon logs in Splunk: 
![strt_srch](assets/img/SplunkLogAnalysis/start_search_sys_log.png)

Successfully ingested the Sysmon logs to Splunk:
![ing_syssplk](assets/img/SplunkLogAnalysis/SPL_serc_fin_sys_evts.png)

# HTTP and SSH Log Analysis in Splunk

In this second part of this activity, I ingested HTTP and SSH logs by using the "Upload" option in Splunk Enterprise, and then performed HTTP and SSH log analysis in Splunk using SPL queries and Interesting fields in Splunk. There are three common methods to ingest logs into Splunk using the "Upload" option to upload the log files from the local machine that I used in this log analysis activity. The "Monitoring" option is use for a limited number of log files using Splunk Indexer, and the "Forwarder" option, which is more commonly use for continuous data ingestion by using a lightweight Splunk agent on the endpoints.  

# 1. HTTP Log Analysis in Splunk

In this part of the log analysis activity, I ingested the HTTP logs "http_logs_analysis.json" in Splunk and then performed the log analysis tasks using SPL queries and an interesting field in Splunk. The HTTP file is in ".json" format, so that I didn't have to pass data manually, and it made it easier for Splunk to create interesting fields from the HTTP logs in ".json" format.

# 1a. Ingesting the HTTP Logs into Splunk using Upload Option:

In this step, I first ingest the "http_logs_analysis.json" file in Splunk by logging into Splunk Enterprise. Then, from the settings tab, I selected "Add Data" and then selected the option "Upload" and then uploaded the "http_logs_analysis.json" file, the same as I did for Sysmon logs in the first part of my log analysis activity. In the next step, I checked for the "Source type", which is in JSON format, and from the input settings, the host field value. I gave the host name "Windows 10 VM" and then reviewed, done and started searching to perform log analysis tasks.

Using Upload option from Splunk to ingest HTTP logs:
![up_httplog2](assets/img/SplunkLogAnalysis/uploadt_http_log_2.png)

Upload "http_logs_analysis.json" file to Splunk: 
![up_httplog3](assets/img/SplunkLogAnalysis/upload_http_log_3.png)

Checked for the Source Type which is in JSON format in Splunk:
![up_httplog4](assets/img/SplunkLogAnalysis/upload_http_log_4.png)

Set the host name 'Window 10 VM" from the Input setting in Splunk:
![up_httplog5](assets/img/SplunkLogAnalysis/upload_http_log_5.png)

After successfully uploading the "http_log_analysis.json file in Splunk then I performed the following tasks. 

# 1b. Find the Top 10 endpoints that are generating the HTTP traffic:

To find the top 10 endpoints that are generating HTTP traffic, I used the "id.orig_h" field that is generated by Splunk from the HTTP logs in my SPL search query with the stats count command and then sorted and set the count to "-" for counting in descending order and then use head command and give the value 10 for counting the top 10 endpoints, below is the SPL query for this task.

```s
   source = "http_logs_analysis.json" sourcetype="_json" | stats count by "id.orig_h" 
   | sort -count | head 10
```

Using "id.orig_h" intresting filed in Splunk:
![q1_top10endfld](assets/img/SplunkLogAnalysis/Quest_1_top10_inters_fiel.png)

SPL query for the top 10 endpoints that are generating the HTTP traffic:
![q1_top10endpts](assets/img/SplunkLogAnalysis/Quest_1_top_10_endpnt.png)

# 1c. Find the number of Server Side errors in the HTTP traffic:

To find the number of server-side errors in the HTTP traffic, I used the "status_code" interesting field generated by Spunk in my SPL search query with the stats count command and set it to "server-side-errors" as a customized interesting field. Below is the SPL query for this task.

```s
   source="http_logs_analysis.json" sourcetype="_json" 
   status_code>=500 status_code<600 | stats count as server_side_errors
```
Using the "status_code" intresting filed in Splunk:
![q2_stacdeint](assets/img/SplunkLogAnalysis/Quest_2_stat_cod_intre_fiel.png)

SPL query for the finding the number of server-side errors in HTTP traffic in Splunk:
![q2_stacde](assets/img/SplunkLogAnalysis/Quest_2_serv_side_err.png)

# 1d. Find the Suspicious User Agents in the HTTP traffic for scripted attacks:

To find the suspicious user-agents in the HTTP traffic, I used the "user_agent" interesting field generated by Splunk in my SPL query. From the "user_agent" interesting field, I found four suspicious user-agents: "sqlmap/1.5.1", "Python-requests/2.25.1", "curl/7.68.0, and "botnet-checker/1.0", so I included these user-agents in my SPL querry by using "IN" clause and then use stats count command on "user-agent" to count the suspicious user agents, below is the SPL querry for this task.

```s
   source="http_logs_analysis.json" sourcetype="json" user_agent IN 
   ("sqlmap/1.5.1", "curl/7.68.0", "python-requests/2.25.1", "botnet-checker/1.0")
   | stats count by user_agent
```
Using the "user_agent" interesting field in Splunk:
![q3_usragtint](assets/img/SplunkLogAnalysis/Quest_3_usr_agt_inter_field.png)

SPL query for finding the Supsicious User Agents in the HTTP traffic in Splunk:
![q3_usragt](assets/img/SplunkLogAnalysis/Quest_3_usr_agt.png)

# 1e. Find the file transfers that are greater than 500kb:

To find the file transfer that was greater than 500kb, I used four interesting fields: "ts" for the time stamp, "resp_body_len" for the length of the response, "id.orig_h" for the number of host endpoints, and "id.resp_h" for the servers, into a table format and then used the sort -resp_body_len command on "resp_body_len" to count the response lenght in desending order, below is the SPL query for this task.

```s
    source="http_logs_analysis.json" sourcetype="json" resp_body_len>5000000 
    | table ts "id.orig_h" "id.resp_h" "resp_body_len" | sort -resp_body_len
```
Using "ts", "id.orig_h", "id.resp_h", and "resp_body_len" are interesting fields in Splunk: 
![q4_filegr500int](assets/img/SplunkLogAnalysis/Quest_4_gre_500_intr_fiel.png)

SPL query for finding file transfers that are greater than 500kb in Splunk:
![q4_filegr500](assets/img/SplunkLogAnalysis/Quest_4_gret_500_resp.png)

# 2. SSH Log Analysis in Splunk:

​In this part of the log analysis activity, I ingested the SSH logs "ssh_logs_analysis.json" in Splunk and then performed the log analysis tasks using SPL queries and an interesting field in Splunk, the same as I did for the HTTP log analysis. The SSH log file is also in ".json" format, so that I didn't have to pass data manually, and it made it easier for Splunk to create interesting fields from the SSH logs in ".json" format.

# 2a. Ingesting the SSH logs into Splunk using the Upload option:

In this step, I ingested the ssh logs in Splunk, same as I did for HTTP logs, by going to the Settings tab, Add Data, upload and then uploading the "ssh_logs_analysis.json" file, then check the source type to json and give the "host filed value" to 'Windows 10 VM" from input settings, and then done, review and start sarching to perform log analysis tasks.

Upload the "ssh_logs_analysis.json" file to Splunk:
![ssh_logup](assets/img/SplunkLogAnalysis/SSH_log_uplaod_1.png)

Select the Source Type "JSON", and Input settings for the host field "Windows 10 VM" for SSH logs in Splunk:
![ssh_logup2](assets/img/SplunkLogAnalysis/SSH_log_upload_2.png)

After successfully uploading the "http_log_analysis.json file in Splunk then I performed the following tasks. 

# 2b. Find the Top 10 endpoints with failed SSH Login attempts:

To find the top 10 endpoints with the failed SSH login attempts, I used the two interesting fields "auth_sucess", and "id.orig_h" that were generated by Splunk from the SSH logs file. I set the value of "auth_sucess" to false, and then used the stats count command on "id.orig_h" to count the endpoints and then used the sort -count command to count in the descending order and then used the head command with the value 10 for the top 10 endpoints, below is the SPL query for this task.

```s
   source="ssh_logs_analysis. json" host="windows 10 VM" sourcetype="_json" auth_success=false
   | stats count by "id.orig_h"
   | sort -count
   | head 10
```
Using the "auth_success" intresting filed in Splunk: 
![ssh_faillogintr](assets/img/SplunkLogAnalysis/Quest_1_failed-SSH_log_Intrest_fiel.png)

SPL query for Top 10 endpoints with failed SSH Login attempts in Splunk:
![ssh_faillogattp](assets/img/SplunkLogAnalysis/Quest_1_failed_SSH_log_attemp_10.png)

# 2c. Find the number of total SSH Connections:

To find the total number of SSH connections, I used a custom interesting field "total_num_SSH_connections" with the stats count command in Splunk, below is the SPL query for this task.

```s
   source="ssh_logs_analysis. json" host="Windows 10 VM" sourcetype="_json" | stats count as
   total_num_SSH_connections
```
SPL query for the total number of SSH connections in Splunk:
![ssh_logup2](assets/img/SplunkLogAnalysis/Quest_2_tot_num_SSH_conect.png)

# 2d. Find the number of all the event types success, failed, no-auth, multiple failed attempts in the SSH logs:

To find the total numbers of all the event types in the SSH log file, I used the "event_type" interesting field with the stats count by command in the SPL query. Below is the SPL query for this task.

Using the "event_type" intresting field in Splunk:
![ssh_allevtintr](assets/img/SplunkLogAnalysis/Quest_3_all_evt_type_intr_fiel.png)

SPL query for the total number of all the four types of events in Splunk:
![ssh_allevtype](assets/img/SplunkLogAnalysis/Quest_3_all_evt_type.png)

# Conclusion:

In this Log analysis activity, I learned how to download the Sysmon driver from Microsoft Sysinternal and then use Microsoft PowerShell tool to install Sysmon with "sysmonconfig-export.xml" config file that is used to capture the high signal events and make the logs more manageable for analysis, then I used Microsoft Event Viewer to convert Sysmon logs into ".evtx" format and then upload the Sysmon logs in Splunk for log analysis. In the second part of this activity, I performed HTTP and SSH log analysis tasks by filtering and extracting the useful information from the logs using SPL search queries and interesting fields in Splunk.