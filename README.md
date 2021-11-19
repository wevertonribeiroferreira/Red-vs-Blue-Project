<p align="center">
  <img src="https://github.com/wevertonribeiroferreira/Red-vs-Blue-Project/blob/main/Images/logo.png?raw=true" alt="Sublime's custom image"/>
</p>

# Introduction

I concluded this report with an immersive and very hands-on assessment  where I was able to use the tactics and tools available as a Red team player giving me a better understanding of how data exploitation happens and on the Blue side then once the vulnerability have been identified via SIEM (Kibana), I was aware of the same malicious tactics, techniques and best procedures in order to build a better response strategies around them.

# Network Topology

<p align="center">
  <img src="https://github.com/wevertonribeiroferreira/Red-vs-Blue-Project/blob/main/Images/map.png?raw=true" alt="Sublime's custom image"/>
</p>


# **Red Team Assessment** 

### Recon: Describing the Target
Nmap allowed me to identified the following hosts on the network:

| Hostname                              | IP Address    | Role on Network                                |
|---------------------------------------|---------------|------------------------------------------------|
| Hyper-V Azure machine ML-RefVm-684427 | 192.168.1.1   | Host Machine Cloud based                       |
| Kali                                  | 192.168.1.90  | Attacking Machine                              |
| ELK Stack                             | 192.168.1.100 | SIEM VM                                        |
| Capstone                              | 192.168.1.105 | Target Machine Replicating a vulnerable server |


# Vulnerability Assessment

### The assessment uncovered the following critical vulnerabilities in the target:

|Vulnerability                          |Description                                                          | Impact                                   |
|---------------------------------------|---------------------------------------------------------------------|------------------------------------------------|
| Port 80 open  -  CVE-2019-6579        | Open and unsecured access to anyone attempting entry using Port 80. | Files and Folders are readily accessible. Sensitive (and secret) files and folders can be found.                      |
| Ability to discover password by Brute force CVE-2019-3746|When an attacker uses numerous user and password combinations to access a device or system. | Easy access by use of brute force by programs such as ‘John the ripper’, Hydra and so on.|                              |
| LFI Vulnerability                     |LFI allows access into confidential files on a vulnerable machine. | Allows attackers to gain access to sensitive credentials. |
| Weak Passwords     | Common passwords, and the lack of complexity, such as the inclusion of symbols, numbers and capitals. | System access could be discovered by social engineering. that ‘Leopoldo’ password could be cracked in 21 seconds by a computer. |


# Exploitation: **Brute Force Password**


**Findings**

<p align="center">
  <img src="https://github.com/wevertonribeiroferreira/Red-vs-Blue-Project/blob/main/Images/nmap.png"?raw=true" alt="Sublime's custom image"/>
</p>

  After mapping the network with nmap I was able to gather valuable data about user information. I tried all the possibilites and the user **‘ashton’** was a good option to explore since he is the Manager/admin.
                                                                                                                             
<p align="center">
  <img src="https://github.com/wevertonribeiroferreira/Red-vs-Blue-Project/blob/main/Images/Screenshot%202021-10-23%20142446.png"?raw=true" alt="Sublime's custom image"/>
</p>

**Tools & Processes**

I used Hydra along with a wordlist rockyou.txt using the brute force technique.


**Command**: # hydra -l ashton -P /usr/share/wordlists/rockyou.txt -s 80 -f -vV 192.168.1.105 http-get /company_folders/secret_folder/

<p align="center">
  <img src="https://github.com/wevertonribeiroferreira/Red-vs-Blue-Project/blob/main/Images/Screenshot%202021-10-23%20130839.png?raw=true?raw=true" alt="Sublime's custom image"/>
</p>

**Achievements**

After getting all the information I was able to use the login name **‘ashton’** as well as the password **‘leopoldo’** to gain access.

# Exploitation: **Port 80 Open to Public Access**

**Tools & Processes**

NMAP was used to scan for open ports.

**Achievements**

I found 4 hosts up: On the Capstone Machine two ports was open: 22 and 80 (192.68.1.105).

<p align="center">
  <img src="https://github.com/wevertonribeiroferreira/Red-vs-Blue-Project/blob/main/Images/Screenshot%202021-11-08%20120247.png?raw=true?raw=true" alt="Sublime's custom image"/>
