# Installing the Domain Controller

1. Use 'sconfig' to:
    - Change the hostname
    - Change the IP address to static
    - Change the DNS server to our own IP address

2. Install the Active Directory Windows Feature

```shell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
```

# Configure Active Directory Windows Server 2022 Core

```shell
import-Module ADDSDeployment

install-ADDSForest
```

3. Find the index of the interface hosting the current IP address and set the interface’s DNS server
```shell
Get-NetIPAddress -IPAddress xxx.xxx.xxx.xxx
```

```shell
Get-DNSClientServerAddress
```

```shell
Set-DNSClientServerAddress -InterfaceIndex 5 -ServerAddresses xxx.xxx.xxx.xxx
```

# Joining the Workstation to the domain

```shell
Add-Computer -Domainname xyz.com -Credential xyz\Administrator -Force -Restart
```


