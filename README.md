<h1>PowerShell Incident Response Script</h1>

<h2>Description</h2>
This PowerShell script collects and exports critical system information into CSV files for security auditing and incident response. The script is triggered by specified tasks within the Task Scheduler, which will automatically run the script whenever one of the tasks is triggered. This project captures a snapshot of the system's state during potential security incidents for further analysis. 
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
<img src="https://imgur.com/a/2GQG1xz" height="80%" width="80%"/> <br/>
The script begins by creating a directory named IR_Reports on the desktop. Each time the script runs, it generates a new folder inside the IR_Reports, named using the current date and time in the format mm-dd-yy_hh-mm.<br/>
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
