# Source Writeup

<p align="center">
  <img width="80%" height="150" src="assets/header.PNG">
</p>
Source is a beginner-level room that can teach how to gather information, scan ports and directories and escalate privileges using various tools. This is my first writeup and certainly hope that it will be a motivation for me to keep on creating writeups for other rooms. 
<br><br>
Link to the room: <a href ='https://tryhackme.com/room/source'>https://tryhackme.com/room/source</a>
<br><br>
<br>

### Task 1: Enumeration 

Now, we are going to use <b>Nmap</b> for port enumeration and scanning.
```
nmap -T4 -sV {ip_address} 
```
* -T4 is also known as aggressive scan mode and used to scan more quickly
* -sV allows the user to collect version information about the port
* <b>{ip_address}</b> can be found using <b>ifconfig</b> on Linux Terminal
<br>
<p align="center">
  <img width="80%" height="300" src="assets/source12.PNG">
</p>
<br>

Informations from Nmap scan: 
* 2 open ports (port 22 and 10000)
* Port 22 is running SSH (OpenSSH 7.6p1)
* Port 10000 is running MiniServ 1.890 (Webmin httpd)

According to the room description, Webmin is a web-based system configuration tool.
