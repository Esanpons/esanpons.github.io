---
title: Publicar y despublicar App
summary: "Script para powershell para publicar y despublicar una app en BC."
layout: article
author: Esteve Sanpons
#cSpell:disable
category: [PowerShell, Business_Central]
custom_type: Boveda
permalink: /boveda/publicar-y-despublicar-app
#cSpell:enable
date: 2023-03-22 09:07:00 +0200
LinkedIn: false
---

Script para powershell para publicar y despublicar una app en BC.

Se tiene que rellenar las variables y ejecutar como administrador.

```
$Path = "C:\Temp\Mitra Global Services_Pruebas1.0.1.1.app"
$Ins = "BC170"
$NameAppUnPublish = "Pruebas"
$VersionApp = 1.0.0.1

Import-Module "C:\Program Files\Microsoft Dynamics 365 Business Central\170\Service\NavAdminTool.ps1"

Publish-NAVApp -ServerInstance $Ins  -Path $Path -SkipVerification

Unpublish-NAVApp $Ins -name $NameApp -Version $VersionApp
```
