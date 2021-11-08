<p align="center">
  <img src="https://github.com/wevertonribeiroferreira/Red-vs-Blue-Project/blob/main/Images/logo.png?raw=true" alt="Sublime's custom image"/>
</p>

# Preface


# Network Topology

<p align="center">
  <img src="https://github.com/wevertonribeiroferreira/Red-vs-Blue-Project/blob/main/Images/map.png?raw=true" alt="Sublime's custom image"/>
</p>


text

# **Red Team** 
## Security Assessment

### Recon: Describing the Target
Nmap identified the following hosts on the network:

| Hostname                              | IP Address    | Role on Network                                |
|---------------------------------------|---------------|------------------------------------------------|
| Hyper-V Azure machine ML-RefVm-684427 | 192.168.1.1   | Host Machine Cloud based                       |
| Kali                                  | 192.168.1.90  | Attacking Machine                              |
| ELK Stack                             | 192.168.1.100 | Network Monitoring Machine running Kibana      |
| Capstone                              | 192.168.1.105 | Target Machine Replicating a vulnerable server |


# Vulnerability Assessment
##The assessment uncovered the following critical vulnerabilities in the target:

|Vulnerability                          |Description                                                          | Impact                                   |
|---------------------------------------|---------------------------------------------------------------------|------------------------------------------------|
| Port 80 open  -  CVE-2019-6579        | Open and unsecured access to anyone attempting entry using Port 80. | Files and Folders are readily accessible. Sensitive (and secret) files and folders can be found.                      |
| Ability to discover password by Brute force CVE-2019-3746|When an attacker uses numerous user and password combinations to access a device or system. | Easy access by use of brute force by programs such as ‘John the ripper’, Hydra and so on.|                              |
| LFI Vulnerability                     |LFI allows access into confidential files on a vulnerable machine. | Allows attackers to gain access to sensitive credentials. |
| Weak Passwords     | Common passwords, and the lack of complexity, such as the inclusion of symbols, numbers and capitals. | System access could be discovered by social engineering. that ‘Leopoldo’ password could be cracked in 21 seconds by a computer. |


# Exploitation: **Brute Force Password**

**Tools & Processes**

I used Hydra which is already preinstalled on Kali Linux. I also required a password list – in this case I used rockyou.txt

**Command**: $ hydra -l ashton -P /root/Downloads/rockyou.txt -s 80 -f 192.168.1.105 http-get /company_folders/secret_folder
<p align="center">
  <img src="https://github.com/wevertonribeiroferreira/Red-vs-Blue-Project/blob/main/Images/Screenshot%202021-10-23%20130839.png?raw=true?raw=true" alt="Sublime's custom image"/>
</p>
**Achievements**

After getting all the information I was able to use the login name ‘ashton’ as well as the password ‘leopoldo’ to gain access.
