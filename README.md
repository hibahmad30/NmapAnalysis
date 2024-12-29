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

<h2>Network Configuration and Testing:</h2> 

<p align="center">
Next, we need to configure both VMs to communicate with each other to ensure the scan functions properly. In VirtualBox, navigate to 'File > Tools > Network Manager > NAT Networks', then right-click to create a new NAT network and ensure that DHCP is enabled.
 <br/> 
 <br/>
Next, in the Metasploitable VM, run the 'ifconfig' command and note the IP address located after 'inet addr':
 <br/> 
 <br/>
<img src="https://i.imgur.com/QBaGUZ7.png" alt="Metasploitable VM IP address"/>
 <br/> 
 <br/>
From the Windows 10 virtual machine, open PowerShell and run the 'ping' command followed by the IP address of the Metasploitable virtual machine to test connectivity.
 <br/> 
 <br/>
<img src="https://i.imgur.com/Qo22DYP.png" alt="Test Network Connection"/>
 <br/> 
 <br/>
Additionally, on the Windows 10 virtual machine, navigate to the following link to download Nessus Essentials: https://www.tenable.com/products/nessus/nessus-essentials. Optionally, you can run the following command in PowerShell to verify the SHA-256 hash of the downloaded file against the provided hash on Tenable's download page for Nessus Essentials.
 <br/> 
 <br/>
<img src="https://i.imgur.com/GmA24Bw.png" alt="Verify Hash"/>

<h2>Uncredentialed Scan:</h2> 
<p align="center">
In Nessus, click 'New Scan', then select 'Basic Network Scan'. Enter a name for the scan and provide the virtual machine's IPv4 address in the 'Targets' field. Nessus Essentials also allows the administrator to schedule scans at desired intervals and specify contacts to receive scan notifications. Once the scan is configured, click the 'Launch' icon to begin the scan.
<br />
<br />
<img src="https://i.imgur.com/27U9Nqw.png" alt="Nessus Scanner Page"/> 
<br />
<br />
The image below shows the results of the initial scan. Most of the vulnerabilities detected fall under the 'Info' severity, with several categorized as 'Medium' severity. Since the scan was performed without privileged credentials, it could not perform a deep analysis and missed many vulnerabilities. The next step is to conduct a credentialed scan for more comprehensive results.  
<br />
<br />
<img src="https://i.imgur.com/3ghN34P.png" alt="Uncredentialed Scan"/> 

<h2>Credentialed Scan:</h2> 
 <p align="center">
To perform a credentialed scan, the virtual machine must be configured to accept authenticated scans. In Nessus, select the previously created scan, click 'More', then 'Configure'. Navigate to the 'Credentials' tab located next to 'Settings'. Set the 'Authentication method' to 'Password', and enter the username and password for the target Metasploitable virtual machine. After saving the credentials, launch the scan. 
 <br/>
 <br/>
 <img src="https://i.imgur.com/W7IuPhL.png" alt="Credentialed Scan Configuration"/>
  <br/>
 <br/>
The image below shows the results of the credentialed scan, where the total number of vulnerabilities increased from 68 to 90: 
  <br/>
 <br/>
 <img src="https://i.imgur.com/7bVcbEM.png" alt="Credentialed Scan Vulnerabilities"/>
 <br/>
 <br/>
Here is a side-by-side comparison of the two scans, with the credentialed scan displayed on the right: 
  <br/>
 <br/>
 <img src="https://i.imgur.com/pZ7HRqC.png" height="35%" width="35%" alt="Uncredentialed Scan Results"/>     <img src="https://i.imgur.com/OALjpWr.png" height="35%" width="35%" alt="Credentialed Scan Results"/> 
  <br/>
 <br/>
For detailed analysis, click 'Generate Report', choose 'CSV', and select the desired columns. While Nessus also offers the option to download an HTML report, I chose CSV for the additional customization options: 
 <br/>
 <br/>
 <img src="https://i.imgur.com/QjDmAlT.png" alt="Generate Report"/>
  <br/>
 <br/>

<h2>Excel Data Processing:</h2> 
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
