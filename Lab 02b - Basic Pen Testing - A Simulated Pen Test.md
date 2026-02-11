# Lab 2b: Basic Pen Testing - A Simulated Pen Test

Estimated Time: 180 Mins

This Lab will allow you carry out a Pen Test in a ethical way, using a structured professional penetration testing methodology, in a controlled environment. You will be able to undertake a safe reconnaissance and service enumeration on a target virtual machine (TVM), while you map findings to possible vulnerabilities and how you might exploit them, using vendor documentation and CVE advisories. Essentially you are attempting to gain admin or super user access to the TVM. All Tasks should only be executed on the provided TVM and subnet.

> ⚠️ :Warning: DO NOT ATTEMP TO SCAN ANY OTHER HOSTS OR NETWORKS OTHER THAT THE IP ADDRESS PROVIDED DURING THE LAB. 
___

The virtual machine used for this Lab is available for you to download and run locally on your own computer using Oracle Virtual Box. It can be downloaded from here: https://tudublin-my.sharepoint.com/:u:/g/personal/phillip_fitzpatrick_tudublin_ie/IQDh4js5g1umSJuF48eS4yROAVguzcQgkcPXcBgmwY75k5A?e=TUFcqq

💡 :Note: You will need to use a Bridged Adapter network connection between your instance of Kali Linux and the TVM for it to work locally on your machine.

Settings --> Basic --> Network --> Network Adapater 1 --> Attached to & Choose Bridged Adapter from the list of dropdown options. 

> ⚠️ :Warning: YOU SHOULD NOT ATTEMPT TO DO THIS WHILE CONNECTED TO EDUROAM OR ANY TU DUBLIN WIFI NETWORK. 

___


## Step 1 – Reconnaissance (Information Gathering) 

Goal: To build a picture of the target landscape without intrusive actions. Focus on discovering the TVM, identifying reachable services, and form an initial hypothesis about the system. We typically achieve this using nmap. But you can also use tools like netdiscover or arp-scan. (I'll demonstrate this initial step in the lab using the nmap command. nmap -sn 192.168.1.0/24). 
This should give us a list of all active hosts on the scanned network.

Once we have the IP address of the TVM we can perform an initial scan on that machine using nmap.

Task: run ```nmap -sV 192.168.1.x``` - "where x is the actual IP of the target virtual machine that you are looking to scan - I'll give this to you during the Lab". This should return several open ports on the TVM, along with running services. 

We could also run a more comprehensive scan - nmap -sV -A -p- 192.168.0.X -T4, where
-sV – Service/version detection
-A – Aggressive
-p – Scan all TCP ports
-T4 – Timing template: Aggressive

If the nmap scan is too aggressive then it has the potential to trigger any intrusion detection systems that might be active on either the TVM or the network that you are scanning.


## Step 2 – Service Enumeration & Analysis

Goal: To understand what each discovered service does, the versions involved, and any common configuration patterns. During this step the the focus should be on observation and documentation.

Task: run ```nmap -sV -A --script vuln 192.168.1.x```
Or we could run a more controlled vulnerability scan as follows. 

sudo nmap -sV -T4 --script vuln --script-timeout 15s --stats-every 5s -vv 192.168.0.x

It is critical at this stage that every open service is comprehensively investigated to identify any potential vulnerabilities which be exploited to compromise the TVM.

There as many different tools that we can use, for each identified service, to achieve this.

   1. Web servers - Web browser, Nikto, WhatWeb and DevTools
   2. SMB/Windows shares - smbclient or enum4linux
   3. SSH/FTP - OpenSSH or any FTP client
   4. DNS records - nslookup or dig


## Step 3 – Vulnerability Research
Goal: Correlate observed versions/configurations with typical risks using reputable sources. Do not execute exploits at this stage but focus on understanding the different types of vulnerabilities associated with the open ports and identified services. 

There are serval sources that you can use to achieve this including.

   1. CVE databases - will help you understand known vulnerabilities at a high level.
   2. Vendor documentaion & change logs or version history - Will help you identify secure configuration guidance and version notes.
   3. Security advisories - will help you learn common misconfigurations and how they can be mitigated.


## Step 4 – Initial Access
Goal: Explore how week authentication or misconfigurations could potentially expose an access paths on the TVM.

We need to now enumerate for each open service that have been identified.

a) Web Servers

Task: Using your favourite web browser open http://192.168.1.x

Use nikto to get an understanding of the directory structure.

Task: run ```nikto -h 192.168.1.x``` 
We could also use dirb http://192.168.1.x or the dirbuster tools to achieve the same result.

Task: Try out both of these tools to validate the result obtained byfrom using nikto.
When you ran nikto did you find any other directories and where there any file in them?

b) SMB/Windows shares
Lets see can we find any usernames for the SMB service. 

Task: run ```enum4linux 192.168.1.x```

c) Brute Forcing SSH
Lets seen can we brute force the SSH credentials with hydra.

Task: run ```hydra -l Jan -P /usr/share/wordlists/rockyou.txt ssh://192.168.1.x```

💡 :Note: If you have not used the rockyou.txt wordlist before, you will have to unzip it first

Task: run ```sudo gzip -d /usr/share/wordlists/rockyou.txt.gz```

--> Result: TVM access is granted for Jan by exploiting a vulnerability in the SSH service.

Task: ssh jan@192.168.1.x

c) Now we can enumerate the file system on the TCM

Task: run ```ls - la```

Task: run ```cd ..```

Task: run ```ls -la```

Task: run ```CD kay```

Task: run ```ls -la```

Task: run ```is```

Task: run ```whoami```


## Step 5 – Privilege Escalation
Goal: We need to analyse permission boundaries and roles to understand how design issues could enable privilege changes. This could be achieved as follows.
   1. User/group membership and role separation.
   2. File and directory permissions.
   3. Running services and their privileges.
   4. Delegated permissions and allowed actions.

We will enumerate SetUID binaries for privilege escalation.

Task: run ```find / -perm -4000 2>/dev/null```

Task: run ```vim pass.bak```

Task: run ```SU kay```

--> Result: Super User access is granted for kay on the TVM.


## Step 6 – Reporting & Lab Submission
Goal: Deliver a professional-style report for you Lab submission that conveys how the TVM was found to be vulnerable and exploited. Include any and all gathered evidence (e.g., screenshots of banners or configurations) and results of tools used to carry out all scan and associated exploits.

As a minimum you should include the following in your Lab report submission.

• Cover Page

• Table of Contents

• University Declaration and any use of AI

• Executive Summary - non-technical overview

• Methodology and Scope

• Findings
   - with associated context, evidence screenshots, and exploitable vunrability.

• Recommendations -"You will need to do some research"
   - remediation and hardening steps.
     
• Appendices 
   - tools used and associated settings.


## Extra Step

There is one extra step that you might want to try complete. This TVM is actually a capture the flag VM. Can you find the flag and read it?