</p>

# Exploitation: **Hashed Passwords**

 After getting access to "connect_to_corp" it shows details about the hashed password.

**Findings** 
<p align="center">
  <img src="https://github.com/wevertonribeiroferreira/Red-vs-Blue-Project/blob/main/Images/hash.png"?raw=true" alt="Sublime's custom image"/>
</p>

**Tools & Processes**

I used an online tools such as: **hashes.com** and **md5decrypt.net** to crack the hashed password.

**Achievements**

The username **Ryan** used the password ‘**linux4u**’ to access the **/webdav** folder. On my attacker machine I was able to access the server dav://172.16.84.205/webdav and was able to login successfully.

<p align="center">
  <img src="https://github.com/wevertonribeiroferreira/Red-vs-Blue-Project/blob/main/Images/Screenshot%202021-10-23%20130802.png?raw=true?raw=true" alt="Sublime's custom image"/>
</p>

# Exploitation: **LFI vulnerability**

**Tools & Processes**

I used msfvenom and meterpreter to deliver a payload onto the vulnerable machine (the capstone server)

**Achievements**

Using the **multi/handler** exploit I could get access to the machine’s shell.

<p align="center">
  <img src="https://github.com/wevertonribeiroferreira/Red-vs-Blue-Project/blob/main/Images/Screenshot%202021-10-23%20150905.png"?raw=true" alt="Sublime's custom image"/>
</p>

<p align="center">
  <img src="https://github.com/wevertonribeiroferreira/Red-vs-Blue-Project/blob/main/Images/Screenshot%202021-10-23%20155025.png"?raw=true" alt="Sublime's custom image"/>
</p>

<p align="center">
  <img src="https://github.com/wevertonribeiroferreira/Red-vs-Blue-Project/blob/main/Images/Screenshot%202021-10-28%20194630.png"?raw=true" alt="Sublime's custom image"/>
</p>

### Flag captured
<p align="center">
  <img src="https://github.com/wevertonribeiroferreira/Red-vs-Blue-Project/blob/main/Images/InkedScreenshot%202021-10-28%20194727.jpg"?raw=true" alt="Sublime's custom image"/>
</p>

# **Blue Team Assessment** 
## Log Analysis and Attack Characterization

### Analysis: Identifying the Port Scan

● The port scan started on October 23, 2021 at 18:00.

● 84,041 connections occurred, the source IP: 192.168.1.90.

● The sudden peaks in network traffic indicate that this was a port scan.

<p align="center">
  <img src="https://github.com/wevertonribeiroferreira/Red-vs-Blue-Project/blob/main/Images/Screenshot%202021-11-08%20121303.png"?raw=true" alt="Sublime's custom image"/>
 </p>
 
  ### Analysis: Finding the Request for a Hidden Directory
  
 ● The request started at 17:00 hrs on October 23th, 2021.
 
 ● 30,450 requests were made to access the /secret_folder.
 
 ● This folder contained a hash that I could use to access the system using another employee’s credentials (Ryan).
 
 <p align="center">
  <img src="https://github.com/wevertonribeiroferreira/Red-vs-Blue-Project/blob/main/Images/Screenshot%202021-11-08%20121626.png"?raw=true" alt="Sublime's custom image"/>
</p>

### Analysis: Uncovering a Brute Force Attack

 ● 30,450 requests were made in the attack to access the /secret_folder.
 
 ● 30 attacks were successful. 100% of these attacks returned a 301 HTTP status code “Moved Permanently”.
 
 <p align="center">
  <img src="https://github.com/wevertonribeiroferreira/Red-vs-Blue-Project/blob/main/Images/Screenshot%202021-11-08%20121940.png"?raw=true" alt="Sublime's custom image"/>
</p>

### Analysis: Finding the WebDAV Connection

●  96 requests were made to access the /webdav directory.

●  The primary requests were for the passwd.dav and shell.php files.
 
 <p align="center">
  <img src="https://github.com/wevertonribeiroferreira/Red-vs-Blue-Project/blob/main/Images/Screenshot%202021-11-08%20122120.png"?raw=true" alt="Sublime's custom image"/>
