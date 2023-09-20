<h2> Penetration Test Report - Example</h2> 
<h3>The Marketplace's infrastructure </h3>

<h3> Table of Contents </h3>
<ol type="1">
  <li><b>Executive Summary</b></li>
  <ol type="a">
    <li>Brief summary of findings</li>
    <li>Timeline</li>
    <li>Summary of the test scope</li>
    <li>Testing methodology used and Summary of metrics and measures, including the number of findings, listed by severity level</li>
    <li>Objectives of the testing effort, including the main topic or purpose of the test and report</li>
  </ol>
  <li><b>Statement of Scope</b></li>
  <li><b>Statement of Methodology</b></li>
  <ol type="a">
    <li>Reference used methodology</li>
    <li>Open Web Application Security Project (OWASP) Testing Project</li>
  </ol>
  <li><b></b>Statement of Limitations</b></li>
  <li><b>Testing Narrative</b></li>
  <li><b>Findings</b></li>
  <ol type="a">
    <li>Calculating and documenting the risks of the vulnerabilities, Common Vulnerability Scoring system, CVSS.</li>
    <li>OWASP Risk Rating Methodology for estimating the risk of a vulnerability</li>
  </ol>
  <li><b>Findings and recommendations for remediation</b></li>
   <ol type="a">
    <li>Technical details necessary for IT, information security, and development teams to use to address the issues.</li>
  </ol>
  <li><b>Conclusion</b></li>
</ol>  
<br>
</br>

1. <b> Executive Summary </b><br>

<i><b>Ethical Security</b></i> company was contracted by Marketplace industry to conduct a penetration test in order to determine its exposures to a targeted attack. All activities were conducted in a manner that simulated a malicious actor engaged in a targeted attack against Marketplace industry.<br>

<b>Target goals : </b>
<ul>
  <li>Identifying if a remote attacker could penetrate Marketplace industry’s defenses </li>
  <li>The effectiveness of an organization's security policy.</li>
  <li>Confidentiality of the company’s private data.</li>
  <li>Internal infrastructure and availability of Marketplace’s information systems.</li>
  <li>Reduce risk of attack: Mimicking real hacker behaviors provides greater visibility into organization’s weakness.</li>
  <li>Backdoors in the operating system</li>
</ul>  

Efforts were placed on the identification and exploitation of security weaknesses that could allow a remote attacker to gain unauthorized access to organizational data. The attacks were conducted with the level of access that a general Internet user would have. The assessment was conducted in accordance with the recommendations outlined in NIST SP 800-1151 with all tests and actions being conducted under controlled conditions. <br><br>
a.	<b>Brief Summary of Findings</b><br><br>
Initial reconnaissance of the <b>Marketplace’s</b> network resulted in the discovery of a few open TCP ports such as 22/TCP SSH Version: openSSH 7.6p1 Ubuntu0.3 and 80/TCP HTTP-Server-title: The Marketplace.<br><br>
Steps:<br> (1) An examination of these ports revealed a <b>Cross Site Scripting (XSS), SQL Injection and Command Injection via SSH (Secure Shell) connection </b> vulnerabilities. Also, a <b>docker and a tar wildcard privilege escalation</b> vulnerability.<br> (2) After exploiting Marketplace’s webpage, we were able to gain access to an administration account with <b>session hijacking through XSS</b>.<br> (3) In addition, gaining access to the admin account we discovered that there is an <b>information exposure through query strings in URL</b>.<br> (4) The next move was to use <b>SQL injection commands in URL with Burp Suite tool</b> to exploit the database and take information. With the database’s exploitation we found database’s tables: <b> Users, usernames and passwords hashes and cracked them with John the Ripper tool </b>, also we found through database’s table:<b> <i>Messages</i>, a message to a user from the administrator/system for a SSH password change and the new password </b>.<br> (5) We continue with a <b>SSH connect to the specific user with the provided SSH password and we gain access to user’s system</b> with remote access through SSH connection.<br> (6) Finally, we <b>discovered a tar backup.tar * file in the user’s files and we exploit wildcard for privilege escalation to another user</b>. As we gain access to another user and found that he is <b>privileged to use docker, we exploit it with shell to escalate our privileges to root shell</b>.

b. <b>Timeline</b>

