---
title: Ejecutar codeunit des de PowerShell
summary: "Script para ejecutar codeunits des de powershell."
layout: article
author: Esteve Sanpons
category: [PowerShell, Navision, Codeunit]
custom_type: Boveda
date: 2023-04-22 09:24:00 +0200
---

Script para ejecutar codeunits des de powershell.
<br>

```
$Comapny = "Cronus S.L."
$NoCodeUnit = 50000


Import-Module "C:\Program Files\Microsoft Dynamics NAV\71\Service\NavAdminTool.ps1"
Invoke-NAVCodeunit -ServerInstance JobAlmacen -CompanyName $Comapny -CodeunitId $NoCodeUnit -MethodName "ActivatePowerShell"
```
