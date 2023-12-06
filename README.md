# Halo-Home
## Adding Halo Home/ Eaton lights to Homebridge
Source: https://documenter.getpostman.com/view/6065583/RzfmEmUY#intro


## You will need some information for the cURL calls:
###  1. Location
###  2. PID/Device ID/Group ID
### 3. Auth Token

## To get them:
###  1.  LOCATION
###  2a. DEVICES 
```
curl -X GET http://api.avi-on.com/user/devices -H "Content-Type: application/json" -H "Authorization: Token <Token>"
```
###  2b. GROUPS
```
curl -X GET http://api.avi-on.com/user/groups -H "Content-Type: application/json" -H "Authorization: Token <Token>"
```
###  3. AUTH TOKEN
```
curl -X POST http://api.avi-on.com/user/devices -H "Content-Type: application/json" -d "{\"email\":\"user@domain.com\",\"password\":\"password\"}"
```

## In Homebridge Plugins, install homebridge-cmdswitch2

## To control a group of lights, enter your specific version of this config:

```
{
    "platform": "cmdSwitch2",
    "name": "LR Lights",
    "switches": {
        "LR Lights": {
            "name": "LR Lights",
            "on_cmd": "curl --request POST 'https://api.avi-on.com/groups/{{GROUP PID}}/state' \\\n                  --header 'Content-Type: application/json' \\\n                  --header 'Authorization: Token {{TOKEN}}' \\\n                  --data-raw '{\n                      \"state\": {\n                          \"name\": \"on_off\",\n                          \"value\": [1]\n                      }\n                  }'",
            "off_cmd": "curl --request POST 'https://api.avi-on.com/groups/{{GROUP PID}}/state' \\\n                   --header 'Content-Type: application/json' \\\n                   --header 'Authorization: Token {{TOKEN}}' \\\n                   --data-raw '{\n                       \"state\": {\n                           \"name\": \"on_off\",\n                           \"value\": [0]\n                       }\n                   }'"
        }
    }
}
```

Save and restart Homebridge
You should now sww a switch labeled LR Lights that will toggle on and off!
