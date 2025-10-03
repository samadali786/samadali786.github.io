---

title: "Traffic Analysis of Malicious file downloaded from Fake Software Site"
date: "2025-06-18 00:00:00"
categories: [Network Packet Analysis, Traffic Analysis of Malicious file downloaded from Fake Software Site]
tags: [Malware Analysis]

---


![googauthapp](assets/img/maltraffic1/google_authen_app.png)


# Overview Of Incident

In this network traffic analysis activity, I analyze the "2025-01-22-traffic-analysis-exercise.pcap.zip" PCAP file from [MalwareTrafficAnalysis](https://malware-traffic-analysis.net/2025/01/22/index.html 'MalwareTrafficAnalysis.net') in Wireshark. In this activity, the suspicious file was downloaded by a coworker after searching for the Google Authenticator app from the fake software site. The goal of this activity is to find the malicious behavior, infected client (IP address, MAC address, user account name), domain name of the fake Google Authenticator app, and the IP addresses of C2 servers in the given PCAP file.


# LAN Segment Details Given From The PCAP File

**1) LAN segment range: 10.1.17.0/24 (10.1.17.0 through 10.1.17.255).**<br>
**2) Domain: bluemoontuesday.com.**<br>
**3) Active Directory (AD) domain controller: 10.1.17.2 - WIN-GSH54QLW48D.**<br>
**4) AD environment name: BLUEMOONTUESDAY.**<br>
**5) LAN segment gateway: 10.1.17.1.**<br>
**6) LAN segment broadcast address: 10.1.17.255.**<br>


# Steps taken to find malicious behaviour in the PCAP file

# 1. Finding the largest HTTP Object in the PCAP File

   The malicious file had already been downloaded by the coworker, so I started to first find the largest HTTP objects within the PCAP file. 
   Below is the list of the large suspicious HTTP objects in the PCAP file, with the largest one highlighted, and the packet from which the 
   object is downloaded.


  
  List of all HTTP Object packets with the largest one highlighted:
  ![largepacsus](assets/img/maltraffic1/larg_http_obj_with_sus.png)


  The Packet from which the large HTTP packet was downloaded:
  ![largepac](assets/img/maltraffic1/larg_pack_object.png)
  
# 2. Analyzing the HTTP stream of the large HTTP Object Packet for accessing the resource

The client was trying to access the resource "GET /1517096937 HTTP/1.1" from the host "Host: 5.252.153.241" but the server responded with an "404 Not Found" with a few requests from the client, but later accepted the client request with "200 OK" response by providing an "application/octet-stream" file to download that could be a fake Google Authenticator app. Further, following the HTTP stream server not responding to the client request with "404 Not Found", it could be possible to hide from the detection.


HTTP Stream of the large suspicious packet:
![httpstream](assets/img/maltraffic1/HTTP_stream_of_access.png)


# 3. Analyzing the Malicious PowerShell Script found in the HTTP stream of the large HTTP Object Packet

The HTTP data stream in the packet contains a PowerShell script. The script consists of different functions which are used to download a malicious file to a specific directories, and create the startup shortcut for a downloaded file. The script is also used to establish a C2 connection to communicate with the C2 server for persistence. The following are the functions that are used to perform the malicious tasks.

# 3a. Function "Download-Files" from the Malicious Poweshell Script
 
 This function takes three parameters "$panelIP" which is used for a server IP address, "$files" to iterate over the metadata of each file, and "$filesDir" for saving the files that are downloaded following are the steps that were used in this function.

   1. It first checks if the directory exists, and if it's not, it creates the directory, and if the creation fails it returns an error message.

   2. It then iterates over the list of files using the "$files" parameter, and extracts each file's metadata(URL and name) to download each file in a specific   
      directory it uses the "System.Net.WebClient" object from the script.

   3. Error handling is also used for checking the directory creation and file downloading with the error message "Error while download file" with the file name,   
      link and the error exception.

   4. If the function succeeds, it returns a "'status'='success'" message.


   Malicious PowerShell script function "Dowload-Files" present in the large HTTP suspicious packet:
   ![fundown](assets/img/maltraffic1/fun_download_file.png)

