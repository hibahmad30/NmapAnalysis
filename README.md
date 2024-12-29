<h1>Nmap Reconnaissance and Vulnerability Assessment</h1>

<h2>Description</h2>
This project focuses on utilizing Nmap to scan Metasploitable 2, an intentionally insecure virtual machine, from a Windows 10 environment. The objective is to discover open ports, identify running services, and uncover security weaknesses. The project includes an overview of key Nmap commands and demonstrates the use of the Nmap Scripting Engine (NSE) to perform a vulnerability scan. By analyzing the scan results, potential risks are evaluated, showcasing the importance of network reconnaissance and vulnerability assessment in enhancing system security.
<br />

<h2>Environments Used </h2>

- <b>Oracle VM VirtualBox</b>
- <b>Nmap Security Scanner</b>  
- <b>Windows 10</b>
- <b>Rapid7 Metasploitable 2</b>
- <b>Nmap Scripting Engine (NSE)</b>
- <b>Zenmap GUI</b> 

<h2>Prerequisites:</h2> 

<p align="center">
For this project, I will use the same configuration as my previous project, where a Metasploitable 2 virtual machine and a Windows 10 virtual machine are set up in Oracle VirtualBox. Metasploitable 2 serves as an intentionally vulnerable Linux-based machine for testing and training purposes. You can find the configuration instructions for the setup in this project: https://github.com/hibahmad30/MetasploitVulnAnalysis. 
 <br/> 
 <br/>
The primary difference in this project is the addition of Nmap, which will be installed on the Windows 10 virtual machine to conduct port scans and vulnerability assessments. To download and install Nmap, follow the instructions provided on the official Nmap download page: https://nmap.org/download.html 
 <br/> 
 <br/>
<img src="https://i.imgur.com/xs5FPLg.png" alt="Nmap Installation Status"/>
 <br/> 
 <br/>
Once Nmap is installed on Windows, we need to configure it to run from the Windows command line. First, open the Windows command line and run the ‘SystemPropertiesAdvanced.exe’ command to open System Properties to the Advanced tab. 
 <br/> 
 <br/>
Then, click the ‘Environment Variables’ tab, and choose ‘Path’ from the ‘System Variables’ section. Click ‘Edit’, and add the Nmap directory (c:\Program Files (x86)\Nmap).
 <br/> 
 <br/>
<img src="https://i.imgur.com/syFpMSg.png" alt="Edit Environment Variable"/>

<h2>Basic Nmap Scans - TCP Connect & SYN:</h2> 

<p align="center">
In this project, we will use two Nmap scan types: -sT (TCP Connect Scan) and -sS (TCP SYN Scan). These scan types utilize the commands ‘nmap -sT 10.0.2.4’ and ‘nmap -sS 10.0.2.4’, where ‘10.0.2.4’ is the IP address of the target system. If no specific ports are specified, Nmap will scan the top 1000 ports by default.
 <br/> 
 <br/>
-sT completes the full TCP handshake. Nmap sends a SYN packet, and if the port is open, the target replies with a SYN-ACK. Nmap then responds with an ACK, establishing a connection. This scan is slower and easier to detect since it establishes a full connection.
 <br/> 
 <br/>
<img src="https://i.imgur.com/m4n7cV4.png" alt="TCP Connect Scan"/> 
 <br/> 
 <br/>
-sS is a stealthier method of scanning, where Nmap sends a SYN packet, and if the port is open, the target replies with a SYN-ACK. Instead of completing the handshake, Nmap sends a RST packet to terminate the connection before it’s fully established. This makes it faster and less detectable.
 <br/> 
 <br/>
<img src="https://i.imgur.com/L1ZxxPw.png" alt="TCP SYN Scan"/> 
 <br/> 
 <br/>
The key difference is that -sT completes the full connection, while -sS stops before the connection is fully established, making it a more stealthy option.
 
