
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
