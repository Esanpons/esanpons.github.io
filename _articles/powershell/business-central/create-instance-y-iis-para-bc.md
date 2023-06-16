---
title: Create Instance y IIS para BC
summary: "Script para crear una instancia nueva y su IIS con PowerShell"
layout: article
author: Esteve Sanpons
#cSpell:disable
category: [PowerShell, IIS, Instancias]
custom_type: Boveda
permalink: /boveda/create-instance-y-iis-para-bc
#cSpell:enable
date: 2023-03-22 09:03:00 +0200
LinkedIn: true
---

Script para crear una instancia nueva y su IIS

Adjunto el link hacia la documentación oficial New-NAVServerInstance: https://learn.microsoft.com/en-us/powershell/module/microsoft.dynamics.nav.management/new-navserverinstance?view=businesscentral-ps-22

Adjunto el link hacia la documentación oficial New-NAVWebServerInstance: https://learn.microsoft.com/en-us/powershell/module/microsoft.dynamics.nav.management/new-navwebserverinstance?view=dynamicsnav-ps-2017

<br>

```
$Ins = "BC170"
$ServicePort = 7145
$ClientPort = 7146
$SoapPort = 7147
$OdataPort = 7148
$DevelPort = 7149
$Server = "MiServer"
$BBDD = "Cronus"
$Certificate = ""

Import-Module "C:\Program Files\Microsoft Dynamics 365 Business Central\200\Service\NavAdminTool.ps1"

#Para crear la instancia, requiere un usuario administrador
Get-Credential | New-NAVServerInstance $Ins -ServiceAccount User -ClientServicesCredentialType "NavUserPassword" -ServicesCertificateThumbprint $Certificate -DatabaseName $BBDD -DatabaseServer $Server -ManagementServicesPort $ServicePort -ClientServicesPort $ClientPort  -SOAPServicesPort $SoapPort  -ODataServicesPort $OdataPort -DeveloperServicesPort $DevelPort -verbose

#Para crear el IIS
New-NAVWebServerInstance -WebServerInstance $Ins -Server localhost -ServerInstance $Ins -ClientServicesCredentialType NavUserPassword -ClientServicesPort $ClientPort -ManagementServicesPort $ServicePort -verbose

#modificamos los tiempos de conexión del cliente web
Set-NAVServerConfiguration $Ins -KeyName ClientServicesKeepAliveInterval -KeyValue "00:00:15"
Set-NAVServerConfiguration $Ins -KeyName ClientServicesIdleClientTimeout -KeyValue "00:50:00"
Set-NAVServerConfiguration $Ins -KeyName ClientServicesReconnectPeriod -KeyValue "00:05:00"
Set-NAVServerConfiguration $Ins -KeyName NavHttpClientMaxTimeout -KeyValue "00:50:00"
Set-NAVServerConfiguration $Ins -KeyName SqlConnectionIdleTimeout -KeyValue "00:50:00"
Set-NAVWebServerInstanceConfiguration -WebServerInstance $Ins -KeyName SessionTimeout -KeyValue "00:50:00"

#Iniciamos la instancia.
Start-NAVServerInstance -ServerInstance $Ins -Verbose

```