<h2>OS Detection and Scanning Specific Ports:</h2> 
<p align="center">
Nmap also allows for specifying which ports to scan. For example, to scan SMTP (port 25), used for sending emails, DNS (port 53), which is responsible for domain name resolution, and HTTP (port 80), used for web traffic, you would use the command nmap -sT -p 25,53,80 10.0.2.4 for a full TCP connection scan, or nmap -sS -p 25,53,80 10.0.2.4 for stealth mode: 
<br />
<br />
<img src="https://i.imgur.com/lBmA4c3.png" alt="Specify Ports Scan"/> 
<br />
<br />
<img src="https://i.imgur.com/rONhUQZ.png" alt="Specify Ports Scan - Stealth Mode"/> 
<br />
<br />
We can take this a step further by using the nmap -O 10.0.2.4 command to detect the OS of our target system. This command uses Nmap's OS detection feature, which analyzes network responses and compares them to a database of known operating system signatures to identify the OS running on the target machine: 
<br />
<br />
<img src="https://i.imgur.com/Ie7kwwa.png" alt="OS Detection"/> 
<br />
<br />
Instead of using -O, you can use -A to get even more details, such as OS detection, version information, script scanning, and traceroute. The -A option provides a comprehensive scan that includes additional information like SSH hostkey, which is a unique identifier used by SSH servers to establish secure connections, and the version of SSH being used. It also provides SMB info for file sharing and a traceroute to show how far the target is from us on the network.
<br />
<br />
<img src="https://i.imgur.com/jUmU0qP.png" alt="OS Detection Extended"/> 
<br />
<br />
<img src="https://i.imgur.com/xoANwZn.png" alt="SMB Information"/> 

<h2>Scanning with Obfuscation and Zenmap GUI Demo:</h2> 
 <p align="center">
When using '-sS' for a stealth scan, '-D' can be used to specify a decoy IP address. This helps avoid detection by making it harder for the target system to identify the actual source of the scan. If the target sees multiple requests from different IP addresses, it becomes more challenging to trace the scan back to the real attacker. The command would be 'nmap -sS -D 10.0.2.10 10.0.2.4', where '10.0.2.10' is the decoy IP address and '10.0.2.4' is the target.
 <br/>
 <br/>
 <img src="https://i.imgur.com/Zxm9cWt.png" alt="Decoy Scan"/>
  <br/>
 <br/>
As a bonus, if you prefer using a GUI to run Nmap scans, Nmap offers the Zenmap GUI. In Zenmap, you can simply provide the target IP and choose from a variety of preconfigured scan options. Additionally, it allows you to specify custom commands for more advanced scanning needs, offering an easy-to-use interface for those who prefer not to work with the command line.
  <br/>
 <br/>
 <img src="https://i.imgur.com/4uY4ge5.png" alt="Zenmap GUI"/> 

<h2>Comprehensive Vulnerability Scan with Nmap Scripting Engine:</h2> 
 <p align="center">