<table>
  <tr>
    <th>Steps</th>
    <th>Description</th>
    <th>Vulnerability</th>
    <th>Risk Rating</th>
    <th>Timeline</th>
  </tr>
  <tr>
    <td>1</td>
    <td>Active Reconnaissance</td>
    <td>Open Ports</td>
    <td>Moderate</td>
    <td>25-06-22</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Trying XSS in Website</td>
    <td>XSS</td>
    <td>High</td>
    <td>26-06-22</td>
  </tr>
  <tr>
    <td>3</td>
    <td>Successful stored XSS</td>
    <td>XSS</td>
    <td>High</td>
    <td>26-06-22</td>
  </tr>
  <tr>
    <td>4</td>
    <td>Session Hijacking with XSS</td>
    <td>Session Hijacking</td>
    <td>Extreme</td>
    <td>27-06-22</td>
  </tr>
  <tr>
    <td>5</td>
    <td>Successful Privilege Escalation to Admin Account</td>
    <td>Session Hijacking</td>
    <td>Extreme</td>
    <td>28-06-22</td>
  </tr>
  <tr>
    <td>6</td>
    <td>Information Exposure in URL</td>
    <td>Forced browsing</td>
    <td>High</td>
    <td>28-06-22</td>
  </tr>
   <tr>
    <td>7</td>
    <td>Trying SQL injection in URL</td>
    <td>SQL Injection</td>
     <td>High</td>
     <td>29-06-22</td>
  </tr>
   <tr>
    <td>8</td>
    <td>Successful SQL injection in URL</td>
    <td>SQL Injection</td>
     <td>Moderate</td>
     <td>30-06-22</td>
  </tr>
   <tr>
    <td>9</td>
    <td>Exploit Database, tables, columns and data</td>
     <td>SQL Injection</td>
    <td>Moderate</td>
     <td>30-06-22</td>
  </tr>
   <tr>
    <td>10</td>
    <td>Discover in table: Messages, in message content, a SSH password</td>
    <td>SQL Injection</td>
     <td>Extreme</td>
     <td>30-06-22</td>
  </tr>
   <tr>
    <td>11</td>
    <td>SSH Connection</td>
    <td>Command Injection</td>
     <td>High</td>
     <td>31-06-22</td>
  </tr>
  <tr>
    <td>12</td>
    <td>System file checking</td>
    <td>-</td>
     <td>-</td>
     <td>31-06-22</td>
  </tr>
   <tr>
    <td>13</td>
    <td>Found a Tar wildcard file</td>
    <td>-</td>
     <td>-</td>
     <td>01-07-22</td>
   </tr>
   <tr>
    <td>14</td>
    <td>Exploit tar wildcard and Privilege Escalation</td>
    <td>Privilege Escalation</td>
     <td>Moderate</td>
     <td>02-07-22</td>
  </tr>
   <tr>
    <td>15</td>
    <td>System file checking</td>
    <td>-</td>
     <td>-</td>
     <td>02-06-22</td>
  </tr>
   <tr>
    <td>16</td>
    <td>Found user using Docker</td>
    <td>-</td>
     <td>-</td>
     <td>03-06-22</td>
  </tr>
   <tr>
    <td>17</td>
    <td>Exploit Docker and Privilege Escalation to root</td>
    <td>Privilege Escalation</td>
     <td>Extreme</td>
     <td>03-06-22</td>
  </tr>
   <tr>
    <td>18</td>
    <td>Analyze Findings</td>
    <td>-</td>
     <td>-</td>
     <td>04-06-22</td>
  </tr>
   <tr>
    <td>19</td>
    <td>Report</td>
    <td>-</td>
     <td>-</td>
     <td>07-06-22</td>
  </tr>
</table>

c.	<b>Summary of the test Scope</b><br>

The scope of this Penetration Test was limited to a single internet facing web application portal. This is a Marketplace application and the specific instantiation of the portal we were asked to test, was the vulnerabilities we could find. Also, we will perform a <b>black-box penetration testing</b> (pentesting) as external attackers-testers aiming at identifying vulnerabilities in the application and show exactly how. The landing page to the application under review was at the following address:
<table>
  <tr>
    <th>Application	Authentication</th>
    <th> Landing Page</th>
  </tr>
  <tr>
    <td>The Marketplace Webpage</td>
    <td>https://marketplace.com</td>
  </tr>
