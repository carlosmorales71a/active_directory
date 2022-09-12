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

# Create Variable to access comain controller
```shell
 $dc =New-PSSession -ComputerName xxx.xxx.xxx.xxx -Credential (Get-Credential)
```
