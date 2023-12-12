## Useful powershell commands for ADDS

### Commands to install ADDS and ADDS forrest with DNS
```
install-windowsfeature ad-domain-services  -IncludeAllSubFeature -IncludeManagementTools4
```
```
Install-ADDSForest  -DomainName  "mark.ca" -installDns
```

### To add IP address and DNS:

#### Show commands
```
Get-NetAdapter
Get-NetIPAddress
```
#### Adding ip address
```
New-NetIPaddress -interfacealias "adapter name" -Ipaddress 192.168 -Prefixlenght 24
```
#### Show commands of DNS settings
```
Get-DnsClientServerAdress
```
#### Adding DNS client ip address
```
Set-DNSClientServerAddress "Interfacealias" -ServerAddress ("Prepared-Dns-address","Alternate DNS"
```
#### To create organization unit
```
New-ADorganizationalUnit -name "Name you want" -protectFromAccidentalDeletion $false or $true
```
#### To remove organization unit
```
remove-ADorganizationalUnit -identity "OU=name of org,DC=,DC=" -Recursive
```
#### To filter organizational unit
```
Get-ADorganizationalUnit -filter 'name -like "name of the orga"'
```
#### To show all ogranizational unit
```
Get-ADorganizationalUnit -filter 'name -like "*"'
```
### To add user to org
```
New-Aduser -name "name of account" -GivenName "name of account" -SurName "surname of account" -SamAccountName "name of account" -UserPrincipalName "name.mark.ca" -Displayname "What name you want to apear" -Path "Ou=centennial,DC=,DC=" -Account password (read-host -assecurestring "input password of account") -passwordneverexpire $true or $ false -enable $true or $false
```
### To show the user 
```
Get-Aduser -filter -Searchbase "ou=centennial,dc=,dc"
```
### To add new group
```
New-Adgroup -name "name of the group" -path "ou=centennial,DC=,DC=" -GroupCategory security -Groupscope Global
```
### To show the group
```
Get-adgroup -filter -searchbase "ou=centennial,DC=,DC="
```
### To add user to the group
```
Add-ADgroupmember -identity "group name" -members _____,_____,_____ <-- multiple user
```
### To show who is in the group
```
get-adgroupmember -identity "name of the group"
```
### To create a file via cmd
```
mkdir nameOfTheFile
```
### To do a smb file
```
new-smbshare -name "name of the file" -path "c: " -fullaccess "name of the user"
```
### Another way to grant access
```
grant-smbshareaccess -name ____ <-name of file  -Accountname _____ <-name of account -accessrights _____ <-what kind of access rights you wan to grant
```
### To show the user accessrights
```
Get-smbshare access -name ____ <- name of the file
```
### To check if dhcp is installed
```
get-windows feature dhcp
```
### To install dhcp in window server
```
install-windowsfeature dhcp -includeallsubfeature -includemanagementtool
```
### To add dns in dhcp server
```
add-dhcpserverindc -dnsname ____ -ipaddress _____
```
### To show the dhcp in dc
```
get-dhcpserverindc
```
### To add dhcp in adds
```
add-dhcpserver4scope -name "name of the server" -startrange _____ -endrange______ -subnetmask______ -stateactive
```
### To exclude ip address
```
add-dhcpserver4exclusionrange -scopeid ______ -startrange _____ -endrange______
```
```
set-dhcpserveroptionvalue -optionid 3 -value ___ -scopeid ____
```
```
set-dhcp4optionvalue -dnsdomain _____ -dnsserver______
```
### To Install WDS
```
Install-WindowsFeature wds-deployment -includemanagementtools -includemanagementtools
```
### 1-initilze PXE boot so can push deployment over the network
```
$wdsUtilResults = wdsutil /initialize-server /remInst:"C:\RemoteInstall"
$wdsUtilResults | select -last 1
```
### 2-bootloader to start over the network
```
Import-WdsBootImage -Path "D:\sources\boot.wim" -newimagename "Win10"
```
### 3-mandatory when No mapping is appear
```
Get-Service -Name RpcSs | Start-Service
```
```
Get-Service -Name RpcLocator | Start-Service
```
```
Restart-Service WDSServer
```
###  create a new Group called WIn 10
```
New-WdsInstallImageGroup -Name "Win10"
```
### name of the ISO we have into the BOOT
```
Get-WindowsImage -imagePath "D:\sources\install.wim" | select Imagename
```
### pipline the Image so when we import the new image it will refere to the 
```
$imagename
```
```
$imageName = 'Windows 10 Pro'
```
```
Import-WdsInstallImage -ImageGroup "Win10" -Path "D:\sources\install.wim" -ImageName $imageName
```
### Installing HyperV powershell
```
Install-Windowsfeature hyper-v -includeAllsubFeature -IncludemanagementTools
```
### show commands
```
get-vm
```
### DISKPART AND MAKE RAID
```
get-disk
initialize-disk -number ___ <-number of disk -partitionstyle ___ <-MBR/GPT/Dynmic
```
### diskpart
### Raid 1 mirroring
```
create volume mirror disk=1,2
```
### Raid0 stripe
```
create volume stripe n=___ <- is the number of MB disk=3,4
```
### Raid5 
```
create volume raid disk=5,6,7
```
### how to format volume
```
select volume
```
```
format fs=ntfs label=___ <-whatever you want
```
```
assign  letter __ <-letter you want
```   