</table>
<br>
d.	<b>Testing Methodology Used and Summary of metrics and measures, including the number of findings, listed by severity level</b><br><br>
<b>Firstly</b>, we used the <b>cyber kill chain</b> methodology, which is essentially a Lockheed Martin cybersecurity model that traces the stages of a cyber-attack, detects vulnerabilities, and assists security teams in stopping attacks at each point of the chain. The term kill chain was borrowed from the military, which refers to the framework of an attack. It consists of recognizing a target, dispatching it, making a judgment, issuing an order, and eventually destroying the target.

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/78affee0-4ef8-4f9a-8eb2-589a8cdd35a9)

<b>Lockheed Martin’s original cyber kill chain model contained seven sequential steps:</b><br><br>
<b>Phase 1: Reconnaissance</b><br>
During the Reconnaissance phase, we scanned the Marketplace website with nmap tool and explored vulnerabilities and weaknesses that could be exploited within the network. <br>
<b>Phase 2: Weaponization</b>
During the Weaponization phase, we created an attack vector, such as remote access, XSS, SQL Injection or Command Injection that can exploit a known vulnerability.<br> 
<b>Phase 3: Delivery</b><br>
In the Delivery step, we launched the attacks. <br>
<b>Phase 4: Exploitation</b><br>
In the Exploitation phase, the malicious code is executed within the victim’s system.<br>
<b>Phase 5: Installation</b><br>
Immediately following the Exploitation phase, the malware or other attack vector will be installed on the victim’s system.<br>
<b>Phase 6: Command and Control</b><br>
In Command & Control, we have been able to use the exploits to assume remote control of a device or identity within the target network. <br>
<b>Phase 7: Actions on Objective</b><br>
In this stage, we took root privileges.<br><br>
We also used Penetration Testing Phases from NIST SP 800-115. In this process, we used the information gathered during the discovery phase to gain initial access to a system. Once we establish a foothold, we manage to escalate our access until we gain complete administrative control of the system. 

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/a4f196b9-1a0b-41c0-aed7-293cf685fd4e)

<b>Metrics and Measures</b><br><br>
<b>Secondly</b>, we used the OWASP, or Open Web Application Security Project, which is a set of standards and guidelines for the security of web applications, and is often the starting point for IT personnel when initially venturing into the realm of penetration testing. OWASP provides several resources on its own to improve the security posture of both internal and external web applications by providing companies with a comprehensive list of vulnerability categories for web applications, as well as ways to mitigate or remediate them.<br><br>
<b>The OWASP Top 10</b> is a standard awareness document for developers and web application security. It represents a broad consensus about the most critical security risks to web applications.<br>
<table>
  <tr>
    <th>No.</th>
    <th>Vulnerability</th>
    <th>Severity</th>
    <th>OWASP -Metrics and Measures </th>
  </tr>
  <tr>
    <td>1</td>
    <td>SQL Injection</td>
    <td>Extreme</td>
    <td>https://owasp.org/www-community/attacks/SQL_Injection</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Cross Site Scripting</td>
    <td>Extreme</td>
    <td>https://owasp.org/www-community/attacks/xss/</td>
  </tr>
  <tr>
    <td>3</td>
    <td>Command Injection</td>
    <td>High</td>
    <td>https://owasp.org/www-community/attacks/Command_Injection</td>
  </tr>
  <tr>
    <td>4</td>
    <td>Privilege Escalation</td>
    <td>Extreme</td>
    <td>https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation</td>
  </tr>
  <tr>
    <td>5</td>
    <td>Docker</td>
    <td>High</td>
    <td>https://owasp.org/www-project-docker-top-1t0/</td>
  </tr>
  <tr>
    <td>6</td>
    <td>Tar - Wildcard</td>
    <td>Moderate</td>
    <td>https://owasp.org/www-pdf-archive/OWASP_Testing_Guide_v3.pdf</td>
  </tr>
</table>

e.	<b>Objectives of the testing effort, including the main topic or purpose of the test and report</b><br><br>
The practice of analyzing and validating Marketplace application accomplishes what it is designed to do is known as software testing. The advantages of this testing include bug prevention, reduced development costs, and improved performance. The sooner development teams receive test feedback, the sooner they can address issues such as:
<ul>
  <li>Architectural flaws </li>
  <li>Poor design decisions</li>
  <li>Invalid or incorrect functionality</li>
  <li>Security vulnerabilities</li>
  <li>Scalability issues</li>
