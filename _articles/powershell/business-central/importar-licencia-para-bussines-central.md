---
title: Importar licencia para Bussines Central
summary: "Un script de PowerShell para importar la licencia"
layout: article
author: Esteve Sanpons
category: [PowerShell, Business_Central, Licencia]
custom_type: Boveda
date: 2023-04-22 09:05:00 +0200
---

Importar licencia para Bussines Central

<br>

Abrir Administration shell y Ejecutar como administrador

Import-NAVServerLicense **Nombre instancia** -LicenseData ([Byte[]]$(Get-Content -Path **ruta licencia** -Encoding Byte))

**PowerShell ISE**

```
$Ins = BC160
$Path = "C:\Temp\Licencias\BC 16.flf"

Import-Module "C:\Program Files\Microsoft Dynamics 365 Business Central\160\Service\NavAdminTool.ps1"
Import-NAVServerLicense $Ins -LicenseData ([Byte[]]$(Get-Content -Path $Path -Encoding Byte))
Restart-NAVServerInstance $Ins
```