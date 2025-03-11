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
Lines 1-5:<br />
<img src="https://github.com/AndresPineda-CySec/PowerShell-Incident-Response-Script/blob/main/Screenshot%202025-03-10%20230755.png?raw=true" height="80%" width="80%"/> <br/>
The script begins by creating a directory named IR_Reports on the desktop. Each time the script runs, it generates a new folder inside the IR_Reports, which is named using the current date and time in the format mm-dd-yy_hh-mm. Lines 1 and 4 create variables to store the directory paths, simplifying the script by referencing these paths later. Line 3 defines how the folder inside IR_Reports will be named using a timestamp. Line 5 then creates the folder at the specified path, ensuring it's a directory and using -Force to guarantee the file is overwritten or created.  <br/>
<br />
<br />
Lines 7 and 9:<br />
<img src="https://github.com/AndresPineda-CySec/PowerShell-Incident-Response-Script/blob/main/Screenshot%202025-03-11%20001126.png?raw=true" height="200%" width="200%"/> <br/>
Lines 7 and 9 both collect system information. Line 7 uses Get-Process to list all currently running programs, and Select-Object specifies which details to include. The output is then passed through a pipe ( | ) and saved as a CSV file named "Running_Processes" in the timestamped folder within IR_Reports. Line 9 works similarly but uses Get-NetTCPConnection to gather active network connections. Again, Select-Object filters the data, and the results are saved as "Network_Connections" in the same folder. Both lines use pipes to streamline the process of collecting, filtering, and exporting data. Both lines also refer to the variable fullReportPath from line 4, a pattern that will be used throughout the script.
<br />
<br />
Lines 11 and 12:<br />
<img src="https://github.com/AndresPineda-CySec/PowerShell-Incident-Response-Script/blob/main/Screenshot%202025-03-11%20020551.png?raw=true" height="200%" width="200%"/> <br/>
Line 11 starts with declaring the variable "events," which goes through the Windows event logs using the command Get-WinEvent. I Filter out which logs I want to be documented by using the command -FilterHashtable and two parameters: LogName, which is set to only back Security logs, and StartTime, which is set to only look as far back as 24 hours(This is done through the .AddDays(-1) command). Line 12 simply calls the "events" variable and routes the output through the pipe into the CSV named "Event_Logs."
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
