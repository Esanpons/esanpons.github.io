---
title: Publicar y despublicar App

# post specific
# if not specified, .name will be used from _data/owner/[language].yml
author: Esteve Sanpons
# multiple category is not supported
category: PowerShell
# multiple tag entries are possible
tags: [PowerShell, Business Central]
# disable comments on this page
comments_disable: true

# publish date
date: 2023-04-22 09:07:00 +0200
---

<!-- outline-start -->

Script para powershell para publicar y despublicar una app en BC.
<br>

<!-- outline-end -->

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
