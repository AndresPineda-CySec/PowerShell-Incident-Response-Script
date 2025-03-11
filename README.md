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

<h3>Script Breakdown:<h3 />
<p align="center">
<br>The Complete Script:<br />
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
Line 26 retrieves all services using the command "Get-Service" and filters the services to include only those set to start automatically (StartType -eq "Automatic"), not currently running (Status -ne "Running"), and not related to core Windows functionality (DisplayName -notmatch "Windows"). These services are stored in the "services" variable. Line 27 exports the filtered services to a CSV file named "Unnecessary_Services."<br />
<br />
<br />
Lines 29 and 30: <br />
<img src="https://github.com/AndresPineda-CySec/PowerShell-Incident-Response-Script/blob/main/Screenshot%202025-03-10%20203522.png?raw=true" height="200%" width="200%"/> <br />
Line 29 retrieves all firewall rules using the command "Get-NetFirewallRule" and filters them to include only those that allow traffic, indicated by the Action property being set to Allow, and are currently enabled, indicated by the Enabled property being set to True. These filtered rules are stored in the "firewallRules" variable. Line 30 exports the selected rules to a CSV file named "Firewall_Rules," which provides a clear overview of active firewall rules that permit traffic, which can be useful for auditing or troubleshooting network security configurations.<br />
<br />
<br />
This is a complete breakdown of the script, but to use it, I have to create a task in the Task Scheduler to trigger the activation of the script.<br />
<br />
<br />
<br />
<br />
<p align="left">
<h3>How to Activate Through Task Scheduler:</h3>
<p align="center">
For this project, the script is triggered whenever a failed login occurs. To do this, I utilized the Task Scheduler.<br />
<br />
<br />
The first step was to Create a new task:<br />
<img src="https://github.com/AndresPineda-CySec/PowerShell-Incident-Response-Script/blob/main/create%20task.PNG?raw=true" height="80%" width="80%"/> <br />
<br />
<br />
Once the "new task" option is selected, I set the task to run "whether user is logged on or not" and "with highest privilege" under the "general" tab:<br />
<img src="https://github.com/AndresPineda-CySec/PowerShell-Incident-Response-Script/blob/main/general.PNG?raw=true" height="80%" width="80%"/> <br />
<br />
<br />
The next step is to create a new trigger under the "trigger" tab. Once the new option is selected, I set the "begin the task" option to "on an event," Log to "Security," source to "Microsoft Windows Security Auditing," and "Event ID" to "4625," which is the event ID to a failed logon attempt:<br />
<img src="https://github.com/AndresPineda-CySec/PowerShell-Incident-Response-Script/blob/main/trigger.PNG?raw=true" height="80%" width="80%"/> <br />
<br />
<br />
After setting the trigger, the action must be set. I created a new action under the "Actions" tab, which prompted a new window. Inside the new window, I set the "Action" option to "Start a program," "Program/script" option to powershell.exe, and entered "-windowStyle Hidden -ExecutionPolicy Bypass -File 'C:\Users\andro\Documents\Incident_Response.ps1'" in the "Add arguments" option:<br />
<img src="https://github.com/AndresPineda-CySec/PowerShell-Incident-Response-Script/blob/main/actions.png?raw=true" height="80%" width="80%"/> <br />
<br />
<br />
Once all the previous steps had been completed, I saved the changes in Task Scheduler and tested my new script.<br />
<br />
<br />
<br />
<br />
<p align="left">
<h3>Results:</h3>
<p align="center">
<br />
<br />
To perform the test, attempt to log in and intentionally fail. After the failed login attempt, login successfully to the computer. Upon logging in, you’ll notice a new folder on your desktop titled "IR_Reports." Within this folder, you’ll find subfolders named with the date and time of each failed login attempt. Inside these subfolders, you’ll discover the CSV files generated by the script:<br />
<img src="https://github.com/AndresPineda-CySec/PowerShell-Incident-Response-Script/blob/main/Screenshot%202025-03-10%20215932.png?raw=true" height="80%" width="80%"/> <br />
<br />
<br />
<br />
<br />
This concludes the steps I took to complete my PowerShell Incident Response Script.<br />
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
