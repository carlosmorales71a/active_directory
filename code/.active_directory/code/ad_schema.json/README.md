#Create json Configuration file to convert to powershell script

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