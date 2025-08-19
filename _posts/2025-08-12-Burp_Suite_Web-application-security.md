---

title: "How to use Burp Suite with DVWA Vulnerable Web Application"
date: "2025-08-12 00:00:00"
categories: [Web Application Security, Network Packet Analysis, How to use Burp Suite with DVWA Vulnerable Web Application]
tags: [Web Application Security]

---


# Overview

In this web application security project, I practically learned how to use Burp Suite proxy with DVWA (Dam Vulnerability Web Application) vulnerable web application. Burp Suite is a proxy which consists of sets of software tools used for web application security. It is use to intercept HTTP requests between client and server. In this project I covered three main software tools "Proxy", "Repeater", and "Intruder" in Burp Suite Community Edition. DVWA is a vulnerable web application that is used by security professionals and learners to test different types of web application vulnerabilities. Initially, I started this project by configuring Burp Suite with the Firefox browser using the FoxyProxy browser extension on my Kali Linux Virtual machine. Then I intercepted the DVWA(I had already set up in my Kali Linux VM) vulnerability, Brute force login requests in Burp Suite by first testing the request using the Repeater tool and then using Intruder tool to perform two different types of attacks "Sniper", and "Pitchfork" in Burp Suite. The following are the steps that I used to complete this project.


**1) Configure and set up the Burp Suite with Firefox browser using the FoxyProxy browser extension.**<br>
**2) Using Proxy tool to intercept the first web request in Burp Suite.**<br>
**3) Filter and add DVWA web traffic to scope in Burp Suite.**<br>
**4) Using Repeater tool to test intercepted DVWA Brute Force login web request in Burp Suite.**<br>
**5) Using Intruder tool to perform Sniper and Pitch Fork attacks on DVWA Brute Force login web request.**<br>


# 1. Configure and set up the Burp Suite with Firefox browser using FoxyProxy browser extension

In this step, I configured and set up the Burp Suite which was already pre-installed on my Kali Linux virtual machine. I first started the Burp Suite Community Edition in my Kali Linux VM then by going to the Proxy and then going to the proxy settings to check that the proxy listener was checked mark and the Burp Suite was listening request at "127.0.0.1:8080". 


Burp Suite listening request on "127.0.0.1:8080":
![Burp_ipprt](assets/img/BurpSuiteDVWA/BurpSuite_defa_IP_prt.png)

After verifying it, I opened up the FireFox browser and downloaded the Burp Suite certificate by going to the "http://burpsuite", then I uploaded the Burp Suite certificate by going to the Firefox browser settings, privacy and security certificates, Authorities and then imported the downloaded Burp certificate.

Downloading the Burp Suite certificate from "http://burpsuite":
![Burp_ipprt](assets/img/BurpSuiteDVWA/burp_cert_down.png)

Import Burp Suite certificate into the FireFox web browser:
![Burp_cert](assets/img/BurpSuiteDVWA/import_burp_cert_firefx.png)

Import Burp Suite certificate into the FireFox web browser:
![Burp_cert2](assets/img/BurpSuiteDVWA/import_burp_cert2.png)

After importing the Burp certificate into the Firefox browser successfully then I installed the FoxyProxy browser extension, which is used to manage different proxies, and then added the Burp Suite proxy by providing the Burp Suite proxy Hostname "127.0.0.1" and port number "8080" on which the Burp Suite is listening the HTTP requests.

FoxyProxy Browser extension in FireFox web browser:
![Foxypro_ext](assets/img/BurpSuiteDVWA/Foxy_Proxy_ext.png)

Adding Burp Suite to the FoxyProxy browser extension:
![Addburp_foxy](assets/img/BurpSuiteDVWA/add_burp_prox_foxy.png)


# 2. Using Proxy tool to Intercept the first Web Request in Burp Suite.

In this step, I tried to test and intercept the GET request for the "www.google.com" from the FireFox browser to check if I was able to intercept the web request by going to the Proxy tab, then going to the Intercept and clicking the "Intercept on" button. From Firefox, I first verify and check that FoxyProxy is on for the BurpSuite and then send the GET request to "www.google.com" from the Firefox browser. From the Burp Suite Proxy tool, I successfully intercepted the GET request for the "www.google.com". As a note, it's unethical to keep intercepting public website requests. I did it once just to check if I was able to intercept the request.  

