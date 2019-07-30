In the following all of the Node Red entities are documented. Watch out for the places where you need to enter your own credentials like TOKENS, SERIAL NUMBERS and the config for the MQTT Broker


**start every half hour**

    [
    {
        "id": "cd7223f0.1a9398",
        "type": "inject",
        "z": "4b4281a5.b723e8",
        "name": "start every half hour",
        "topic": "",
        "payload": "",
        "payloadType": "date",
        "repeat": "1800",
        "crontab": "",
        "once": false,
        "onceDelay": 0.1,
        "x": 140,
        "y": 80,
        "wires": [
            [
                "21464b90.8d439c",
                "6c6d3382.52739c",
                "113bb07f.bca6b",
                "aa7dcf0d.34ea18"
            ]
        ]
    }
    ]

**TOKEN**

    [
    {
        "id": "21464b90.8d439c",
        "type": "function",
        "z": "4b4281a5.b723e8",
        "name": "TOKEN",
        "func": "msg.headers = {};\nmsg.headers['token'] = 'ENTER_YOUR_PERSONAL_TOKEN_HERE';\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 360,
        "y": 80,
        "wires": [
            [
                "bc58007d.02c1e8",
                "85da95d5.ba1fd8",
                "884f22f4.c9bc2"
            ]
        ]
    }
    ]

**API CALL**

    [
    {
        "id": "bc58007d.02c1e8",
        "type": "http request",
        "z": "4b4281a5.b723e8",
        "name": "API CALL",
        "method": "GET",
        "ret": "obj",
        "paytoqs": false,
        "url": "https://app-api.niu.com/v3/motor_data/battery_info?sn=YOUR_SERIALNUMBER",
        "tls": "",
        "proxy": "",
        "authType": "basic",
        "x": 540,
        "y": 80,
        "wires": [
            [
                "a0d59476.f31848",
                "ce4f8bad.5fe5d8",
                "8f262b60.98705",
                "9945fd5a.d2b34"
            ]
        ]
    }
    ]

**set_niu_battery**

    [
    {
        "id": "a0d59476.f31848",
        "type": "change",
        "z": "4b4281a5.b723e8",
        "name": "",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "payload.data.batteries.compartmentA.batteryCharging",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 770,
        "y": 80,
        "wires": [
            [
                "e2eab287.d9e87"
            ]
        ]
    }
    ]
    
**niu_battery**

    [
    {
        "id": "e2eab287.d9e87",
        "type": "mqtt out",
        "z": "4b4281a5.b723e8",
        "name": "niu_battery",
        "topic": "node_red/niu_battery",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 990,
        "y": 80,
        "wires": []
    },
    {
        "id": "46614582.33caac",
        "type": "mqtt-broker",
        "z": "",
        "name": "YOUR_MQTT_BROKER_NAME",
        "broker": "YOUR_BROKER",
        "port": "YOUR_PORT",
        "clientid": "",
        "usetls": false,
        "compatmode": true,
        "keepalive": "60",
        "cleansession": true,
        "birthTopic": "",
        "birthQos": "0",
        "birthRetain": "true",
        "birthPayload": "",
        "closeTopic": "",
        "closeQos": "0",
        "closeRetain": "true",
        "closePayload": "",
        "willTopic": "",
        "willQos": "0",
        "willRetain": "true",
        "willPayload": ""
    }
    ]

**set_niu_mileage_left**

    [
    {
        "id": "ce4f8bad.5fe5d8",
        "type": "change",
        "z": "4b4281a5.b723e8",
        "name": "set_niu_mileage_left",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "payload.data.estimatedMileage",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 760,
        "y": 140,
        "wires": [
            [
                "10064ad.e64b235"
            ]
        ]
    }
    ]
    
**niu_mileage**

    [
    {
        "id": "10064ad.e64b235",
        "type": "mqtt out",
        "z": "4b4281a5.b723e8",
        "name": "niu_mileage",
        "topic": "node_red/niu_mileage",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 990,
        "y": 140,
        "wires": []
    },
    {
        "id": "46614582.33caac",
        "type": "mqtt-broker",
        "z": "",
        "name": "YOUR_MQTT_BROKER_NAME",
        "broker": "YOUR_BROKER",
        "port": "YOUR_PORT",
        "clientid": "",
        "usetls": false,
        "compatmode": true,
        "keepalive": "60",
        "cleansession": true,
        "birthTopic": "",
        "birthQos": "0",
        "birthRetain": "true",
        "birthPayload": "",
        "closeTopic": "",
        "closeQos": "0",
        "closeRetain": "true",
        "closePayload": "",
        "willTopic": "",
        "willQos": "0",
        "willRetain": "true",
        "willPayload": ""
    }
    ]

**set_battery_temp**

    [
    {
        "id": "8f262b60.98705",
        "type": "change",
        "z": "4b4281a5.b723e8",
        "name": "set_battery_temp",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "payload.data.batteries.compartmentA.temperature",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 770,
        "y": 200,
        "wires": [
            [
                "f8a81ad2.405578"
            ]
        ]
    }
    ]

**niu_battery_temp**

    [
    {
        "id": "f8a81ad2.405578",
        "type": "mqtt out",
        "z": "4b4281a5.b723e8",
        "name": "niu_battery_temp",
        "topic": "node_red/niu_battery_temp",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1010,
        "y": 200,
        "wires": []
    },
    {
        "id": "46614582.33caac",
        "type": "mqtt-broker",
        "z": "",
        "name": "YOUR_MQTT_BROKER_NAME",
        "broker": "YOUR_BROKER",
        "port": "YOUR_PORT",
        "clientid": "",
        "usetls": false,
        "compatmode": true,
        "keepalive": "60",
        "cleansession": true,
        "birthTopic": "",
        "birthQos": "0",
        "birthRetain": "true",
        "birthPayload": "",
        "closeTopic": "",
        "closeQos": "0",
        "closeRetain": "true",
        "closePayload": "",
        "willTopic": "",
        "willQos": "0",
        "willRetain": "true",
        "willPayload": ""
    }
    ]

**set_energy_consumed_today**

    [
    {
        "id": "9945fd5a.d2b34",
        "type": "change",
        "z": "4b4281a5.b723e8",
        "name": "set_energy_consumed_today",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "payload.data.batteries.compartmentA.energyConsumedTody",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 810,
        "y": 260,
        "wires": [
            [
                "95359b16.fd9e98"
            ]
        ]
    }
    ]

**consumed_today**

    [
    {
        "id": "95359b16.fd9e98",
        "type": "mqtt out",
        "z": "4b4281a5.b723e8",
        "name": "niu_consumed_today",
        "topic": "node_red/niu_consumed_today",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1080,
        "y": 260,
        "wires": []
    },
    {
        "id": "46614582.33caac",
        "type": "mqtt-broker",
        "z": "",
        "name": "YOUR_MQTT_BROKER_NAME",
        "broker": "YOUR_BROKER",
        "port": "YOUR_PORT",
        "clientid": "",
        "usetls": false,
        "compatmode": true,
        "keepalive": "60",
        "cleansession": true,
        "birthTopic": "",
        "birthQos": "0",
        "birthRetain": "true",
        "birthPayload": "",
        "closeTopic": "",
        "closeQos": "0",
        "closeRetain": "true",
        "closePayload": "",
        "willTopic": "",
        "willQos": "0",
        "willRetain": "true",
        "willPayload": ""
    }
    ]
