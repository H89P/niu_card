In the following all of the Node Red entities are documented. Watch out for the places where you need to enter your own credentials like TOKENS, SERIAL NUMBERS and the config for the MQTT Broker

Make sure to replace the following placeholders:
* YOUR_MQTT_BROKER_NAME
* YOUR_SERIAL_NUMBER
* YOUR_BROKER
* YOUR_PORT
* YOUR_TOKEN
* YOUR_ENERGY_COST (ct/kWh)
* YOUR_CO2_EMISSION_OF_COMPARED_VEHICLE (kg/km)

In order to gather the TOKEN follow the instructions [here](https://github.com/volkerschulz/NIU-API)

Some of the sensor values gathered via the node red flows are not utiized in my current implementation but already gathered for future use

<img  src=https://github.com/H89P/niu_card/blob/master/resources/node_red_pics/node_red_1.PNG>


**NIU Flow**

    [
    {
        "id": "4b4281a5.b723e8",
        "type": "tab",
        "label": "NIU",
        "disabled": false,
        "info": ""
    },
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
                "113bb07f.bca6b"
            ]
        ]
    },
    {
        "id": "bc58007d.02c1e8",
        "type": "http request",
        "z": "4b4281a5.b723e8",
        "name": "API CALL",
        "method": "GET",
        "ret": "obj",
        "paytoqs": false,
        "url": "https://app-api.niu.com/v3/motor_data/battery_info?sn=YOUR_SERIAL_NUMBER",
        "tls": "",
        "proxy": "",
        "authType": "basic",
        "x": 568,
        "y": 80,
        "wires": [
            [
                "a0d59476.f31848",
                "ce4f8bad.5fe5d8",
                "8f262b60.98705",
                "9945fd5a.d2b34"
            ]
        ]
    },
    {
        "id": "21464b90.8d439c",
        "type": "function",
        "z": "4b4281a5.b723e8",
        "name": "TOKEN",
        "func": "msg.headers = {};\nmsg.headers['token'] = 'YOUR_TOKEN';\nreturn msg;",
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
    },
    {
        "id": "85da95d5.ba1fd8",
        "type": "http request",
        "z": "4b4281a5.b723e8",
        "name": "API CALL",
        "method": "GET",
        "ret": "obj",
        "paytoqs": false,
        "url": "https://app-api.niu.com/v3/motor_data/index_info?sn=YOUR_SERIAL_NUMBER",
        "tls": "",
        "proxy": "",
        "authType": "basic",
        "x": 568,
        "y": 380,
        "wires": [
            [
                "d6688258.67ae68",
                "a483e9f6.9f0e7",
                "2a11b8dc.e6a3e8"
            ]
        ]
    },
    {
        "id": "e2eab287.d9e87",
        "type": "mqtt out",
        "z": "4b4281a5.b723e8",
        "name": "niu_battery",
        "topic": "node_red/niu_battery",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1078,
        "y": 80,
        "wires": []
    },
    {
        "id": "a0d59476.f31848",
        "type": "change",
        "z": "4b4281a5.b723e8",
        "name": "set_niu_battery",
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
        "x": 788,
        "y": 80,
        "wires": [
            [
                "e2eab287.d9e87"
            ]
        ]
    },
    {
        "id": "10064ad.e64b235",
        "type": "mqtt out",
        "z": "4b4281a5.b723e8",
        "name": "niu_mileage",
        "topic": "node_red/niu_mileage",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1078,
        "y": 140,
        "wires": []
    },
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
        "x": 808,
        "y": 140,
        "wires": [
            [
                "10064ad.e64b235"
            ]
        ]
    },
    {
        "id": "f8a81ad2.405578",
        "type": "mqtt out",
        "z": "4b4281a5.b723e8",
        "name": "niu_battery_temp",
        "topic": "node_red/niu_battery_temp",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1098,
        "y": 200,
        "wires": []
    },
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
        "x": 798,
        "y": 200,
        "wires": [
            [
                "f8a81ad2.405578"
            ]
        ]
    },
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
        "x": 838,
        "y": 260,
        "wires": [
            [
                "95359b16.fd9e98"
            ]
        ]
    },
    {
        "id": "95359b16.fd9e98",
        "type": "mqtt out",
        "z": "4b4281a5.b723e8",
        "name": "niu_consumed_today",
        "topic": "node_red/niu_consumed_today",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1108,
        "y": 260,
        "wires": []
    },
    {
        "id": "d6688258.67ae68",
        "type": "change",
        "z": "4b4281a5.b723e8",
        "name": "set_latitude",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "payload.data.postion.lat",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 778,
        "y": 320,
        "wires": [
            [
                "9c276695.5f41a8"
            ]
        ]
    },
    {
        "id": "9c276695.5f41a8",
        "type": "mqtt out",
        "z": "4b4281a5.b723e8",
        "name": "niu_latitude",
        "topic": "node_red/niu_latitude",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1078,
        "y": 320,
        "wires": []
    },
    {
        "id": "a483e9f6.9f0e7",
        "type": "change",
        "z": "4b4281a5.b723e8",
        "name": "set_longitude",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "payload.data.postion.lng",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 778,
        "y": 380,
        "wires": [
            [
                "d1eb1fb2.207468"
            ]
        ]
    },
    {
        "id": "d1eb1fb2.207468",
        "type": "mqtt out",
        "z": "4b4281a5.b723e8",
        "name": "niu_longitude",
        "topic": "node_red/niu_longitude",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1078,
        "y": 380,
        "wires": []
    },
    {
        "id": "fc472453.b12c5",
        "type": "mqtt out",
        "z": "4b4281a5.b723e8",
        "name": "niu_last_track_timestamp",
        "topic": "node_red/niu_last_track_timestamp",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1118,
        "y": 440,
        "wires": []
    },
    {
        "id": "2a11b8dc.e6a3e8",
        "type": "change",
        "z": "4b4281a5.b723e8",
        "name": "set_timestamp",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "payload.data.lastTrack.time",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 788,
        "y": 440,
        "wires": [
            [
                "fc472453.b12c5"
            ]
        ]
    },
    {
        "id": "884f22f4.c9bc2",
        "type": "http request",
        "z": "4b4281a5.b723e8",
        "name": "API CALL",
        "method": "GET",
        "ret": "obj",
        "paytoqs": false,
        "url": "https://app-api.niu.com/v3/motor_data/battery_info/health?sn=YOUR_SERIAL_NUMBER",
        "tls": "",
        "proxy": "",
        "authType": "basic",
        "x": 568,
        "y": 540,
        "wires": [
            [
                "4e53f15d.11d798",
                "96c89314.bca708"
            ]
        ]
    },
    {
        "id": "4e53f15d.11d798",
        "type": "change",
        "z": "4b4281a5.b723e8",
        "name": "set_charge_count",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "payload.data.batteries.compartmentA.healthRecords[0].chargeCount",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 798,
        "y": 500,
        "wires": [
            [
                "dd7317e6.ea33d8"
            ]
        ]
    },
    {
        "id": "dd7317e6.ea33d8",
        "type": "mqtt out",
        "z": "4b4281a5.b723e8",
        "name": "niu_charge_count",
        "topic": "node_red/niu_charge_count",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1098,
        "y": 500,
        "wires": []
    },
    {
        "id": "96c89314.bca708",
        "type": "change",
        "z": "4b4281a5.b723e8",
        "name": "set_battery_grade",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "payload.data.batteries.compartmentA.gradeBattery",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 798,
        "y": 560,
        "wires": [
            [
                "8622ee5c.fef088"
            ]
        ]
    },
    {
        "id": "8622ee5c.fef088",
        "type": "mqtt out",
        "z": "4b4281a5.b723e8",
        "name": "niu_battery_grade",
        "topic": "node_red/niu_battery_grade",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1098,
        "y": 560,
        "wires": []
    },
    {
        "id": "fa50ac1b.e215e",
        "type": "http request",
        "z": "4b4281a5.b723e8",
        "name": "API CALL",
        "method": "POST",
        "ret": "obj",
        "paytoqs": false,
        "url": "https://app-api.niu.com/motoinfo/overallTally",
        "tls": "",
        "proxy": "",
        "authType": "basic",
        "x": 568,
        "y": 660,
        "wires": [
            [
                "21002b84.7f89fc",
                "aabb60e.21b592"
            ]
        ]
    },
    {
        "id": "6c6d3382.52739c",
        "type": "function",
        "z": "4b4281a5.b723e8",
        "name": "TOKEN & SN",
        "func": "msg.headers = {};\nmsg.payload = { sn : \"YOUR_SERIAL_NUMBER\"};\nmsg.headers['token'] = 'YOUR_TOKEN';\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 380,
        "y": 660,
        "wires": [
            [
                "fa50ac1b.e215e"
            ]
        ]
    },
    {
        "id": "21002b84.7f89fc",
        "type": "change",
        "z": "4b4281a5.b723e8",
        "name": "set_total_mileage",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "payload.data.totalMileage",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 798,
        "y": 620,
        "wires": [
            [
                "92186800.7d9a3"
            ]
        ]
    },
    {
        "id": "92186800.7d9a3",
        "type": "mqtt out",
        "z": "4b4281a5.b723e8",
        "name": "niu_total_mileage",
        "topic": "node_red/niu_total_mileage",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1098,
        "y": 620,
        "wires": []
    },
    {
        "id": "f0766295.c48368",
        "type": "mqtt out",
        "z": "4b4281a5.b723e8",
        "name": "niu_total_days",
        "topic": "node_red/niu_total_days",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1088,
        "y": 680,
        "wires": []
    },
    {
        "id": "aabb60e.21b592",
        "type": "change",
        "z": "4b4281a5.b723e8",
        "name": "set_total_days",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "payload.data.bindDaysCount",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 788,
        "y": 680,
        "wires": [
            [
                "f0766295.c48368"
            ]
        ]
    },
    {
        "id": "113bb07f.bca6b",
        "type": "function",
        "z": "4b4281a5.b723e8",
        "name": "TOKEN &&",
        "func": "msg.headers = {};\nmsg.payload = { sn : \"YOUR_SERIAL_NUMBER\", index : \"0\", pagesize : \"100\"};\nmsg.headers['token'] = 'YOUR_TOKEN';\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 370,
        "y": 800,
        "wires": [
            [
                "d4fbdb6c.b2624"
            ]
        ]
    },
    {
        "id": "d4fbdb6c.b2624",
        "type": "http request",
        "z": "4b4281a5.b723e8",
        "name": "API CALL",
        "method": "POST",
        "ret": "obj",
        "paytoqs": false,
        "url": "https://app-api.niu.com/v3/motor_data/track",
        "tls": "",
        "proxy": "",
        "authType": "basic",
        "x": 568,
        "y": 800,
        "wires": [
            [
                "68a3b7cb.3fb13",
                "8309a61a.19e2a",
                "cc01db65.33e808",
                "9cc55341.97e7c",
                "405c990f.e241e8",
                "a38148cc.79eaa"
            ]
        ]
    },
    {
        "id": "68a3b7cb.3fb13",
        "type": "change",
        "z": "4b4281a5.b723e8",
        "name": "set_last_ride_start_time",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "payload.data[0].startTime",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 810,
        "y": 800,
        "wires": [
            [
                "e4e40eca.3ffc18"
            ]
        ]
    },
    {
        "id": "e4e40eca.3ffc18",
        "type": "mqtt out",
        "z": "4b4281a5.b723e8",
        "name": "niu_last_ride_starttime",
        "topic": "node_red/niu_last_ride_starttime",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1106,
        "y": 800,
        "wires": []
    },
    {
        "id": "d08ce66b.9b9fb8",
        "type": "mqtt out",
        "z": "4b4281a5.b723e8",
        "name": "niu_last_ride_endtime",
        "topic": "node_red/niu_last_ride_endtime",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1105,
        "y": 860,
        "wires": []
    },
    {
        "id": "8309a61a.19e2a",
        "type": "change",
        "z": "4b4281a5.b723e8",
        "name": "set_last_ride_end_time",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "payload.data[0].endTime",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 810,
        "y": 860,
        "wires": [
            [
                "d08ce66b.9b9fb8"
            ]
        ]
    },
    {
        "id": "cc01db65.33e808",
        "type": "change",
        "z": "4b4281a5.b723e8",
        "name": "set_last_ride_dist",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "payload.data[0].distance",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 790,
        "y": 920,
        "wires": [
            [
                "5908d8e.1b35ba8"
            ]
        ]
    },
    {
        "id": "5908d8e.1b35ba8",
        "type": "mqtt out",
        "z": "4b4281a5.b723e8",
        "name": "niu_last_ride_distance",
        "topic": "node_red/niu_last_ride_distance",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1104,
        "y": 920,
        "wires": []
    },
    {
        "id": "9cc55341.97e7c",
        "type": "change",
        "z": "4b4281a5.b723e8",
        "name": "set_last_ride_avg_speed",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "payload.data[0].avespeed",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 810,
        "y": 980,
        "wires": [
            [
                "b00d0b3a.3ce0e"
            ]
        ]
    },
    {
        "id": "b00d0b3a.3ce0e",
        "type": "mqtt out",
        "z": "4b4281a5.b723e8",
        "name": "niu_last_ride_avgspeed",
        "topic": "node_red/niu_last_ride_avgspeed",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1115,
        "y": 980,
        "wires": []
    },
    {
        "id": "405c990f.e241e8",
        "type": "change",
        "z": "4b4281a5.b723e8",
        "name": "set_last_ride_riding_time",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "payload.data[0].ridingtime",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 810,
        "y": 1040,
        "wires": [
            [
                "5506876d.1652a8"
            ]
        ]
    },
    {
        "id": "5506876d.1652a8",
        "type": "mqtt out",
        "z": "4b4281a5.b723e8",
        "name": "niu_last_ride_ridingtime",
        "topic": "node_red/niu_last_ride_ridingtime",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1116,
        "y": 1040,
        "wires": []
    },
    {
        "id": "a38148cc.79eaa",
        "type": "change",
        "z": "4b4281a5.b723e8",
        "name": "set_last_ride_date",
        "rules": [
            {
                "t": "set",
                "p": "payload",
                "pt": "msg",
                "to": "payload.data[0].date",
                "tot": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 790,
        "y": 1100,
        "wires": [
            [
                "48140b9f.c5e174"
            ]
        ]
    },
    {
        "id": "48140b9f.c5e174",
        "type": "mqtt out",
        "z": "4b4281a5.b723e8",
        "name": "niu_last_ride_date",
        "topic": "node_red/niu_last_ride_date",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1095,
        "y": 1100,
        "wires": []
    },
    {
        "id": "46614582.33caac",
        "type": "mqtt-broker",
        "z": "",
        "name": "YOUR_MQTT_BROKER_NAME",
        "broker": "YOUR_MQTT_BROKER_NAME",
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

**NIU Flow 2**

<img src="https://github.com/H89P/niu_card/blob/master/resources/node_red_pics/node_red_2.PNG">

    [
    {
        "id": "90d1d0e7.a63718",
        "type": "api-call-service",
        "z": "3029f75d.1728c8",
        "name": "notify_start_percentage",
        "server": "242cf831.b9a518",
        "version": 1,
        "service_domain": "notify",
        "service": "telegram_notifier",
        "entityId": "",
        "data": "{\"message\":\"NIU Batterie lädt - Start bei: {{ entity.sensor.niu_battery }}%\"}",
        "dataType": "json",
        "mergecontext": "",
        "output_location": "",
        "output_location_type": "none",
        "mustacheAltTags": false,
        "x": 1209,
        "y": 80,
        "wires": [
            []
        ]
    },
    {
        "id": "e7605834.48003",
        "type": "server-state-changed",
        "z": "3029f75d.1728c8",
        "name": "charging_plug_changed",
        "server": "242cf831.b9a518",
        "version": 1,
        "entityidfilter": "switch.0x00158d000356d5c8_switch",
        "entityidfiltertype": "exact",
        "outputinitially": false,
        "state_type": "str",
        "haltifstate": "on",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "outputs": 2,
        "output_only_on_state_change": true,
        "x": 200,
        "y": 82,
        "wires": [
            [
                "7003aca9.fb2b9c",
                "fc8e46d1.5c087"
            ],
            [
                "175f7df.ba8de82",
                "b56023c1.700d98",
                "d647f87b.c30c98"
            ]
        ]
    },
    {
        "id": "89fae87a.b2d828",
        "type": "api-call-service",
        "z": "3029f75d.1728c8",
        "name": "",
        "server": "242cf831.b9a518",
        "version": 1,
        "service_domain": "notify",
        "service": "telegram_notifier",
        "entityId": "",
        "data": "{\"message\":\"NIU Batterie Ladevorgang beendet: \\n   Energieverbrauch:  {{entity.sensor.niu_last_charge_kwh}}kWh\\n   Kosten: {{entity.sensor.niu_last_charge_euro}}€\"}",
        "dataType": "json",
        "mergecontext": "",
        "output_location": "",
        "output_location_type": "none",
        "mustacheAltTags": false,
        "x": 1460,
        "y": 280,
        "wires": [
            []
        ]
    },
    {
        "id": "7003aca9.fb2b9c",
        "type": "ha-get-entities",
        "z": "3029f75d.1728c8",
        "server": "242cf831.b9a518",
        "name": "get_kWh",
        "rules": [
            {
                "property": "entity_id",
                "logic": "is",
                "value": "sensor.energy_spent",
                "valueType": "str"
            }
        ],
        "output_type": "split",
        "output_empty_results": false,
        "output_location_type": "msg",
        "output_location": "payload",
        "output_results_count": 1,
        "x": 499,
        "y": 80,
        "wires": [
            [
                "c15e70ca.2e094"
            ]
        ]
    },
    {
        "id": "50605c58.6bdbcc",
        "type": "function",
        "z": "3029f75d.1728c8",
        "name": "set_initial_kwh",
        "func": "var initial_kwh = msg.payload.state || 0;\nflow.set('initial_kwh',initial_kwh);\nmsg.payload = initial_kwh;\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 939,
        "y": 80,
        "wires": [
            [
                "90d1d0e7.a63718"
            ]
        ]
    },
    {
        "id": "b67422a.09ae96",
        "type": "function",
        "z": "3029f75d.1728c8",
        "name": "calculate_kwh_spent",
        "func": "var neu_kwh = msg.payload.state || 0;\nneu_kwh = neu_kwh - flow.get('initial_kwh');\nflow.set('new_kwh', neu_kwh);\nmsg.payload = neu_kwh;\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 959,
        "y": 140,
        "wires": [
            [
                "78bdde2f.56f498"
            ]
        ]
    },
    {
        "id": "175f7df.ba8de82",
        "type": "ha-get-entities",
        "z": "3029f75d.1728c8",
        "server": "242cf831.b9a518",
        "name": "get_kWh",
        "rules": [
            {
                "property": "entity_id",
                "logic": "is",
                "value": "sensor.energy_spent",
                "valueType": "str"
            }
        ],
        "output_type": "split",
        "output_empty_results": false,
        "output_location_type": "msg",
        "output_location": "payload",
        "output_results_count": 1,
        "x": 499,
        "y": 140,
        "wires": [
            [
                "3248e365.a2230c"
            ]
        ]
    },
    {
        "id": "78bdde2f.56f498",
        "type": "mqtt out",
        "z": "3029f75d.1728c8",
        "name": "niu_last_charge_kwh",
        "topic": "node_red/niu_last_charge_kwh",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1199,
        "y": 140,
        "wires": []
    },
    {
        "id": "e92b49aa.e9b3d",
        "type": "function",
        "z": "3029f75d.1728c8",
        "name": "calculate_price",
        "func": "var neu_kwh = msg.payload.state || 0;\nneu_kwh = neu_kwh - flow.get('initial_kwh');\nflow.set('new_kwh', neu_kwh);\nmsg.payload = neu_kwh* YOUR_ENERGY_COST;\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "x": 939,
        "y": 200,
        "wires": [
            [
                "19c0954a.04b83b",
                "eaddc1e3.4d41"
            ]
        ]
    },
    {
        "id": "19c0954a.04b83b",
        "type": "mqtt out",
        "z": "3029f75d.1728c8",
        "name": "niu_last_charge_euro",
        "topic": "node_red/niu_last_charge_euro",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1199,
        "y": 200,
        "wires": []
    },
    {
        "id": "3248e365.a2230c",
        "type": "json",
        "z": "3029f75d.1728c8",
        "name": "convert_to_number",
        "property": "payload.state",
        "action": "",
        "pretty": false,
        "x": 709,
        "y": 140,
        "wires": [
            [
                "b67422a.09ae96",
                "e92b49aa.e9b3d"
            ]
        ]
    },
    {
        "id": "c15e70ca.2e094",
        "type": "json",
        "z": "3029f75d.1728c8",
        "name": "convert_to_number",
        "property": "payload.state",
        "action": "",
        "pretty": false,
        "x": 709,
        "y": 80,
        "wires": [
            [
                "50605c58.6bdbcc"
            ]
        ]
    },
    {
        "id": "eaddc1e3.4d41",
        "type": "delay",
        "z": "3029f75d.1728c8",
        "name": "delay_5_sec",
        "pauseType": "delay",
        "timeout": "5",
        "timeoutUnits": "seconds",
        "rate": "1",
        "nbRateUnits": "1",
        "rateUnits": "second",
        "randomFirst": "1",
        "randomLast": "5",
        "randomUnits": "seconds",
        "drop": false,
        "x": 1169,
        "y": 280,
        "wires": [
            [
                "89fae87a.b2d828"
            ]
        ]
    },
    {
        "id": "b56023c1.700d98",
        "type": "function",
        "z": "3029f75d.1728c8",
        "name": "calculate_total_kwh",
        "func": "const globalHomeAssistant = global.get('homeassistant');\nvar ges_kwh = parseFloat(globalHomeAssistant.homeAssistant.states[\"sensor.niu_charged_kwh_total\"].state);\nvar n_kwh = parseFloat(globalHomeAssistant.homeAssistant.states[\"sensor.niu_last_charge_kwh\"].state);\nges_kwh = ges_kwh + n_kwh \nmsg.payload = ges_kwh\nreturn msg;\n",
        "outputs": 1,
        "noerr": 0,
        "x": 530,
        "y": 360,
        "wires": [
            [
                "bc40be53.9347f"
            ]
        ]
    },
    {
        "id": "d647f87b.c30c98",
        "type": "function",
        "z": "3029f75d.1728c8",
        "name": "calculate_total_cost",
        "func": "const globalHomeAssistant = global.get('homeassistant');\nvar ges_euro = parseFloat(globalHomeAssistant.homeAssistant.states[\"sensor.niu_charging_cost_total\"].state);\nvar n_euro = parseFloat(globalHomeAssistant.homeAssistant.states[\"sensor.niu_last_charge_euro\"].state);\nges_euro = ges_euro + n_euro \nmsg.payload = ges_euro\nreturn msg;\n",
        "outputs": 1,
        "noerr": 0,
        "x": 530,
        "y": 440,
        "wires": [
            [
                "7643552c.f9ac74",
                "bd7a62c9.3d063"
            ]
        ]
    },
    {
        "id": "7643552c.f9ac74",
        "type": "function",
        "z": "3029f75d.1728c8",
        "name": "calculate_total_co2_saving",
        "func": "const globalHomeAssistant = global.get('homeassistant');\nvar n_co2 = parseFloat(YOUR_CO2_EMISSION_OF_COMPARED_VEHICLE*globalHomeAssistant.homeAssistant.states[\"sensor.niu_total_mileage\"].state- YOUR_ENERGY_COST*globalHomeAssistant.homeAssistant.states[\"sensor.niu_charged_kwh_total\"].state);\nmsg.payload = n_co2\nreturn msg;\n",
        "outputs": 1,
        "noerr": 0,
        "x": 860,
        "y": 520,
        "wires": [
            [
                "d9043ec.016844"
            ]
        ]
    },
    {
        "id": "bd7a62c9.3d063",
        "type": "mqtt out",
        "z": "3029f75d.1728c8",
        "name": "charging_cost_total",
        "topic": "node_red/charging_cost_total",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1190,
        "y": 440,
        "wires": []
    },
    {
        "id": "bc40be53.9347f",
        "type": "mqtt out",
        "z": "3029f75d.1728c8",
        "name": "charging_kwh_total",
        "topic": "node_red/charging_kwh_total",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1190,
        "y": 360,
        "wires": []
    },
    {
        "id": "d9043ec.016844",
        "type": "mqtt out",
        "z": "3029f75d.1728c8",
        "name": "co2_saving_total",
        "topic": "node_red/co2_saving_total",
        "qos": "",
        "retain": "true",
        "broker": "46614582.33caac",
        "x": 1190,
        "y": 520,
        "wires": []
    },
    {
        "id": "242cf831.b9a518",
        "type": "server",
        "z": "",
        "name": "Home Assistant",
        "legacy": false,
        "hassio": true,
        "rejectUnauthorizedCerts": true,
        "ha_boolean": "y|yes|true|on|home|open",
        "connectionDelay": true
    },
    {
        "id": "46614582.33caac",
        "type": "mqtt-broker",
        "z": "",
        "name": "YOUR_MQTT_BROKER_NAME",
        "broker": "YOUR_MQTT_BROKER",
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
    
