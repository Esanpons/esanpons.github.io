---
title: "Debugar WebServices en BC"
summary: "Ejemplo de configuración para el launch.json."
layout: article
author: Esteve Sanpons
#cSpell:disable
category: [Business_Central]
custom_type: Boveda
permalink: /boveda/debugar-webservices-en-bc
#cSpell:enable
date: 2023-06-24 03:00:00 +0200
LinkedIn: false
---
# Debugar WebServices en BC


Ejemplo de configuración para el launch.json.
<br><br>

```json

{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "DebbugerWS",
            "type": "al",
            "request": "attach",
            "server": "http://bc.com",
            "serverInstance": "BC200",
            "port": 7049,
            "authentication": "UserPassword",
            "breakOnError": true,
            "breakOnRecordWrite": false,
            "enableSqlInformationDebugger": true,
            "enableLongRunningSqlStatements": true,
            "longRunningSqlStatementsThreshold": 500,
            "numberOfSqlStatements": 10,
            "breakOnNext": "WebServiceClient"
        }
    ]
}

```
<br><br>

Enlace oficial [link](https://docs.microsoft.com/en-us/dynamics365/business-central/dev-itpro/developer/devenv-attach-debug-next)
<br><br>