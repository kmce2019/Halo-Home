# Halo-Home
## Adding Halo Home/ Eaton lights to Homebridge
Sources: https://documenter.getpostman.com/view/6065583/RzfmEmUY#intro & 
        https://community.home-assistant.io/t/eaton-halo-home-integration/286724


## You will need some information for the cURL calls:
  1. AUTH TOKEN
  2. PIDs
  3. LOCATIONS

## To get them:
  1. AUTH TOKEN
```
curl -X POST -H "Content-Type: application/json" \
    -d '{"email": "your_email", "password": "your_password"}' \
    https://api.avi-on.com/sessions
```
  2. DEVICES 
```
curl -X GET -H "Content-Type: application/json" \
    -H "Authorization: Token <token>" \
    https://api.avi-on.com/locations/<location>/abstract_devices\?page\=1\&pagesize\=10
```
  3. LOCATIONS
```
curl -X GET -H "Content-Type: application/json" \
    -H "Authorization: Token <token>" \
    https://api.avi-on.com/user/locations
```
## Useful Tip
OF course you are running the above commands in a terminal and the output can be hard to read.
Copy/paste into Notepad ++
Now you can search for Device Names, Group Names, etc
You need these data points in the next part.

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