Intercepting "www.google.com" request in Burp Suite Proxy tool:
![Intr_googreq](assets/img/BurpSuiteDVWA/inter_goo_req_frefox.png)


# 3. Filter and add DVWA Web Traffic to Scope in Burp Suite.

To work with DVWA vulnerable web application requests, first I had to install and setup DVWA on my Kali Linux VM which I had already downloaded and installed from GitHub"https://github.com/digininja/DVWA.git". As a note, if installing the DVWA on a virtual machine, make sure to use NAT network configuration for security. After installing and setting up the DVWA on my Kali Linux VM, I went to the DVWA website "http://localhost/DVWA/index.php" from the Firefox browser, and I was successfully able to reach the DVWA website by providing my username and password.

DVWA web application:
![Dvwa_webapp](assets/img/BurpSuiteDVWA/DVWA_site.png)

To add the DVWA web traffic to the scope in Burp Suite, I first went to the proxy tool and turned on the Intercept and refreshed the DVWA website. I got the GET request that was intercepted by Burp Suite. I then forward that request, and the website shows the DVWA index page. To add the DVWA web traffic to the scope in Burp Suite, I went to the Target then site map, then right-clicked on the url "http://localhost/" which belongs to the DVWA website, and added it to the scope by clicking "Add to Scope". From the proxy tool settings, I also changed it to show "show only in-scope items" by going to Proxy tool, then HTTP history, then Filter settings. After changing these settings, Burp Suite will only intercept the request from the DVWA web application and not intercept other web traffic.

Adding DVWA web traffic to the scope in Burp Suite:
![add_dvwascop](assets/img/BurpSuiteDVWA/add_dvwa_scope_burp.png)

Intercepting only DVWA web traffic in Burp Suite:
![intr_dvwascop](assets/img/BurpSuiteDVWA/interc_only_DVWA_traffic.png)


# 4. Using Repeater tool to test intercepted DVWA Brute Force Login Web Request in Burp Suite

The repeater tool in Burp Suite is used to test the intercepted request by changing and modifying the request, and send those modified requests to check the response from the server by not sending back those responses to the user's web browser. In this step, I first send the DVWA Brute Force login request with the username and password and intercept that POST request in the Burp Suite proxy tool. From the proxy tool, I right-clicked on that POST request and clicked "Send to Repeater".

Sending DVWA Brute Force login request:
![Dvwa_req](assets/img/BurpSuiteDVWA/send_req_from_dvwa_in_rep.png)

Intercepting and sending DVWA Brute Force login request from Proxy tool to Repeater tool:
![req_torep](assets/img/BurpSuiteDVWA/send_req_to_rep.png)

From the Repeater tool, I modified the DVWA Brute Force login request by changing the username to "admin3" and password to "password" and then sent that modified request to Repeater to check the response. From the response, I found out that the user was not granted access with the message "Username and/or password incorrect". I modified that request again with the username to "admin" and password to "password" and then checked the response again and found out that the user was granted access with the message "welcome to the password protected area admin". 

Response from the Login request is not accepted with the username "admin3" and password "password":
![req_noacc](assets/img/BurpSuiteDVWA/req_acc_no_grant.png)

Response from the Login request is accepted with the username "admin" and password "password":
![req_acc](assets/img/BurpSuiteDVWA/req_acc_gran.png)


# 5. Using Intruder tool to perform Sniper and Pitch Fork attacks on DVWA Brute Force Login Web Request

The Intruder tool in Burp Suite is used to perform attacks against web applications by automatically customizing the attacks, and actions that we take on Intruder will affect the web application in the user's web browser. It is used for attack, not for the testing requests that we did on the repeater tool in the steps above. There are four different types of attack that we can perform from the Intruder tool "Sniper attack", "Battering attack", "Pitchfork attack", and "Cluster bomb attack". For testing purposes, I performed "Sniper" and "Pitch Fork" attacks from the Intruder tool.

# 5a. Sniper attack on DVWA Brute Force Login Web Request
 
