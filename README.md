# Structure thing (for a lack of a better name)

A softwarer for structuring, visualizing and monitoring of hierarchical data systems.

## Introduction

Many systems in real-life can be viewed as multiple sets of key-value pairs, with hierarchical relations between those sets.
An [example](https://demo.home.nullco.de/) would be:
```
Smart Home
├── Living Room [temperature]
│   ├── Smart TV [power, channel, volume]
├── Kitchen [temperature]
│   ├── Smart Oven [temperature, timer]
└── Bathroom [temperature, moisture]
    ├── Smart Mirror [light on/off]
    └── Smart Light [brightness, hue, effect]
```
As shown, nodes on every level can have their own attributes, as well as children.
Another [example](https://daniel.nullco.de/):
```
Person
├── Languages
│   ├── Python [lines of code written]
│   ├── C/C++ [segfaults caused, rockets crashed]
├── Projects
│   ├── BLEnky
│   ├── Structure thing
└── Presentations
    ├── Web&Wine
    │   ├── Hacking a closed source smartwatch
    │   ├── Node-red
    └── Hackerkiste
        └── Hacking a closed source smartwatch
```

In `structure-thing`, these nodes look like this:

![image](https://github.com/user-attachments/assets/85b2553f-e4db-446c-8164-e75da300f05f)

As evident, a node can have a set of attributes, while also containing further children.

To get real-time data into the software, its API can be used. Using the API, the following actions can be performed:
- Setting/Getting single values
- Setting/Getting data for a whole set
- Setting/Getting data for multiple sets based on filters
- Getting data in real-time through websockets

Furthermore, `structure-thing` provodes the following functions:
- Managing external access to nodes/subtrees with granular permissions
- Filtering incoming data in order to reject values based on JSONata expressions, for instance invalid json documents
- Transforming incoming data, for examples to convert units or to extract values from a JSON document.
- Setting up alert rules for data, for example when a temperature drops below 10° (`$ < 10`)
- Setting up frontend transformation, for example to add a unit sign: `$ & '°'`

At this point it should be mentioned that the software if context-agnostic. As such, neither does it understand a temperature or any other information, nor is it able to retrieve that information by itself.
To get any useful live information into the system, usually a middleware is required in order to transfer said information. Here is an example:
```
Structure-thing <-> connector <-> Stock prices API
```

If some foreign software sends out JSON webhooks, they can be send directly to `structure-thing` though. Via transformation the needed data can be extracted and forwarded to the system.
Here is an example:
```
The Things Network --> Structure-thing
Telegram Bot Api --> Structure-thing

Transformation: data.value
```
If you want to test the Telegram bot, [here](https://structure.nullco.de/?node_id=66de37d172285b08bc86cd3c&token=3c55f7bb1de0e79c9020dbb09deac741df56fc5474825407abb94f96714ce134) you go.

## Personal usages

### Smart home
I use this software in order to control my smart home. A close-to-real demo can be viewed [here](https://demo.home.nullco.de/).
Most of my devices are connected using BLE through [BLEnky](https://github.com/dakhnod/blenky/). As such, the infrastructure needs MQTT to BLE bridges, in order to connecte
the raspberry pi to BLE devices through WiFi. Each bridge needs a configuration containing the BLE devices to connecte to.
Since the topology of rooms/devices is known through the hierarchical structure in `structure-thing`, bridge configuration get derived automatically on change and pushed to the bridges.
If I move a device from one room to another in the software, the devices MAC address gets deleted on one bridge and appended to another.

### Personal portfolio
My projects and presentations can be viewed [here](https://daniel.nullco.de/).

### Inventory management
If you are interested in what electronical and mechanical parts I have in stock, you can check out [this](https://inventory.nullco.de).


## Installation

All services are provided as docker-compose. As such, they can be installed like this:
```
git clone git@gitlab.com:structure-thing/docker.git structure-thing
cd structure-thing
docker compose up -d

docker inspect structure-nginx-1
# Then visit the IPAddress of the nginx container in your browser.
# This should give you one shot to set up a root account
```