</p>

# **Blue Team**

## Proposed Alarms and Mitigation Strategies

# Mitigation: Blocking the Port Scan

## Alarm

● I recommend an alert to be sent once 1000 connections occurs in 30 minutes.

## System Hardening

● Automatize a Python script along with NMAP Scan to proactively detect and audit any open ports.

● Ensure the firewall has the latest patched constantly in order to avoid zero-day attacks.

●  Redirect open ports to “honeypots” or empty hosts.

●  Enable 3rd gen. of Deception Technology.

# Mitigation: Finding the Request for the Hidden Directory

## Alarm

● Set an alert when an invader requests access to hidden folder occur. 

● I would recommend a threshold of maximum 3 attempts per every 30 minutes that would trigger an alert to be sent.

## System Hardening

● Offline storage for highly sensitive information.

● Rename folders containing critical data.

● Encrypt data contained within confidential folders 

● Manage IP addresses either to add on the whitelist or blocklist.

# Mitigation: Preventing Brute Force Attacks

## Alarm

A HTTP 401 Unauthorized client error indicates that the client failed to provide any such authentication. 

● I would detect future brute force attacks by setting an alarm that alerts if a 401 error is returned. 

● The threshold I would set to activate this alarm would be when 10 errors are returned.

## System Hardening

● I would create a policy that locks out accounts after 3 unsuccessful attempts and contact Service Desk to confirm PII and unlock account. 

● I would create a password policy that requires password complexity (Lowercase, Uppercase, Number and a Special character.) expiring every 3 months and not accepting same old passwords.

● I would create a blocklist of IP addresses based on IP addresses that have 30 unsuccessful attempts in 3 months. If the IP address happens to be a staff member, advise may be required by the cybersecurity team.

# Mitigation: Detecting the WebDAV Connection

## Alarm

● First, I would create a Whitelist of trusted IP Addresses. According to the least privilege access On HTTP GET request, I would set an alarm that activates on any IP address trying to access the webDAV directory. The threshold I would set to activate this alarm would be when any HTTP PUT request is made.

## System Hardening

● Create a whitelist of trusted IP addresses and ensure that the firewall security policy prevents all other access. 

● In conjunction with other mitigation strategies, I would ensure that any access to the WebDAV folder is only permitted by users with ‘Salted’ passwords.

● Enable MFA.

# Mitigation: Identifying Reverse Shell Uploads

## Alarm

● Create an alert for any suspicious traffic on port 4444. The alert needs to be sent is when one or more attempt is made. 

● I recommend setting an alert for suspicious extensions being uploaded into the /webDAV folder. The alert needs to be sent is when one or more attempt is made. 

## System Hardening

● Block all IP addresses other than whitelisted IP addresses (because reverse shells can be created over DNS, this action will only limit the risk of connect-back shell). 

● On the /webDAV folder enable read only to prevent payloads uploaded.

● Only necessary ports are open.

## Sources:

● [CVE-2019-6579](https://nvd.nist.gov/vuln/detail/CVE-2019-6579)

● [CVE-2019-3746](https://nvd.nist.gov/vuln/detail/CVE-2019-3746)

● [File Inclusion Vulnerabilities](https://www.offensive-security.com/metasploit-unleashed/file-inclusion-vulnerabilities/)

● [Password Monster](https://www.passwordmonster.com/)

● [Hydra](https://www.nanoshots.com.br/2015/07/brute-force-em-portas-ssh-com-hydra.html)

● [Hashes](https://www.hashes.com)

● [MD5Decrypt](https://www.md5decrypt.net)

● [Zero-Day](https://www.fireeye.com/current-threats/what-is-a-zero-day-exploit.html)

● [Honeypot](https://www.kaspersky.com.br/resource-center/threats/what-is-a-honeypot)

● [Deception Technology](https://revolutionaries.zscaler.com/insights/you-need-deception-technology-and-its-not-why-you-think)

## Apresentation Version:

[Link](https://docs.google.com/presentation/d/1NBSeRC3NZ-KwQSiWs8FmSGG9fKPx8ULl/edit?usp=sharing&ouid=113802642956802592612&rtpof=true&sd=true)

