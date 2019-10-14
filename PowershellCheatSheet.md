# Powershell Commands Cheat Sheet

## General Commands
* Open sconfig from cmdline
```
sconfig
```
* Open PowerShell from cmdline
```
powershell
```
* Open Powershell as Admin
```
powershell -Command "Start-Process PowerShell -Verb RunAs"
```
* Restart Computer
```
Restart-Computer
```
* Shutdown computer
```
Stop-Computer
```
* Enable Remote Desktop
```
Enable-NetFirewallRule -DisplayGroup "Remote Desktop"
```
* Get Service's `LogOn as` user
```
(Get-WmiObject Win32_Service -Filter "Name='ServiceNameGoesHere'").StartName
```
* Reset Admin password
```
net user administrator "password"
```

## Folder Shares
* Get Folder Shares
```
Get-WmiObject -Class Win32_Share
```
* Add Folder Share (for everyone)
```
New-SmbShare -Name "ShareName" -Path "C:\SharePath" -FullAccess "Everyone"
```
* Remove Folder Share
```
Remove-SmbShare -Name "ShareName"
```

## Moving VMs
VMs do not liked to be moved when inside a domain, e.g. test.gmsl.local.  Use these commands to get around this.  Assumed domain is TESTGMSL (test.gmsl.local)
* Remove computer from domain
```
Remove-Computer -UnjoinDomaincredential TESTGMSL\your.name -Force -Restart
```
* Add computer to domain
```
Add-Computer -DomainName "TESTGMSL" -Credential TESTGMSL\your.name -Verbose -Restart
```
* Rename VM
```
Add-Computer -ComputerName MachineName -DomainName TESTGMSL -NewName NewMachineName -Credential TESTGMSL\your.name -Restart
```
* Reset Trust Relationship
```
Reset-ComputerMachinePassword -Credential TESTGMSL\your.name
```

* Reset Trust Relationship (remotely from VM on domain)
```
Invoke-Command -ComputerName "Remote_VM_name" -ScriptBlock {Reset-ComputerMachinePassword}
```
## Enivronment Variables
* Get (list) Env variables
```
Get-ChildItem Env:
```
* Get (specific) Env variable
```
Get-Item env:EnvVarName
```
* Set Env Variable (system `machine` level)
```
[Environment]::SetEnvironmentVariable("EnvVarName", "value", "Machine")
```
* Remove Env variable
```
Remove-Item env:EnvVarName
```
## IIS & Remote Mgmt
* Install IIS (incl. mgmt tools)
```
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```
* Install WinAuth for IIS
```
Install-WindowsFeature Web-Windows-Auth
```
* Install Web Mgmt Service (for remote access)
```
Install-WindowsFeature Web-Mgmt-Service
```
* Enable Remote Web Mgmt Access
```
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\WebManagement\Server" -Name "EnableRemoteManagement" -Value 1
```
* Set Web Mgmt Service to autostart
```
Set-Service WMSVC -StartupType Automatic
```
* Start Web Mgmt Service
```
Start-Service WMSVC
```
* Set localhost to trust HTTPS connections locally only
    * Note that certificate required for remote access
```
dotnet dev-certs https --trust
```
## Services
* Get Service
```
Get-Service ServiceName
```
* Start Service
```
Start-Service ServiceName
```
* Stop Service
```
Stop-Service ServiceName
```
* See [Useful Notes](## Useful Notes)
## Windows Server CORE & GUI
* Enable GUI on CORE (with internet connection)
```
Install-Windowsfeature Server-Gui-Mgmt-Infra, Server-Gui-Shell –restart
```
* Enable GUI on CORE (using installation media, mounted on D:\)
```
Install-WindowsFeature Server-Gui-Mgmt-Infra, Server-Gui-Shell –restart –source d:\sources\sxs
```
## Ports
* Get Ports in use (established)
```
Get-NetTCPConnection -State established
```
* Check port 8080 in use (established)
```
Get-NetTCPConnection -State established | findstr 8080
```
## Useful Notes
* You can cycle through commands
    1. `start-serv` + `<Tab>` gives you `Start-Service`
    1. `Start-Service Gmsl` + `<Tab` gives you `Start-Service GmslReporterService`
    1. Assuming `C:\Temp\Testing` exists, entering `C:\temp\t` + `<Tab>` gives you `C:\Temp\Testing`
