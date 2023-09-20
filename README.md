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

<i><b>Offensive Security</b></i> was contracted by Marketplace industry to conduct a penetration test in order to determine its exposures to a targeted attack. All activities were conducted in a manner that simulated a malicious actor engaged in a targeted attack against Marketplace industry.<br>

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
    <td>25-05-22</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Trying XSS in Website</td>
    <td>XSS</td>
    <td>High</td>
    <td>26-05-22</td>
  </tr>
  <tr>
    <td>3</td>
    <td>Successful stored XSS</td>
    <td>XSS</td>
    <td>High</td>
    <td>27-06-22</td>
  </tr>
  <tr>
    <td>4</td>
    <td>Session Hijacking with XSS</td>
    <td>Session Hijacking</td>
    <td>Extreme</td>
    <td>28-06-22</td>
  </tr>
  <tr>
    <td>5</td>
    <td>Successful Privilege Escalation to Admin Account</td>
    <td>Session Hijacking</td>
    <td>Extreme</td>
    <td>29-06-22</td>
  </tr>
  <tr>
    <td>6</td>
    <td>Information Exposure in URL</td>
    <td>Forced browsing</td>
    <td>High</td>
    <td>29-06-22</td>
  </tr>
   <tr>
    <td>7</td>
    <td>Trying SQL injection in URL</td>
    <td>SQL Injection</td>
     <td>High</td>
     <td>30-06-22</td>
  </tr>
   <tr>
    <td>8</td>
    <td>Successful SQL injection in URL</td>
    <td>SQL Injection</td>
     <td>Moderate</td>
     <td>31-06-22</td>
  </tr>
   <tr>
    <td>9</td>
    <td>Exploit Database, tables, columns and data</td>
     <td>SQL Injection</td>
    <td>Moderate</td>
     <td>31-06-22</td>
  </tr>
   <tr>
    <td>10</td>
    <td>Discover in table: Messages, in message content, a SSH password</td>
    <td>SQL Injection</td>
     <td>Extreme</td>
     <td>01-06-22</td>
  </tr>
   <tr>
    <td>11</td>
    <td>SSH Connection</td>
    <td>Command Injection</td>
     <td>High</td>
     <td>02-06-22</td>
  </tr>
  <tr>
    <td>12</td>
    <td>System file checking</td>
    <td>-</td>
     <td>-</td>
     <td>02-06-22</td>
  </tr>
   <tr>
    <td>13</td>
    <td>Found a Tar wildcard file</td>
    <td>-</td>
     <td>-</td>
     <td>02-06-22</td>
   </tr>
   <tr>
    <td>14</td>
    <td>Exploit tar wildcard and Privilege Escalation</td>
    <td>Privilege Escalation</td>
     <td>Moderate</td>
     <td>03-06-22</td>
  </tr>
   <tr>
    <td>15</td>
    <td>System file checking</td>
    <td>-</td>
     <td>-</td>
     <td>03-06-22</td>
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
     <td>05-06-22</td>
  </tr>
   <tr>
    <td>18</td>
    <td>Analyze Findings</td>
    <td>-</td>
     <td>-</td>
     <td>06-06-22</td>
  </tr>
   <tr>
    <td>19</td>
    <td>Report</td>
    <td>-</td>
     <td>-</td>
     <td>07-06-22</td>
  </tr>
</table>