This type of attack uses a single payload list and uses that list to the selected parameters from the request. To perform the Sniper attack, I first sent the modified DVWA Brute Force login request with the username "admin3" and password "password" from the Repeater tool to Intruder by right-clicking on the request and then clicking "Send to Intruder". From the Intruder tool, I select the parameters "admin3" and "password" and click on the "Add $" button to add those parameters.
 
Sending DVWA Brute Force request from Repeater tool to Intruder tool:
![req_intr](assets/img/BurpSuiteDVWA/send_req_intr.png)
  
Adding parameters to perform Sniper attack:
![snip_atk](assets/img/BurpSuiteDVWA/snip_att.png)

After adding the parameters from the payload tab, I selected the payload type to "Simple list" and then clicked on the load button to upload the "passwords.txt" file as a list which contains three passwords "password", "secure", and "test", and then performed the attack on DVWA Brute Force login request.

Adding "passwords.txt" list as a payload list to perform Sniper attack:
![add_paslst](assets/img/BurpSuiteDVWA/add_pass_lst_pay.png)

After performing the attack, I noticed that it performed the attack with 6 requests. The first 3 requests have the usernames that are used from the "passwords.txt" list with the password parameter being the same as "password". So it changes the username by replacing it with the passwords that I provided in the "passwords.txt". In the last 3 requests, the username was not changed, and it replaced the password with the passwords provided in the "passwords.txt". I figured out that the Sniper attack is good for single sets of payloads.

First three requests with different usernames from "passwords.txt" payload list with the same passwords:
![snip_diffusr](assets/img/BurpSuiteDVWA/snip_diff_usr_same_pass.png)
 
Last three requests with the same usernames and different passwords from "passwords.txt" payload list:
![snip_samusr](assets/img/BurpSuiteDVWA/snip_same_usr_name_diff_pass.png)

# 5b. Pitch Fork attack on DVWA Brute Force Login Web Request 

This type of attack is used to simultaneously test the multiple payload positions in parallel by using the two sets of payload lists. The payloads are inserted parallel from the lists and placed in the corresponding payload positions at the same time. To perform the Pitch Fork attack, I sent the same DVWA Brute Force login request that I used in the steps above from the Repeater tool to the Intuder. Then I added the username and password parameter using "Add $" button and then, from the payload tab, I loaded two lists, "users.txt" which contains three users "admin", "sam", and "kate" as my first payload list and then loaded the same "passwords.txt" list which contains three password "password", "secure", and "test" as my second payload list, then I perfromed the attack.

Adding "users.txt" as a first payload list to perform Pitch Fork attack:
![user_lst](assets/img/BurpSuiteDVWA/user_name_list_ptch_attk.png)

Adding "passwords.txt" as a second payload list to perform Pitch Fork attack:
![pass_lst](assets/img/BurpSuiteDVWA/pass_lst_ptch_attk.png)

 After performing the attack, I noticed that it performed the attack with 3 different requests by iterating each of the usernames and passwords from the "users.txt" and "passwords.txt" payload lists in parallel by replacing the usernames and password from the request. After analyzing the response from the first request with the username "admin" from the "users.txt" and the "password" password from "passwords.txt" I found out the user was granted access with the message "Welcome to the password protected area" The other two requests with the username "sam" and "kate" and with the password "secure" and "test" not granted the access with the message "Username and/or password incorrect".

Response from the first request with the username "admin" and password "password" was accepted:
![req_grnt](assets/img/BurpSuiteDVWA/req_accp_attk.png)

Response from the second request with the username "sam" and password "secure" was not accepted:
![req_nogran](assets/img/BurpSuiteDVWA/req_no_accp_attk.png)


# Conclusion

In this web application security project I learned how to set up Burp Suite, and how to configure it using the FoxyProxy browser extension in the Firefox web browser. I also learned how to intercept the request, test the request, and how to perform different types of attacks by using Burp Suite's three main tools "Proxy", "Repeater", and "Intruder". To test these tools, I used the DVWA vulnerable web-application Brute Force login to test login requests using the Repeater tool and to perform "Sniper" and "Pitch Fork" attacks using the Intruder tool from Burp Suite.








