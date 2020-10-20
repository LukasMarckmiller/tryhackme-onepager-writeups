TryHackMe Boiler CTF

Montag, 19. Oktober 2020

13:21

 

# Flags:

<table><thead><tr class="header"><th>#</th><th>Found</th><th>Entry</th></tr></thead><tbody><tr class="odd"><td>User Flag</td><td>.secret</td><td>$ssh with user basterd</td></tr><tr class="even"><td>Root Flag</td><td>Root.txt</td><td>SUID privilege escalation in $find</td></tr></tbody></table>

#  

# Host Discovery:

 

<table><thead><tr class="header"><th></th><th>Scanned</th><th>Done</th></tr></thead><tbody><tr class="odd"><td>IP#1 (Dynmaic IP)</td><td><blockquote><p>yes </p></blockquote></td><td><blockquote><p>yes </p></blockquote></td></tr></tbody></table>

 

<table><thead><tr class="header"><th><p>IP#1</p><p>&lt;&lt;boilerctf.xml&gt;&gt;</p></th></tr></thead><tbody><tr class="odd"><td><table><thead><tr class="header"><th>Port</th><th>Service</th><th>Software/<br />
Version</th><th>Credentials</th><th>Done</th><th><p>Flag/Step</p><p>Solution</p></th><th>Misc</th></tr></thead><tbody><tr class="odd"><td>21</td><td>FTP</td><td>vsftpd 3.0.3</td><td><table><thead><tr class="header"><th>Username</th><th>Anonymous</th></tr></thead><tbody><tr class="odd"><td>Password</td><td>-</td></tr></tbody></table></td><td>yes </td><td><p>Found .info.txt</p><p>"Whfg jnagrq gb frr vs lbh svaq vg. Yby. Erzrzore: Rahzrengvba vf gur xrl!" -&gt; (Vigenere Decrypt)</p><p> </p><p>Just wanted to see if you find it. Lol. Remember: Enumeration is the key!</p><p> </p><p><em>Aus &lt;<a href="https://www.dcode.fr/vigenere-cipher">https://www.dcode.fr/vigenere-cipher</a>&gt;</em></p><p> </p><p> </p></td><td><p>Found .info.txt</p><p> </p></td></tr><tr class="even"><td>80</td><td>HTTP</td><td>Apache httpd 2.4.18 ((Ubuntu))</td><td> </td><td>yes </td><td><p>Found /joomla CMS</p><p>Found log.txt by Vuln#1</p><p>Found SSH Credentials</p><p> </p></td><td> </td></tr><tr class="odd"><td>10000</td><td>HTTPS</td><td>MiniServ 1.930 (Webmin httpd)</td><td><table><thead><tr class="header"><th>Username</th><th>???</th></tr></thead><tbody><tr class="odd"><td>Password</td><td>???</td></tr></tbody></table></td><td>yes </td><td> </td><td> </td></tr><tr class="even"><td>55007</td><td>SSH</td><td>OpenSSH 7.2p2 Ubuntu 4ubuntu2.8</td><td><table><thead><tr class="header"><th>Username</th><th>basterd</th></tr></thead><tbody><tr class="odd"><td>Password</td><td>superduperp@$$</td></tr></tbody></table><p> </p><table><thead><tr class="header"><th>Username</th><th>stoner</th></tr></thead><tbody><tr class="odd"><td>Password</td><td>superduperp@$$no1knows</td></tr></tbody></table></td><td>yes </td><td><p>Found .secret in /home/basterd/.secret</p><p> </p><p>Found root.txt in /root/txt</p></td><td> </td></tr></tbody></table></td></tr></tbody></table>

 

<table><thead><tr class="header"><th>80</th></tr></thead><tbody><tr class="odd"><td><table><thead><tr class="header"><th>Path</th><th>Contents</th><th>Misc</th></tr></thead><tbody><tr class="odd"><td>/robots.txt</td><td><p>User-agent: *Disallow: /</p><p>/tmp/.ssh/yellow/not/a+rabbit/hole/or/is/it079 084 108 105 077 068 089 050 077 071 078 107 079 084 086 104 090 071 086 104 077 122 073 051 089 122 085 048 077 084 103 121 089 109 070 104 078 084 069 049 079 068 081 075</p></td><td> </td></tr><tr class="even"><td>/joomla</td><td><p>/index.php,</p><p>/_archive (Status: 301)</p><p>/_database (Status: 301)</p><p>/_files (Status: 301)</p><p>/_test (Status: 301)</p><p>/~www (Status: 301)</p><p>/administrator (Status: 301)</p><p>/bin (Status: 301)</p><p>/build (Status: 301)</p><p>/cache (Status: 301)</p><p>/components (Status: 301)</p><p>/images (Status: 301)</p><p>/includes (Status: 301)</p><p>/index.php (Status: 200)</p><p>/installation (Status: 301)</p><p>/language (Status: 301)</p><p>/layouts (Status: 301)</p><p>/libraries (Status: 301)</p><p>/media (Status: 301)</p><p>/modules (Status: 301)</p><p>/plugins (Status: 301)</p><p>/templates (Status: 301)</p><p>/tests (Status: 301)</p><p>/tmp (Status: 301)</p></td><td> </td></tr><tr class="odd"><td>/manual</td><td> </td><td> </td></tr><tr class="even"><td>/joomla/_test</td><td><table><thead><tr class="header"><th><blockquote><p> </p></blockquote></th><th>CVE</th><th>PoC</th><th>Exploit</th></tr></thead><tbody><tr class="odd"><td><p>Vuln#1 -</p><p> </p></td><td>-</td><td><p>will execute<br />
the command you entered. After command injection press "select # host" then your command's<br />
output will appear bottom side of the scroll screen.</p><p> </p><p><em>Aus &lt;<a href="https://www.exploit-db.com/exploits/47204">https://www.exploit-db.com/exploits/47204</a>&gt;</em></p><p> </p><p>RESULT of log.txt</p><p>Aug 20 11:16:26 parrot sshd[2443]: Server listening on 0.0.0.0 port 22.</p><p>Aug 20 11:16:26 parrot sshd[2443]: Server listening on :: port 22.</p><p>Aug 20 11:16:35 parrot sshd[2451]: Accepted password for basterd from 10.1.1.1 port 49824 ssh2 #pass: superduperp@$$</p><p>Aug 20 11:16:35 parrot sshd[2451]: pam_unix(sshd:session): session opened for user pentest by (uid=0)</p><p>Aug 20 11:16:36 parrot sshd[2466]: Received disconnect from 10.10.170.50 port 49824:11: disconnected by user</p><p>Aug 20 11:16:36 parrot sshd[2466]: Disconnected from user pentest 10.10.170.50 port 49824</p><p>Aug 20 11:16:36 parrot sshd[2451]: pam_unix(sshd:session): session closed for user pentest</p><p>Aug 20 12:24:38 parrot sshd[2443]: Received signal 15; terminating.</p><p> </p><p> </p></td><td><table><thead><tr class="header"><th>ExploitDB</th><th> </th></tr></thead><tbody><tr class="odd"><td>ID</td><td>47204</td></tr><tr class="even"><td>Language</td><td>PHP</td></tr><tr class="odd"><td>Name</td><td>Sar2HTML 3.2.1 RCE</td></tr></tbody></table></td></tr></tbody></table></td><td> </td></tr><tr class="odd"><td>/joomla/_files</td><td>Bas64Decode() -&gt; Woopy Daisy</td><td> </td></tr></tbody></table></td></tr></tbody></table>

 
