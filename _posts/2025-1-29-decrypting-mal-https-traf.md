---

title: "How to decrypt and analyze malicious HTTPS traffic in Wireshark"
date: "2025-1-29 00:00:00"
categories: [Malware Analysis, How to Decrypt and analyze malicious HTTPS traffic in Wireshark]
tags: [Malware Analysis]

---

# Overview

Decrypting and analyzing the malicious HTTPS traffic is one of the role that is performed by the Blue team in Cybersecurity to find the malicious activity on the network, it can be done by using GUI network packet analyzing tool like Wireshark or any other network packet analyzer tools. HTTPS network traffic is encrypted by SSL and TLS encryption, but sometimes it could be intercepted by the attacker as MITM (Men in the Middle) attacks, these types of attacks are hard to detect, but with the help of tool like Wireshark we can able to decrypt the malicious HTTPS network traffic and find the malicious content like files, attacker source IP address, and C2 Server information.

In this project I use already captured malicious (Malicious_HTTPS_SSL-TLS.pcap) packet capture file and (Key_Log_file.txt) which consist of SSL enctyption keys that is use for decrypting the malicious traffic in Malicious_HTTPS_SSL_TLS.pcap file for analysis, and extract the malicious file all using Wireshark tool, and then use VirusTotal to detect the malicious file content from the decrypted packet, below are the items and tools that I used to complete this project.

 **1) Malicious_HTTPS_SSL-TLS.pcap.**<br>
 **2) Key_Log_file.txt.**<br>
 **3) Wireshark tool.**<br>
 **4) VirusTotal.**<br>


# What is Wireshark?

 Wireshark is a open-souce GUI network packet analyzer tool that is use for capturing network traffic, we can use Wireshark anywhere on the network for network packet analyzing, most of the time Wireshark is place between server and switch or between the client and switch to capture every packets, it's also recomend not to place Wireshark on the servers that could sometimes overload the servers. There are two main filters that we use in Wireshark are Capture Filter (pre-capturing) and Display Filter (post-capturing). we use the capture filters on the network interface to only capture the traffic that we wants example (HTTP, HTTPS, SMTP...), After that we use Display filter to further filter those traffic that we already captured by using Capture Filter on network interface. For further understanding of Wireshark check on this link:<br>
 [Wireshark](https://www.wireshark.org/docs/wsug_html_chunked/ChapterIntroduction.html#ChIntroWhatIs 'wireshark').
 


# What is VirusTotal? 

VirusTotal is a online malware scanning tool that helps to detect and analyze malicious files, file hashes, IP addresses, URL's, and domians, it scans these items with over 70 antivirus security vendors and provide the score to determine if the item is malicious or not, we can also find the detail, relationship, association, and behaviour of the malicious item. For further understanding of VirusTotal check on this link:<br> 
[VirusTotal](https://docs.virustotal.com/docs/how-it-works 'VirusTotal').


# Steps to decrypt and analyze malicious HTTPS traffic in Wireshark

# 1 Open the Malicious_HTTPS_SSL_TLS.pcap file

In this step we open the Malicious_HTTPS_SSL_TLS.pcap file in Wireshark the file contains 1465 captured packets and have seven columns.

1) No. (Number of packet).<br>
2) Time (When the packet was captured).<br>
3) Source (Source IP address).<br>
4) Destination (Destination IP address).<br>
5) Protocol (Name of the protocl use in the packet).<br>
6) Length (Packet length).<br>
7) Info (information related to that packet).<br>


Malicious_HTTPS_SSL_TLS.pcap file:
![pac_cap_1](assets/img/dechttptraf/pac_cap_1.png)

We insert two more columns Source Port and Destination Port for further analysis 

Malicious_HTTPS_SSL_TLS.pcap file after adding source and destination ports:
![pac_cap_2](assets/img/dechttptraf/pac_cap_src_dst_prt.png)


# 2 Using Wireshark Display Filter to filter TLS handshake packets

 After opening and adding source and destination ports we can filter the TLS handshake traffic for analysis, to filter the TLS handshake we 
 use "tls.handshake.type eq 1" filter in the display filter, as a result of this filter we get all the TLS handshake traffic, after that we 
 right click on secound captured TLS packet => follow => TCP Stream we see that all of the data of that packet is encrypted.

  Using Display Filter "tls.handshake.type eq 1" to get TLS handshake traffic:
 ![pac_cap_tls](assets/img/dechttptraf/pac_tls_handshake.png)

  Encrypted TLS TCP stream: 
 ![pac_cap_enc_tls_str](assets/img/dechttptraf/pac_cap_enc_tls.png)


