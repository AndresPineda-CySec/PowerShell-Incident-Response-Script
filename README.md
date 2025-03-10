<h1>PowerShell Incident Response Script</h1>

<h2>Description</h2>
This is a PowerShell script that collects and exports critical system information into CSV files for security auditing and incident response. It creates a timestamped folder on the desktop and saves details about running processes, network connections, security event logs, missing updates, password policies, unnecessary services, and firewall rules. The script is triggered by specified tasks within the Task Scheduler, which will automatically run the script whenever one of the tasks is triggered. This project captures a snapshot of the system's state during potential security incidents for further analysis. 
<br />


<h2>Languages and Utilities Used</h2>

- <b>Windows PowerShell ISE</b> 
- <b>Task Scheduler</b>

<h2>Environments Used </h2>

- <b>Windows 11</b>

<h2>Program walk-through:</h2>

<p align="center">
<br />
<br />
Observe the wiped disk:  <br/>
<img src="https://i.imgur.com/AeZkvFQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
