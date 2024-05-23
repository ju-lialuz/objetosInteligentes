# Objetos Inteligentes 
Repositório para o projeto de Objetos Inteligentes 5H



## Integrantes

> Beatriz Andreucci - 10401801

> Emily Morimoto - 10402275

> Giovanna Clementi - 10400852

> Júlia Luz - 10402335

## Descrição do projeto

O projeto tem como objetivo utilizar sensores de temperatura para após identificá-la, ligar ou desligar o ar condicionado do ambiente. Após nossas pesquisas, identificamos que a ventilação feita assim de forma híbrida auxilia na diminuição do gasto exessivo de energia. Caso a temperatura captada pelo sensor esteja abaixo de 26ºC, o ar condicionado será desligado e caso esteja acima, será ligado, mantendo uma temperatura agradável sem desperdícios.



## Código fonte do dispositivo:

[Link Wokwi](https://wokwi.com/projects/398550884312794113)


[Link Wokwi](https://wokwi.com/projects/398550889021953025)

## Código fonte da plataforma de programação “low code” (Node-RED):
```
[
    {
        "id": "997c1c2716fb99e1",
        "type": "tab",
        "label": "Fluxo 3",
        "disabled": false,
        "info": "",
        "env": []
    },
    {
        "id": "4205a95cd384bc84",
        "type": "mqtt out",
        "z": "997c1c2716fb99e1",
        "name": "",
        "topic": "home/relay",
        "qos": "2",
        "retain": "",
        "respTopic": "",
        "contentType": "",
        "userProps": "",
        "correl": "",
        "expiry": "",
        "broker": "7d07c3cf02c5975a",
        "x": 810,
        "y": 540,
        "wires": []
    },
    {
        "id": "f636971de2ec27e3",
        "type": "influxdb out",
        "z": "997c1c2716fb99e1",
        "influxdb": "a63ec929aaf59c9a",
        "name": "InfluxDB",
        "measurement": "Temperatura",
        "precision": "",
        "retentionPolicy": "",
        "database": "database",
        "precisionV18FluxV20": "ms",
        "retentionPolicyV18Flux": "",
        "org": "bbdc244c5e44e1fe",
        "bucket": "AppleGirlsBucket",
        "x": 400,
        "y": 700,
        "wires": []
    },
    {
        "id": "1545815e52d73d00",
        "type": "mqtt in",
        "z": "997c1c2716fb99e1",
        "name": "Temperatura",
        "topic": "TESTMACK10402275/Temperatura",
        "qos": "2",
        "datatype": "auto-detect",
        "broker": "7d07c3cf02c5975a",
        "nl": false,
        "rap": true,
        "rh": 0,
        "inputs": 0,
        "x": 210,
        "y": 540,
        "wires": [
            [
                "process_temp",
                "f636971de2ec27e3"
            ]
        ]
    },
    {
        "id": "31b21886ffa892a9",
        "type": "node-red-contrib-whatsapp-cmb-send-message",
        "z": "997c1c2716fb99e1",
        "name": "",
        "credtype": "account",
        "account": "6c21e7a593c863af",
        "text": "payload",
        "phonenumbervalue": "+5544999912455",
        "apikeyvalue": "4336898",
        "apikeyinputtypemessage": "msg",
        "phonenumberinputtypemessage": "msg",
        "inputtypemessage": "msg",
        "rejectssl": false,
        "x": 640,
        "y": 820,
        "wires": [
            [
                "ae89db7c8dda7547"
            ]
        ]
    },
    {
        "id": "ae89db7c8dda7547",
        "type": "debug",
        "z": "997c1c2716fb99e1",
        "name": "debug 8",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "payload",
        "targetType": "msg",
        "statusVal": "",
        "statusType": "auto",
        "x": 880,
        "y": 840,
        "wires": []
    },
    {
        "id": "5f2602fb1d896c45",
        "type": "influxdb out",
        "z": "997c1c2716fb99e1",
        "influxdb": "a63ec929aaf59c9a",
        "name": "InfluxDB2",
        "measurement": "test/relay",
        "precision": "",
        "retentionPolicy": "",
        "database": "database",
        "precisionV18FluxV20": "ms",
        "retentionPolicyV18Flux": "",
        "org": "bbdc244c5e44e1fe",
        "bucket": "AppleGirlsBucket",
        "x": 780,
        "y": 700,
        "wires": []
    },
    {
        "id": "process_temp",
        "type": "function",
        "z": "997c1c2716fb99e1",
        "name": "Process Temperature",
        "func": "\n// var temperatura = msg.payload.temperatura; \n// var message = \"\"\n// if (temperatura > 26){    \n//     msg.payload = { relay: \"ON\"  }\n// }\n// else {    \n//     msg.payload = { relay: \"OFF\" };\n//     }\n// return msg;\n\n// var temp = msg.payload.temperatura; \n// if(temp > 26) {\n//     msg.payload = {\n//         relay: \"ON\" \n//     };\n// } else {    \n//     msg.payload = { \n//         relay: \"OFF\" \n//     };\n// }\n// return msg;\n\nvar temp = parseInt(msg.payload, 10); \nif (temp > 26) {\n    msg.payload = \"A temperatura lida no sensor é \" + temp + \"°C. Relay: ON\";\n} else {\n    msg.payload = \"A temperatura lida no sensor é \" + temp + \"°C. Relay: OFF\";\n}\nreturn msg;\n",
        "outputs": 1,
        "timeout": "",
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 500,
        "y": 600,
        "wires": [
            [
                "31b21886ffa892a9",
                "4205a95cd384bc84",
                "5f2602fb1d896c45"
            ]
        ]
    },
    {
        "id": "7d07c3cf02c5975a",
        "type": "mqtt-broker",
        "name": "",
        "broker": "broker.hivemq.com",
        "port": "1883",
        "clientid": "",
        "autoConnect": true,
        "usetls": false,
        "protocolVersion": "4",
        "keepalive": "60",
        "cleansession": true,
        "autoUnsubscribe": true,
        "birthTopic": "",
        "birthQos": "0",
        "birthRetain": "false",
        "birthPayload": "",
        "birthMsg": {},
        "closeTopic": "",
        "closeQos": "0",
        "closeRetain": "false",
        "closePayload": "",
        "closeMsg": {},
        "willTopic": "",
        "willQos": "0",
        "willRetain": "false",
        "willPayload": "",
        "willMsg": {},
        "userProps": "",
        "sessionExpiry": ""
    },
    {
        "id": "a63ec929aaf59c9a",
        "type": "influxdb",
        "hostname": "127.0.0.1",
        "port": "8086",
        "protocol": "http",
        "database": "database",
        "name": "InfluxDB",
        "usetls": false,
        "tls": "",
        "influxdbVersion": "2.0",
        "url": "https://us-east-1-1.aws.cloud2.influxdata.com",
        "timeout": "10",
        "rejectUnauthorized": true
    },
    {
        "id": "6c21e7a593c863af",
        "type": "node-red-contrib-whatsapp-cmb-account",
        "name": "Mensagem "
    }
]
```


## Link Youtube
[Vídeo no Youtube](https://youtu.be/Yuo29p9j1gM)