The Nmap Scripting Engine (NSE) is a feature within Nmap that allows users to automate a wide range of scanning tasks using pre-written scripts. NSE scripts are written in the Lua programming language and are organized into categories based on their functionality.
<br />
<br />  
For a list of available NSE scripts, you can visit: https://nmap.org/nsedoc/categories/. Under the ‘vuln’ category, there are various scripts designed to scan for specific vulnerabilities. The command 'nmap --script vuln 10.0.2.4' runs all the scripts in the ‘vuln’ category on the target IP (10.0.2.4), performing a comprehensive vulnerability scan and providing details on any identified CVEs (Common Vulnerabilities and Exposures). 
<br />
<br />  
<img src="https://i.imgur.com/YJs8y5m.png" alt="Nmap Scripting Engine"/>
<br />
<br />
Here are a few of the vulnerabilities detected by the scan, starting with CVE-2011-2523, a vulnerability found in a specific version of the vsftpd (Very Secure FTP Daemon) software, version 2.3.4. If this version of vsftpd was downloaded between June 30, 2011, and July 3, 2011, it contained a malicious backdoor. This backdoor was intentionally inserted by an attacker into the source code.
<br />
<br />  
When exploited, the backdoor opens a shell (a command line interface) on port 6200/tcp, giving the attacker unauthorized remote access to the system. Essentially, it allows someone to take control of the server without proper permission, making it a significant security risk.
<br />
<br />  
<img src="https://i.imgur.com/oxveLd8.png" alt="FTP Backdoor"/> 
<br />
<br />  
The next vulnerability involves port 1099, used by the Java RMI Registry. By default, the registry's configuration allows it to load code from remote servers. This can be exploited by an attacker to trick the registry into downloading and running malicious code from their server, potentially leading to remote code execution and full system compromise.
<br />
<br />  
<img src="https://i.imgur.com/6qDNiRI.png" alt="Remote Code"/>
<br />
<br />   
Next is CVE-2014-3566, also known as the POODLE vulnerability, which affects SSL 3.0, a version of the security protocol used to encrypt data over the internet. In SSL 3.0, there is a flaw in how it handles padding (extra data added to messages to make them fit a specific size). This flaw allows an attacker to intercept encrypted data and, through a technique called a "padding oracle attack," gradually figure out the original, unencrypted data (cleartext).
<br />
<br /> 
This vulnerability makes it easier for attackers to steal sensitive information, such as login credentials, by exploiting the weakness in how SSL 3.0 handles encryption.
<br />
<br /> 
<img src="https://i.imgur.com/zo1yOH9.png" alt="SSL Poodle"/>
<br />
<br />  
CVE-2014-0224, the CCS Injection vulnerability, affects older versions of OpenSSL. It allows attackers to trick systems into using a weak encryption key by modifying a specific message, the ChangeCipherSpec (CCS) message, during communication. This can allow attackers to hijack the session and steal sensitive information.  
<br />
<br /> 
<img src="https://i.imgur.com/jcjA1t0.png" alt="CCS Injection"/>
<br />
<br />   
CVE-2007-6750 is a vulnerability in the Apache HTTP Server. It allows attackers to overload the server by sending incomplete or "slow" HTTP requests, which can keep the server waiting and cause it to crash. This happens because older versions of Apache lack a feature (mod_reqtimeout) that would stop these types of requests. This can lead to a denial of service, meaning the server becomes unavailable to legitimate users.
<br />
<br />   
<img src="https://i.imgur.com/5loHs8h.png" alt="Slowloris"/>
 
<h2>Key takeaways:</h2>
The project began by installing Nmap on a Windows 10 virtual machine to perform a comprehensive network scan. The target system for the scan was Metasploitable 2, a deliberately vulnerable virtual machine designed for testing. Basic Nmap scans, including TCP Connect and SYN scans, were executed to identify open ports and assess potential entry points. These scans provided a foundational understanding of the system's surface area for potential attacks.
<br/>
<br/>
After performing basic scans, more advanced techniques were utilized, such as OS detection and scanning specific ports, to gather deeper insights into the target system. Specific ports like SMTP, DNS, and HTTP were scanned to understand the services running and their potential vulnerabilities. To further enhance the scanning process, obfuscation techniques were used by employing decoy IP addresses to avoid detection while scanning, along with a Zenmap GUI demo for a user-friendly interface to visualize results.
<br/>
<br/>
The project also involved running a comprehensive vulnerability scan using the Nmap Scripting Engine (NSE). This allowed for a more detailed exploration of vulnerabilities, such as CVEs, and provided critical information about weaknesses in the target system. A few of the identified vulnerability were researched to understand their impact and severity, contributing to a deeper understanding of how attackers might exploit these issues.
<br/>
<br/>
In an enterprise environment, the steps taken in this project could strengthen the overall security posture by identifying vulnerabilities that could potentially be exploited by attackers. By using Nmap’s extensive scanning capabilities, an organization can proactively assess their systems, prioritize vulnerabilities based on severity, and reduce their attack surface. Regular vulnerability assessments like these help minimize the risk of cyberattacks, ensuring that potential weaknesses are addressed before they can be exploited, ultimately strengthening security defenses across the organization. 
<br/>
<br/>
In conclusion, this project highlights the importance of network scanning, vulnerability detection, and prioritization in maintaining a secure enterprise environment. By utilizing tools like Nmap, performing thorough scans, and researching vulnerabilities, I gained valuable insights into effective security practices to enhance an organization’s defense mechanisms and reduce exposure to potential threats.

<p align="center">
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
