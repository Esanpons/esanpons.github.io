---
title: Ejecutar codeunit des de PowerShell

# post specific
# if not specified, .name will be used from _data/owner/[language].yml
author: Esteve Sanpons
# multiple category is not supported
category: PowerShell
# multiple tag entries are possible
tags: [PowerShell, Navision, Codeunit]
# disable comments on this page
comments_disable: true

# publish date
date: 2023-04-22 09:24:00 +0200
---

<!-- outline-start -->

Script para ejecutar codeunits des de powershell.
<br>

<!-- outline-end -->

```
$Comapny = "Cronus S.L."
$NoCodeUnit = 50000


Import-Module "C:\Program Files\Microsoft Dynamics NAV\71\Service\NavAdminTool.ps1"
Invoke-NAVCodeunit -ServerInstance JobAlmacen -CompanyName $Comapny -CodeunitId $NoCodeUnit -MethodName "ActivatePowerShell"
```