</ul>  
Penetration Testing is done in order to assess its vulnerability to a targeted attack. All operations were carried out in a way that imitated a malicious actor carrying out a targeted attack on the <b>Marketplace industry</b>.
After the Penetration, team members can share status, goals, and test results thanks to reporting and analytics. Advanced tools incorporate project metrics and display the results in a dashboard. Teams can rapidly evaluate a project's overall health and monitor interactions between test, development, and other project parts.<br><br>
2.	<b>Statement of Scope</b><br>
We performed an assessment of the Ethical Security Company Services in Greece in order to provide detailed analysis of: identification of application, system and network vulnerabilities; gaps in IT security governance; assessment of patching methodologies; current network security capabilities and potential existing security incidents. The assessment and reporting will be based on the NIST 800-53 MODERATE security controls.<br><br>

<table>
  <tr>
    <th>Criteria</th>
    <th>Response</th>
  </tr>
  <tr>
    <td>Project Title</td>
    <td>Penetration Testing Marketplace's Website</td>
  </tr>
  <tr>
    <td>Project Scope Description</td>
    <td>Web Application – The Marketplace</td>
  </tr>
  <tr>
    <td>Date Prepared</td>
    <td>07 June 2022</td>
  </tr>
  <tr>
    <td>Prepared by:</td>
    <td>George Ziras</td>
  </tr>
  <tr>
    <td>Deadline for Questions</td>
    <td>17 June 2022</td>
  </tr>
  <tr>
    <td>Host names</td>
    <td>https://marketplace.com</td>
  </tr>
  <tr>
    <td>IP addresses:</td>
    <td>10.10.110.33</td>
  </tr>
  <tr>
    <td>Agency:</td>
    <td>Ethical Security Company Services</td>
  </tr>
  <tr>
    <td>Assessment Category:</td>
    <td>Risk Assessment</td>
  </tr>
  <tr>
    <td>Hours of Testing:</td>
    <td>00:00 – 06:00</td>
  </tr>
  <tr>
    <td>Number of Hours to Complete the Project:</td>
    <td>156 Hours</td>
  </tr>
  <tr>
    <td>Cost</td>
    <td>23.658 €</td>
  </tr>
   <tr>
    <td>Remediate Methodology: </td>
    <td>OWASP Top 10</td>
  </tr>
  <tr>
    <td>Attack Methodology:</td>
    <td>Cyber Kill Chain& Pentest Phases</td>
  </tr>
</table>

3.	<b>Statement of Methodology</b><br><br>
We used the <b>Open Web Application Security Project (OWASP)</b> framework that offers advice on how to create, buy, and maintain trustworthy and secure software applications. OWASP is well-known for its popular Top 10 list of web application security flaws. The OWASP Biggest 10 list of security vulnerabilities is based on developer community opinion on the top security dangers. Every few years, it is revised as hazards alter and new ones develop. <br>
Some of these Web security vulnerabilities have been found at Marketplace’s Web Application, such as XSS, SQL Injection and Command Injection.<br>
<b>Cross-Site Scripting (XSS)</b> attacks occur when:<br>
1.	Data enters a Web application through an untrusted source, most frequently a web request.
2.	The data is included in dynamic content that is sent to a web user without being validated for malicious content.
The malicious content sent to the web browser often takes the form of a segment of JavaScript, but may also include HTML, Flash, or any other type of code that the browser may execute.
SQL injection attack occurs when:
1.	An unintended data enters a program from an untrusted source.
2.	The data is used to dynamically construct a SQL query
Command injection is an attack in which the goal is execution of arbitrary commands on the host operating system via a vulnerable application. Command injection attacks are possible when an application passes unsafe user supplied data (forms, cookies, HTTP headers etc.) to a system shell. In this attack, the attacker-supplied operating system commands are usually executed with the privileges of the vulnerable application. Command injection attacks are possible largely due to insufficient input validation.
4. Statement of Limitations
Limitation of Time
When we performed penetration testing, we had a timeboxed assessment that needed to be completed in a predefined time period. The testing team has to identify potential threats and vulnerabilities, and produce results within this specified time period. This time of period was:

