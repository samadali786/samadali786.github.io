---

title: "Network Packet Analysis of TLS traffic with Wireshark and DeepSeek AI"
date: "2025-1-31 00:00:00"
categories: [Activities, Network Packet Analysis, Network Packet Analysis with Wireshark and DeepSeek AI]
tags: [Malware Analysis]

---

# Overview

In this network packet analysis activity, I first captured, analyzed, and understood the protocol version mismatch network traffic error on Wireshark locally. Then I used DeepSeek AI assistance to check if it's able to provide me with the solution that I already figured out in Wireshark. I am quite surprised with the results and the key evidence that I was getting from DeepSeek AI assistance.   


# Steps for Network Packet Analysis of TLS traffic with Wireshark and DeepSeek AI

# 1 Capturing the deprecated TLS version network traffic on Wireshark

In this first step, I went over to the [bad ssl](https://badssl.com/ 'bad ssl') link from here I used the deprecated tls-v-1 version protocol while capturing the network traffic on Wireshark. I found the client "Hello" and Server "Hello" request and response packets, then I stopped the packet capturing on Wireshark and analyzing the TLS Handshake of those two packets from the Packet Detail pane.


# 2 Analyzing the TLS Handshake protocl version of the client request "Hello"

In this step I analyzed the TLS Handshake of the client request "Hello" from the packet detail pane and found that the client use
TLS 1.2 version and also support TLS 1.3 version.

Client "Hello" TLS Handshake request with TLS 1.2 version: 
![clientreq](assets/img/deepseekpcap/client_hello_tlsv1.2.png)


Client supporting TLS 1.3 and TLS 1.2 TLS Handshake versions:
![clientversup](assets/img/deepseekpcap/client_supported_tls_version.png)


# 3 Analyzing the TLS Handshake protocl version of the server response "Hello"

In this step, I analyzed the TLS Handshake of the server response "Hello" from the packet detail pane and found that the server uses TLS 1.1 version, which was a deprecated TLS Handshake version because of security flaws.

Server using TLS 1.1 TLS Handshake version:
![serverres](assets/img/deepseekpcap/server_hello_tls_v1.1.png)

# 4 Analyzing the TLS Handshake protocol version mismatch error from client

In this step, I analyzed the next client packet after getting the "hello" response from the server, and found that the client did not want to continue the connection with the server due to the protocol version mismatch error, because the client did not support the deprecated (TLS 1.0, TLS 1.1) versions, and server send response by using TLS 1.1 version.

Client not supporting server TLS 1.1 TLS Handshake version:
![clientnotsup](assets/img/deepseekpcap/client_not_sup_ver1.1.png)

After analyzed the network traffic in Wireshark I found that there is a TLS handshake protocol version mismatch error, so I decided to try
DeepSeek AI assistance to check if it wil able to figured out this problem.

# 5 Using DeepSeek AI to check if it is able to find out TLS handshake protocol versions mismatch error:

In this step I first changed the already captured (.pcap) file into (.txt) file in bytes from Wireshark because DeepSeek AI not accepting the (.pcap) files for analysis, so I uploaded that converted pcap file(broken_tls.txt) on DeepSeek AI with the question below.

Question to DeepSeek AI with the pcap file that was converted into .txt(broken_tls.txt) file:
![deepseeques](assets/img/deepseekpcap/DeepSeek_brok_tls_ques_with_.txt.png)


# DeepSeek AI response with key Evidence

DeepSeek AI able to found the TLS protocol version mismatch error with the Server configuration Issue, Client Behaviour, and Results in the breakdown
format.

DeepSeek AI response related to TLS protocol version mismatch error:
![deepseeresp](assets/img/deepseekpcap/DeepSeek_resp.png)


DeepSeek AI Key Evidence from the PCAP:
![deepseekyev](assets/img/deepseekpcap/key_evidence_DeepSeek.png)


# Conclusion

In this activity I was trying to figure out if DeepSeek AI was able to find the protocol mismatch error from the (.pcap) file, and it was successfully able to figure out the problem with the valid response and key evidence, but I am not sure yet if it could be able to provide solutions to the more complex packet capture analysis solutions.



















