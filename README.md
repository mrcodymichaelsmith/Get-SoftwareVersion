# Get-SoftwareVersion
<!DOCTYPE html>
<html>

  <body>
    <h1>PowerShell and Software Updates</h1>
    <p>To address a security concern regarding outdated software on computers without interrupting users, PowerShell can be used to obtain a comprehensive list of all installed software, along with its version and installation date. The aim is to provide security with a report indicating the computer's up-to-date status.</p>
    <p>Although users typically avoid contact with the helpdesk unless they require assistance, PowerShell provides a way to obtain the required information without user interference. Initially, Get-Hotfix was considered but discarded because it only provides data about hotfixes installed by the Windows Installer. Therefore, after extensive searching, two solutions were found.</p>
    <p>The first solution uses Get-ItemProperty to search the Registry for software name, DisplayVersion, and InstallDate. It is the faster of the two options. The second solution employs a WMI object and retrieves the name, Version, Vendor, and Install date, which takes slightly longer to run. However, the second solution provides more information, so it is the preferred option.</p>
    <p>To remotely force an update, it is necessary to install the SDK on the computer, which is not always feasible. In most cases, it is discovered that the computer is up to date, but a reboot is required to complete the installation process. Therefore, if security requests an update, this one-liner can be used to determine the computer's update status, and users can be notified to reboot if necessary.</p>
  </body>
</html>



Use PowerShell to get a list of ALL install software on a computer, version and install date
Security was yelling at us one day because their Tenable reports were saying that Microsoft Edge, Microsoft Word, Google Chrome and other programs were missing critical software updates. They asked us to find the users of these computers and force them to update. Now, In my experience, most people don't want to talk to the helpdesk unless they need something from us. So, to fix this issue without interrupting our users, I wanted to use PowerShell to get a list of all installed software, the version installed and when it was installed. I would then run this program to get a report from the remote computer to show security that the computer was up to date. And, ideally, I would do what I could to make the software update
At first, I wanted use Get-Hotfix. It seemed perfect. Unfortunately This does not work because it only gives information about hotfixes installed by the windows installer. I wanted ALL software 
After a lot of digging I found two solutions . 
1: Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, InstallDate | Sort-Object DisplayName
2: Get-WmiObject -Class Win32_Product | Select-Object Name, Version, Vendor, InstallDate | Sort-Object Name
The first option searches the Registry for the software Name, DisplayVersion and InstallDate. It runs REALLY fast. The second option uses a WMI object and grabs the name Version, Vendor and Install date. It takes a minute to run. I have compared the two commands and I get more info out of the second option, so that is the primary one I want to use. 
Now, something I wanted to do is force the update remotely if I can. Most of these programs needed to be updated via software center. Unfortunately, without install the SDK onto my computer I am unable to force software center to install a package, or even view pending updates on a computer. That is disappointing but not the end of the world. 
I have used this one liner a lot and I have mostly learned one thing. The computer is usually up to date, but it needs to reboot to install the update. So, if you have security asking you to update computers because they are not up to date, go ahead and use this one liner to see if they are up to date or not. And then send an email to the user asking them to reboot. 