# 3 Using Key_Log_file.txt to decrypt the TLS traffic

 To decrypt the encrypted TLS/SSL HTTPS traffic we use Key_Log_file.txt that contains SSL encryption keys, to open this 
 file in Wireshark we have to goto Edit from the top menu bar => prefrences => Protocol under Protocol drop down click TLS
 on the right side of the prefrence menu we have the option "(Pre)-Master-Secret log file name" from here we can browse and
 open our Key_Log_file.txt.


Open Key_Log_file.txt contains SSL keys in Wireshark:
![pac_cap_ssl_key](assets/img/dechttptraf/pac_ssl_key_file.png)


After opening the Key_Log_file.txt we can do the same above steps by first going to the "tls.handshake.type eq 1" to get the
TLS traffic, then right click on second TLS packet => follow => TLS Stream we find that the data of that packet is decrypted by
the SSL encyption key file (Key_Log_file.txt) that we use in Wireshark. 

Decrypted TLS handshake traffic using SSL encryption keys in Key_Log_file.txt:
![pac_cap_decr_tls](assets/img/dechttptraf/pac_cap_decr_tls.png)


# 4 Using Wireshark display filter to decrypt and analyze HTTP request, TLS handshake traffic and exlude SSDP protocol

We can then use another display filter "(http.request or tls.handshake.type eq 1) and !(ssdp)" to filter the encrypted http request, TLS handshake, and exclude SSDP protocol traffic, after using this filter we can able to see all the HTTP requests(GET, POST). After analyzing the filtered traffic we find the suspicious
GET request on the packet number "165" that contains the "invest_20.dll file that shows that the GET request is trying to download or to get access
to system resource file that could be suspicious.

Using display filter "(http.request or tls.handshake.type eq 1) and !(ssdp)" to filter HTTP request, TLS handshake, and exclude SSDP protocol:
![pac_cap_http_tls_ssdp](assets/img/dechttptraf/pac_cap_http_req_tls_ssdp.png)


Suspicious GET request on packet number 165 that trying to download or access system resource file:
![pac_cap_http_susp](assets/img/dechttptraf/pac_cap_http_req_tls_mal.png)


After that we right click on the packet 165 and follow the HTTP stream as we did in above steps we detect that the GET request from the attacker source IP address of (10.4.1.101) is accepted by the server with the IP address of (94.103.84.245) with the response code of 200 and server give access for this file (invest_20.dll) to the attacker, after further analyzing the HTTP stream we can also find the octate file data that shows the "This program cannot be run on DOS" so that we can confirm it's
the actual .dll system file.


Suspecious HTTP stream for the GET request from the packet 165 to access the "invest_20.dll" file:
![pac-cap_http_sus_str](assets/img/dechttptraf/pac_cap_http_str_sus.png)


# 5 Extract the malicious file from the Wireshark and analyze it in VirusTotal

Now we can extract and export the malicious file (invest_20.dll) from the packet 165 as an object and save it locally  (it's always good practice to download or extract the malicious files in a sandbox environment) for malware analysis by going to the File => Export Object => HTTP... => click on the file packet => and click Save to save it locally.

Export the invest_20.dll and save it locally for malware analysis:
![pac-cap-mal-file-ext](assets/img/dechttptraf/pac_cap_mal_file_ext.png)


After extracting and exporting the malicious file locally now we perfrom malware analysis of the invest_20.dll file in VirusTotal.
After analyzing the malicious file in VirusToal we found that this is a malicious file and also uses other names of the .dll file this is a type
of a (Dridex Trojan Malware) that is use to target the financial institutions by targeting the MS Office documents most likely a SpreadSheets that
uses custom macros, and it download specific tools and utilities which is use to download the piece of malware. We can also figure out that this malware also 
imports "KERNAL32.dll" and other System dll files by going to the details => import tabs in VirusTotal.

VirusTotal detection of the malicious file (invest_20.dll):
![vir_tot_det](assets/img/dechttptraf/crwd_dll_mal.png)


VirusTotal detail tab showing the malware uses many other names:
![mal_names](assets/img/dechttptraf/mal_names.png)


VirusTotal detail tab showing that malware also import many other system .dll files:
![imp_sys_dll](assets/img/dechttptraf/mal_import.png)


# Conclusion

After detecting and analyzing the malicious HTTP traffic and file (invest_20.dll) we conclude that the user was compromised and some how targeted by the phishing
attack and downloaded the malicious MS Excel document and open it up in MS Excel and executed the macro which download this file (invest_20.dll) 
which infects his system. This practical activity shows how to decrypt and analyze the HTTP and TLS traffic using SSL encrypted keys and how to extract and export malicious file from Wireshark and then how to perform malware analysis of the malicious file in VirusTotal. To perform any malware analysis please use a safe sandbox environment.  

