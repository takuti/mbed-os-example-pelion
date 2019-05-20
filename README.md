Mbed OS Example: Pelion Data + Device
===

## Introduction

This example demonstrates how to use the Treasure Data REST API to send data from mbed OS to Treasure Data. In this example we will grab device information on the CPU, Heap, Stack and System information and forward it on to treasure data to log. 

## Pre-Requisites

- Treasure Data Account
- Treasure Data REST API Key
- Mbed OS Account

## How to use

```sh
pip install mbed-cli
```

```sh
mbed import git@github.com:takuti/mbed-os-example-pelion.git
cd mbed-os-example-pelion
```

Set Treasure Data dababase name and API key, and WiFi SSID/password to [`mbed_app.json`](./mbed_app.json):

```json
{
    "macros": [ "MBED_ALL_STATS_ENABLED"],
    "config": {
        "database":{
            "help":  "Treasure Data database name",
            "value": "\"REPLACE_WITH_YOUR_DB_NAME\""
        },
        "api-key":{
            "help":  "REST API Key for Treasure Data",
            "value": "\"REPLACE_WITH_YOUR_KEY\""
        },
        "main-stack-size": {
            "value": 12000
        }
    },
    "target_overrides": {
        "*": {
            "platform.stdio-convert-newlines": true,
            "mbed-trace.enable"                 : null,
            "nsapi.default-wifi-security"       : "WPA_WPA2",
            "nsapi.default-wifi-ssid"           : "\"SSID\"",
            "nsapi.default-wifi-password"       : "\"PASSWORD\""
        },
```

Install [`GCC_ARM`](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads) and configure:

```sh
mbed config -G GCC_ARM_PATH "/path/to/gcc-arm-none-eabi-xxxx-major/bin"
```

Compile and Run code on board: 

```sh
mbed compile --target auto --toolchain GCC_ARM --flash --sterm
```

You will see the messages in serial terminal:

```
Treasure Data REST API Demo
Connecting to the network using the default network interface...
Connected to the network successfully. IP address: ...
Success

MAC: ...
IP: ...
Netmask: ...
Gateway: ...

 Sending CPU Data: '{"uptime":3731872,"idle_time":509490,"sleep_time":509490,"deep_sleep_time":0}'

 Sending Heap Data: '{"current_size":15320,"max_size":75469,"total_size":742115,"reserved_size":301784,"alloc_cnt":14,"alloc_fail_cnt":0}'

 ...
```
    
Or, compile it in the [online compiler](http://os.mbed.com/compiler?import=https://github.com/takuti/mbed-os-example-pelion).

Finally, view data in Treasure Data (it will take 3-5 minutes to appear).

### What does this look like?

[![How to send data from Mbed OS to Treasure Data with HTTPS](https://img.youtube.com/vi/_tqD6GLMHQA/0.jpg)](https://www.youtube.com/watch?v=_tqD6GLMHQA)


### What if i'm not using Wifi? Or a different Wifi?

Thats awesome! In this program we are using the ism43362 wifi on the ST IOT01A board, but if you have a different board just swap out the network driver and change the config setting in the `mbed_app.json` file and you're good to go! All the NSAPI IP based drivers are compatible, so it works with Wifi, Ethernet, Cellular, ...etc. 

## How is this working?

See the readme on the [Treasure Data REST API Library](https://github.com/blackstoneengineering/mbed-os-treasuredata-rest).

## Troubleshooting

### SSL Failure

If over time the SSL cert expires you will need to replace the cert in `treasuredata-sslcert.h`. You can do this by running ` openssl s_client -connect in.treasuredata.com:443 -showcerts ` and replacing the cert with the lat one in the chain that is displayed. 

### HTTP failures

Try a different access point, double check you changed your Treasure Data API keys and the SSID / Password for the wifi are set correctly in `mbed_app.json`. 

## Liscense

Apache 2.0
