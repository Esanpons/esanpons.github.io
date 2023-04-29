---
title: Importar licencia para Bussines Central

# post specific
# if not specified, .name will be used from _data/owner/[language].yml
author: Esteve Sanpons
# multiple category is not supported
category: [PowerShell, Business_Central, Licencia]
# disable comments on this page
comments_disable: true

# publish date
date: 2023-04-22 09:05:00 +0200
---

<!-- outline-start -->

Importar licencia para Bussines Central

<br>
<!-- outline-end -->

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
