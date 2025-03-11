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
The Complete Script:<br />
<img src="https://github.com/AndresPineda-CySec/PowerShell-Incident-Response-Script/blob/main/Screenshot%202025-03-11%20023722.png?raw=true" height="200%" width="200%"/> <br />
<br />
<br />
Lines 1-5:<br />
<img src="https://github.com/AndresPineda-CySec/PowerShell-Incident-Response-Script/blob/main/Screenshot%202025-03-10%20230755.png?raw=true" height="80%" width="80%"/> <br />
The script begins by creating a directory named IR_Reports on the desktop. Each time the script runs, it generates a new folder inside the IR_Reports, which is named using the current date and time in the format mm-dd-yy_hh-mm. Lines 1 and 4 create variables to store the directory paths, simplifying the script by referencing these paths later. Line 3 defines how the folder inside IR_Reports will be named using a timestamp. Line 5 then creates the folder at the specified path, ensuring it's a directory and using -Force to guarantee the file is overwritten or created.<br />
<br />
<br />
Lines 7 and 9:<br />
<img src="https://github.com/AndresPineda-CySec/PowerShell-Incident-Response-Script/blob/main/Screenshot%202025-03-11%20001126.png?raw=true" height="200%" width="200%"/> <br />
Lines 7 and 9 both collect system information. Line 7 uses Get-Process to list all currently running programs, and Select-Object specifies which details to include. The output is then passed through a pipe ( | ) and saved as a CSV file named "Running_Processes" in the timestamped folder within IR_Reports. Line 9 works similarly but uses Get-NetTCPConnection to gather active network connections. Again, Select-Object filters the data, and the results are saved as "Network_Connections" in the same folder. Both lines use pipes to streamline the process of collecting, filtering, and exporting data. Both lines also refer to the variable fullReportPath from line 4, a pattern that will be used throughout the script.<br />
<br />
<br />
Lines 11 and 12:<br />
<img src="https://github.com/AndresPineda-CySec/PowerShell-Incident-Response-Script/blob/main/Screenshot%202025-03-11%20020551.png?raw=true" height="200%" width="200%"/> <br />
In line 11, the variable "events" is declared and populated using the Get-WinEvent command, which retrieves Windows event logs. The -FilterHashtable parameter is used to filter the logs, specifying two key criteria: "LogName = 'Security'" will only search for Security logs, and "StartTime = (Get-Date).AddDays(-1)" will limit the result to events from the last 24 hours. Line 12 then takes the "events" variable, pipes the output, and exports it to a CSV file named "Event_Logs."<br />
<br />
<br />
Lines 14 and 15:<br />
<img src="https://github.com/AndresPineda-CySec/PowerShell-Incident-Response-Script/blob/main/Screenshot%202025-03-11%20024328.png?raw=true" height="200%" width="200%"/> <br />
Line 14 creates the variable "missingUpdates," which uses Get-WmiObject to query the Win32_QuickFixEngineering WMI class. The query retrieves all properties related to installed Windows updates using "SELECT *." The output is then filtered using Select-Object to include only the HotFixID and InstalledOn date. In Line 15, the "missingUpdates" variable is piped and exported to a CSV file named "Missing_Updates," creating a report of installed updates.<br />
<br />
<br />
Lines 17-24: <br />
<img src="https://github.com/AndresPineda-CySec/PowerShell-Incident-Response-Script/blob/main/Screenshot%202025-03-11%20031410.png?raw=true" height="200%" width="200%"/> <br />
Line 17 retrieves the default domain password policy using the command "Get-ADDefaultDomainPasswordPolicy" and filters the output to include only MinPasswordLength, LockoutThreshold, and MaxPasswordAge; the results are stored in the "pwPolicy" variable. Lines 18-20 declare that if a password policy is found, it is exported to a CSV file named "Password_Policy." Lines 21-24 generate a warning message if no policy is detected, highlighting the security risks of not having a password policy, and saves it to a .T file named "Password_Policy_WARNING." This ensures that the system's password policy is documented or flagged for review if missing.<br />
<br />
<br />
Lines 26 and 27: <br />
<img src="https://github.com/AndresPineda-CySec/PowerShell-Incident-Response-Script/blob/main/Screenshot%202025-03-11%20033146.png?raw=true" height="200%" width="200%"/> <br />
Line 26 retrieves all services using Get-Service and filters them to include only those set to start automatically (StartType -eq "Automatic"), not currently running (Status -ne "Running"), and not related to core Windows functionality (DisplayName -notmatch "Windows"). These services are stored in the $services variable. Line 2 exports the filtered services to a CSV file named Unnecessary_Services.csv, including properties like DisplayName (and potentially others, such as StartType or Status, depending on the intended output). This helps identify unnecessary or problematic services that are configured to start automatically but are not running.
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