# 3b. Function "Invoke-Startup" From the Malicious PowerShell Sript 

 This function is used to maintain persistence on the client system by creating a startup shortcut for the malicious downloaded file. The following are the steps that were used by this function..
 
  1. This function first calls the previous function, the "Download-Files" to download the file from the server, and if it's not able to download, the function  
     returns with the error message "Error while creating shortcut.".
      
  2. There is an additional parameter "$startupFileName" that is used to take the name of the file, that is used in the "$startupFilePath" function, which is  
     hardcoded in the script as "$startupFilePath = "C:\ProgramData\huo\TeamViewer.exe".

  3. The script also creates the shortcut by using "Create-Shortcut" function, which calls inside this function.

  4. The hardcoaded file path "$startupFilePath = "C:\ProgramData\huo\TeamViewer.exe" shows that the script uses the legitimate software "TeamViewer.exe" to  
     disguise itself.
                    
  Malicious PowerShell script function "Invoke-Startup" present in the large HTTP suspicious packet:
  ![funinvo](assets/img/maltraffic1/fun_invo-startup.png)

# 3c. Functions "Send-Log" and "ConvertTo-StringData" From the Malicious PowerShell Script 

   These functions are used to perform malicious behavior by ensuring persistence, converting the execution results into the string format, and then sending them back to the C2 remote server. Following are the steps that were used in this function.

  1. The function "Send-Log" uses the "$result" parameter that is used to get the log result and then upload it to the URL using "$uploadUrl" and then using the 
     "System.Net.WebClinet" to download the files.

  2. The function "ConvertTo-StringData" uses the "$hashTable" parameter to iterate over each "$item" as a key value pair to convert it into string format.

  3. The script further defines the files that include "TeamViwer.exe", ".dll's" files, and PowerShell script that is downloaded from the Server "5.252.153.241"  
     that could be an attacker C2 IP address, and stored it into the directory that it creates in the "Download-files" function "C:\ProgramData\huo".

  4. Finally, the script converts the execution results from the client system and sends them back to the suspected C2 IP address "5.252.153.241" in an HTTP GET 
     request, which shows that the script is used to maintain persistence and control over the infected client system.    

   Malicious PowerShell script function "Send-Log" and "ConvertTo-StringData present in the large HTTP suspicious packet:
   ![funsenlog](assets/img/maltraffic1/send_log.png)
   


After analyzing the malicious behavior and discovering the malicious application and PowerShell script inside the packet of the large HTTP Object, In the next steps of my analysis I am trying to find out the infected client (IP address, MAC address, user account name), domain name of the fake Google Authenticator app, and the IP addresses of C2 servers. 


# Finding the infected client IP address, MAC address, and User account name from PCAP file

To find the client IP address, I used the earlier steps for finding the large HTTP Object first with the large size to determine the infected client IP address that downloaded the Authenticator app.


IP address of the infected client in a PCAP file: 
![clidesip](assets/img/maltraffic1/client_ip_addr.png)


To find the MAC address of the infected client, I used the same previous steps that I used to find the client IP address.


MAC address of the infected client in a PCAP file: 
![climacip](assets/img/maltraffic1/Mac_addr_client.png)


From the given LAN segment data it shows that the client was using Active Directory with the domain controller IP address of "10.1.17.2.
by using the following querry in display filter "ip.addr == 10.1.17.215 & 10.1.17.2 && kerberos" which are use to filter the traffic between client and domain controller of the Active Directory, and further filter it by using LDAP and Kerberos protocols that are use 
in the Active Directory environment.


The Kerberos authentication packet entry from client Active directory domain controller and the user account name of the client:
![useaccname](assets/img/maltraffic1/user_acc_name.png)

# Finding the domain of the fake Googel Authenticator app from the PCAP file

The domain name for the fake Google Authenticator app to which the client initially connected, and is using typosquatting to resolve the 
domain name to "authenticatoor.org".


The domain name of the fake Google Authenticator app:
![gooauthdom](assets/img/maltraffic1/goo_auth_dom.png)


# Finding the IP addresses of the C2 servers from the PCAP file

After analyzing the network traffic from the PCAP file, I found two IP addresses that are suspicious and are likely to be used by the attackers for the C2 servers.

 1. C2 IP address "5.25.153.241"
    
    This IP address is used to download the malicious file, maintain persistence, and infect the client system by using malicious PowerShell script as a payload.

 2. C2 IP address "82.221.136.26"

    This IP address is the initial IP address to which the client is connected first, which uses the "authenticatoor.org" domain name, which uses typosquatting to re-direct the client to the attacker's C2 IP address of "5.35.153.241".   

# Conclusion

  This activity helps me to understand how to filter and analyze the large HTTP Object packets in the PCAP file using Wireshark, and how to analyze the malicious PowerShell script as a payload in the large HTTP packet, which is further used to download the malicious file to infect the client system and to maintain presistence by establishing the C2 connections with the attacker IP addresses.  
 





















