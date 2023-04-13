# Get-SoftwareVersion
Use Powershell to get a list of ALL install software on a computer, version and install date


Security was yelling at us one day because their Tenable reports were saying that Microsoft Edge, Microsoft Word, Google Chrome and other programs were missing criicle software updates. 
They asked us to find the users of these computers and force them to update. 
Now, In my experiance, most people don't want to talk to the helpdesk unless they need something from us. 
I want to use powershell to get a list of all installed software, the version installed and when it was installed. 
I would then run this program to get a report from the remote computer to show security that the computer was up to date. 

At first I wanetd to use Get-Hotfix. This does not work because it only gives information about hotfixes installed by the windows installer. 
This gives me no info on MS Office, Edge, Chrome, etc. 

Get-WmiObject -Class Win32_Product -ComputerName REMOTE_COMPUTER_NAME | Select-Object Name, Version, InstallDate

Note that querying the Win32_Product class can take some time, especially on systems with a large number of installed products, because it verifies the installation source and can trigger repairs for products that are not installed properly. If you only need a list of installed products and don't need the installation source verification or repair triggers, you can use the Get-ItemProperty cmdlet and query the uninstall registry keys instead. Here's an example command for that:

Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, InstallDate
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* -ComputerName REMOTE_COMPUTER_NAME | Select-Object DisplayName, DisplayVersion, InstallDate


omething pending is software center
Get-WmiObject -Namespace root\ccm\ClientSDK -Class CcmSoftwareUpdates | Where-Object { $_.EvaluationState -eq 2 }


Did something in software Center fail
Get-WmiObject -Namespace root\ccm\ClientSDK -Class CcmSoftwareUpdate | Where-Object { $_.ComplianceState -eq 4 }


How to force a program to install from software Center
$DeploymentID = "DeploymentIDHere"
New-CMApplicationDeployment -CollectionName "CollectionNameHere" -DeploymentId $DeploymentID -ForceInstall -FastNetworkInstall -DisableWoL

How to Get Deployment ID of software Center
Get-CMApplicationDeployment -Name "ApplicationNameHere" | Select-Object -ExpandProperty DeploymentId


$uninstallApps = Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object @{Name='Name';Expression={$_.DisplayName}}, DisplayVersion, Publisher, InstallDate
$wmiApps = Get-WmiObject -Class Win32_Product | Select-Object @{Name='Name';Expression={$_.Name}}, Version, Vendor, InstallDate
$allApps = $uninstallApps + $wmiApps | Sort-Object -Unique Name
$allApps


$uninstallApps = Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, Vendor, InstallDate
$wmiApps = Get-WmiObject -Class Win32_Product | Select-Object Name, Version, Vendor, InstallDate
$allApps = $uninstallApps + $wmiApps | Sort-Object -Unique DisplayName
$allApps
