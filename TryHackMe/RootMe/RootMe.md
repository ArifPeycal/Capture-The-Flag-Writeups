# RootMe

<p align="center">
  <img width="80%" height="200" src="assets/header.png">
</p>

### Task 1: Deploy the machine

Power up OpenVpn on your machine or use the AttackBox provided by the TryHackMe room. 
<p align="center">
  <img width="80%" height="200" src="assets/Task1.PNG">
</p>

### Task 2: Reconnaissance

Now, we are going to use <b>Nmap</b> for port enumeration and scanning.
```
nmap -T4 -sV {ip_address} 
```
* -T4 is also known as aggressive scan mode and used to scan more quickly
* -sV allows the user to collect version information about the port
* <b>{ip_address}</b> can be found using <b>ifconfig</b> on Linux Terminal
<p align="center">
  <img width="80%" height="200" src="assets/rootme1.PNG">
</p>
From this Nmap scan, we can answer several questions

<b>2.1 Scan the machine, how many ports are open? </b>
* 2
<b>2.2 What version of Apache is running? </b>
*2.4.49
<b>2.3 What service is running on port 22? </b>
* ssh

Next, we need to use <b>Gobuster</b> tool to find the hidden directories. 
```
gobuster dir -w {wordlist_path} -u http://{ip_address}
```
* <b>{wordlist_path}</b> can be found in Kali Linux directory, (usr/share/wordlists). There are many common-used wordlists such as common.txt, rockyou.txt and more. You can also search for wordlist text files online.

<p align="center">
  <img width="80%" height="400" src="assets/rootme2.PNG">
</p>

There are several hidden directories that are interesting to check out which are /panel and /uploads

<b>2.4 What is the hidden directory? </b>
* /panel

### Task 3: Getting a shell

We'll check the /panel directory first.
<p align="center">
  <img width="80%" height="400" src="assets/rootme4.PNG">
</p>

The /panel directory requires us to upload a file. The question has given us some hints about where we need to upload PHP reverse shell to the web. 

