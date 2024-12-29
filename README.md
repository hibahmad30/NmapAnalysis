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
To prepare the CSV file for analysis, use Excel to apply additional formatting, such as expanding columns, changing header colors, bolding the header text, enabling text wrapping, and turning on filters for the headers. I also renamed the headers for better clarity: 'Name' to 'Vulnerability Title', 'Synopsis' to 'Vulnerability Description', 'Description' to 'Plugin Description', and 'Solution' to 'Vulnerability Solution'.
 <br/>
 <br/>
<img src="https://i.imgur.com/medb6YU.png" alt="Format CSV File"/>
<br />
<br />
The first pivot table summarizes the number of Critical, High, Medium, and Low vulnerabilities for the Metasploitable host, along with the grand total of vulnerabilities. If the scan were conducted against multiple hosts, this pivot table could be used to easily compare the total number of vulnerabilities by risk level for each host. By double-clicking on any risk level, an additional Excel sheet is generated, providing further details on the specific vulnerabilities within that risk level.
<br />
<br />
<img src="https://i.imgur.com/BSxWNBu.png" alt="Asset IP Pivot Table"/>
<br />
<br />
I then created a second pivot table that lists each vulnerability title, along with its associated risk level and the number of hosts impacted. In this specific use case, there is only one host, however multiple vulnerabilities with distinct CVEs share the same vulnerability title. This pivot table is particularly useful for vulnerability management, as it allows for easy identification of vulnerabilities by title and risk level, making it simpler to prioritize remediation efforts. It also provides insight into the level of impact, highlighting which vulnerabilities affect a larger number of hosts and helping guide the allocation of resources for mitigation.
<br />
<br />
<img src="https://i.imgur.com/nwNIN58.png" alt="Risk by Vulnerability Pivot Table"/>

<h2>Vulnerability Analysis:</h2> 
 <p align="center">
CVEs (Common Vulnerabilities and Exposures) are unique identifiers assigned to publicly known security vulnerabilities in software, hardware, or firmware, enabling standardized tracking and sharing across platforms. Each CVE provides a brief description and references to relevant fixes or patches. 
<br />
<br />  
The NIST NVD (National Vulnerability Database) is a comprehensive resource managed by the National Institute of Standards and Technology, offering detailed information on CVEs, including severity ratings and potential mitigations. In this project, we will explore some of the vulnerabilities found in our scan by referencing their CVEs in the NVD to analyze their impact and remediation strategies.
<br />
<br />
Starting with CVE-2003-1567, the vulnerability details can be found on the NIST NVD at the following link: https://nvd.nist.gov/vuln/detail/CVE-2003-1567. 
<br />
<br />
 <img src="https://i.imgur.com/wTNh2vc.png" height="70%" width="70%" alt="NIST NVD CVE Details"/>
<br />
<br />
CVE-2003-1567 is a security flaw in an older version of Microsoft Internet Information Services (IIS), which is used to manage websites. Rapid7's inclusion of Microsoft IIS on Metasploitable simulates real-world scenarios where outdated or poorly configured web servers become targets for attacks. This flaw involves a feature called the "TRACK" method, which is intended to track certain actions on a website. 
<br />
<br />  
However, when used, it can accidentally reveal sensitive information like login details or cookies (small pieces of data that remember a user's actions on a website). Attackers can exploit this flaw to steal such information or bypass security measures, making it easier for them to access user accounts or sensitive data without permission. To prevent this, website administrators should disable unused features like the "TRACK" method to improve security.
<br />
<br />
Next is CVE-2008-0166, a critical vulnerability related to insecure remote SSH host keys. 
<br />
<br />
<img src="https://i.imgur.com/lghZE79.png" height="70%" width="70%" alt="NIST NVD CVE Vulnerability"/>
<br />
<br />
This vulnerability involves an outdated version of OpenSSL, a tool used to secure online communications. The problem is that the random number generator it uses to create secure keys is not truly random, meaning the numbers can be predicted by attackers. 
<br />
<br />  
If attackers can predict these numbers, they can attempt to guess the cryptographic keys used to secure data through a method called brute force, where they try many possible combinations until they find the correct one. This makes it easier for attackers to break the encryption and access sensitive information. To prevent this, it's important to use a secure, updated method for generating cryptographic keys.  
<br />
<br />
When prioritizing vulnerabilities for remediation, it's important to focus on those that pose the greatest risk to your system first. Start with critical vulnerabilities that are easy to exploit or affect important systems. These are often high-impact issues that could allow attackers to gain access to sensitive information or disrupt services. 
<br />
<br />  
Next, consider vulnerabilities with a high likelihood of being exploited, such as those that are widely known or already being targeted. By addressing the most severe and likely-to-be-exploited vulnerabilities first, you reduce the chances of a successful attack and improve your overall security posture. Prioritizing helps ensure that resources are used effectively to minimize risk. 
<br />
<br />  
Tenable provides documentation on prioritizing vulnerabilities based on certain metrics, such as the Common Vulnerability Scoring System, or CVSS, which helps assess the severity of vulnerabilities. To learn more about how to use CVSS for prioritization, navigate to the following guide by Tenable: https://docs.tenable.com/cyber-exposure-studies/application-software-security/Content/VulnerabilitiesCVSS.htm. 
 
<h2>Key takeaways:</h2>
The project began by installing Nessus on a Windows 10 virtual machine to perform the vulnerability assessment. The target system for the scan was Metasploitable, a purposely vulnerable VM designed for testing. Both credentialed and non-credentialed scans were configured and executed to evaluate the vulnerabilities present on the target system.
<br/>
<br/>
After the scans were completed, the results were exported in CSV format and imported into Excel for further analysis. The Excel file was then formatted to improve readability by expanding columns, applying filters, and creating pivot tables. These tables helped summarize and organize the vulnerabilities based on their risk levels and the systems they affected.
<br/>
<br/>
The project included researching specific vulnerabilities by referencing CVE entries and CVSS scores. This research provided a deeper understanding of the vulnerabilities, helping to evaluate their severity and potential impact. This was an important step in determining which vulnerabilities posed the highest risk to the system.
<br/>
<br/>
Vulnerability management is a critical process in cybersecurity that helps identify, assess, and mitigate security risks. Regular scans and effective management of vulnerabilities ensure that weaknesses in a system are addressed before they can be exploited by attackers. By following a structured approach to vulnerability management, organizations can minimize their exposure to threats.
<br/>
<br/>
Prioritization is a key aspect of vulnerability management. Vulnerabilities should be prioritized based on factors such as their severity, ease of exploitation, and the potential impact they could have. CVE and CVSS scores play a significant role in helping to rank vulnerabilities, ensuring that the most critical issues are dealt with first.
<br/>
<br/>
In conclusion, this project highlighted the importance of vulnerability scanning, prioritization, and research in securing systems. By utilizing Nessus and examining CVE entries along with CVSS scores, I learned that effective vulnerability management involves a structured approach and informed decision-making to mitigate risks and strengthen overall security.


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
