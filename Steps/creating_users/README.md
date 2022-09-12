# Create .json configuration file to convert to PS Code to create AD Users and Groups

// ad_schema.json

```shell
{
    "groups" : [
        {
            "name": "Cyber Security"
        }
    ],

    "users": [

        {
            "first_name": "Carlos",
            "last_name": "Morales",
            "password": "literallyanything",
            "username": "cmorales",
            "groups": [
                "Cyber Security"
            ]
        }
    ]
}
```

# Convert from json code

```shell
Get-Content .\ad_schema.json |ConvertFrom-Json
```

# Create variable to access domain controller 
```shell
 $dc =New-PSSession -ComputerName xxx.xxx.xxx.xxx -Credential (Get-Credential)
```

# Copy ad_schema.json to domain controller

```shell
Copy-Item .\ad_schema.json -ToSession $dc C:\Windows\Tasks
Enter-PSSession $dc
```

# Create Powershell Script gen_ad.ps1
\\ gen_ad.ps1
```shell
param( [Parameter(Mandatory=$true)] $JSONFile)

function CreateADGroup(){
    param( [Parameter(Mandatory=$true)] $groupObject)

    $name = $groupObject.name
    New-ADGroup -name $name -GroupScope Global 
}

function CreateADUser(){
    param( [Parameter(Mandatory=$true)] $userObject)

    # Pull out the name from the Json object
    $name = $userObject.name
    $password = $userObject.password

    # Generate a "first initial, last name" structure for username
    $firstname, $lastname = &name.Split(" ")
    $username = ($firstname[0] + $lastname).ToLower()
    $samAccountName = $username
    $principalname = $username

    # Create AD User object
    New-ADUser -Name "$name" -GivenName $firstname -Surname $lastname -SamAccountName $SamAccountName -UserPrincipalName $principalname@$Global:Domain -AccountPassword (ConvertTo-SecureString $password -AsPlainText -Force) -PassThru | Enable-ADAccount
 
    # Add the user to its appropiate group
    foreach($group_name in $userObject.groups) {

        try {
            Get-ADGroup -Identity “$group_name”
            Add-ADGroupMember -Identity $group_name -Members $username
        }
        catch [Microsoft.ActiveDirectory.Management.ADIdentityNotFoundException]
         {
            Write-Warning “User $name NOT added to group $group_name because it does not exist”
         }
    }
}


$json = ( Get-Content $JSONFile | ConvertFrom-Json)

$Global:Domain = $json.domain

foreach ( $group in $json.groups ){
    CreateADGroup $group
}

foreach ( $user in $json.users ){
    CreateADUser $user
}
```
\\ Set Execution policy
```shell
Set-ExecutionPolicy RemoteSigned
```

