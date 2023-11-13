# RootMe Writeup

<p align="center">
  <img width="80%" height="200" src="assets/header.png">
</p>
<br><br>

### Task 1: Deploy the machine

Power up OpenVpn on your machine or use the AttackBox provided by the TryHackMe room. 
<p align="center">
  <img width="80%" height="200" src="assets/Task1.PNG">
</p>
<br><br>

### Task 2: Reconnaissance

Now, we are going to use <b>Nmap</b> for port enumeration and scanning.
```
nmap -T4 -sV {ip_address} 
```
* -T4 is also known as aggressive scan mode and used to scan more quickly
* -sV allows the user to collect version information about the port
* <b>{ip_address}</b> can be found using <b>ifconfig</b> on Linux Terminal
<br>

<p align="center">
  <img width="80%" height="200" src="assets/rootme1.PNG">
</p>
From this Nmap scan, we can answer several questions:

<b>2.1 Scan the machine, how many ports are open? </b>
* 2
  
<b>2.2 What version of Apache is running? </b>
* 2.4.49
  
<b>2.3 What service is running on port 22? </b>
* ssh

Next, we need to use <b>Gobuster</b> tool to find the hidden directories. 
```
gobuster dir -w {wordlist_path} -u http://{ip_address}
```
* <b>{wordlist_path}</b> can be found in Kali Linux directory, (usr/share/wordlists). There are many common-used wordlists such as common.txt, rockyou.txt and more. You can also search for wordlist text files online.
<br>

<p align="center">
  <img width="80%" height="400" src="assets/rootme2.PNG">
</p>

There are several hidden directories that are interesting to check out which are /panel and /uploads

<b>2.4 What is the hidden directory? </b>
* /panel
<br><br>

### Task 3: Getting a shell

We'll check the /panel directory first.
<p align="center">
  <img width="80%" height="400" src="assets/rootme4.PNG">
</p>
<br> 

The /panel directory requires us to upload a file. The question has given us some hints about where we need to upload PHP reverse shell to the web. You can download the PHP reverse shell from the <a href="https://github.com/pentestmonkey/php-reverse-shell" target="_blank">PentestMonkey</a> Github repository. 

<p align="center">
  <img width="80%" height="250" src="assets/php.PNG">
</p>
<br>

After downloading the reverse shell, we need to change the IP address according to your machine and the port number to any number that you want (make sure that the port number had not been used by other services). 

<p align="center">
  <img width="80%" height="400" src="assets/rootme5.PNG">
</p>
<br>

Looks like it gives us an error when we want to upload the file. So, we can try to change the extension of the file from .php to other PHP extensions (.phtml, .php3, .php4, .php5). I try to change the extension into .php5 and it works. 

<p align="center">
  <img width="80%" height="400" src="assets/rootme7.PNG">
</p>
<br>

After you have successfully uploaded the reverse shell, we can start Netcat listener in our terminal. Make sure to use the same port number that you choose in the reverse shell.
```
nc -lvnp {port_no}
```
<br>

Now you have Netcat listening to the port number, and you need to find a way to execute the reverse shell that you had uploaded. Remember /uploads directory we get from the Gobuster tool? This is where we need to access that directory.
<p align="center">
  <img width="70%" height="300" src="assets/rootme8.PNG">
</p>
<br>

Click on the reverse shell that you uploaded. Then, go to the Netcat listener and you will have a shell running in Netcat terminal. 

<p align="center">
  <img width="70%" height="300" src="assets/rootme9.PNG">
</p>
<br>

In order to find user.txt, you can use this command: 

```
find -name user.txt
```
It will shows you the path and you can read the file using ```cat```

<b>3.1 user.txt</b>
* THM{***************"}
<br><br>

### Task 4: Privilege escalation

To find files that have SUID permissions, we can use this command:

```
find / -perm -u=s -type f 2>/dev/null
```

<p align="center">
  <img width="70%" height="300" src="assets/rootme11.PNG">
</p>
<br>

<b>4.1 Search for files with SUID permission, which file is weird?</b>
* /usr/bin/python

<b>4.2 Find a form to escalate your privileges.</b>

We can use Python to escalate the privileges to root. Refer to <a href="https://gtfobins.github.io" target="_blank">GTFOBins</a> website and find the command that can exploit SUID permissions using Python.

```
usr/bin/python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
```

<p align="center">
  <img width="70%" height="200" src="assets/rootme12.PNG">
</p>
<br>
FINALLY, WE GET TO THE ROOT!!!

Now you can find root.txt using ```find``` command:

```
find -name root.txt
```

<b>4.3 root.txt</b>
* THM{*******************}
