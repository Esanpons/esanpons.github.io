---
title: Ejecutar codeunit desde PowerShell
summary: "Script para ejecutar codeunits desde powershell."
layout: article
author: Esteve Sanpons
#cSpell:disable
category: [PowerShell, Navision, Codeunit]
custom_type: Boveda
permalink: /boveda/ejecutar-codeunit-des-de-powershell
#cSpell:enable
date: 2023-03-22 09:24:00 +0200 
LinkedIn: true
---

Script para ejecutar codeunits des de powershell.
<br>

Adjunto el link hacia la documentación oficial en el [Link](https://learn.microsoft.com/en-us/powershell/module/microsoft.dynamics.nav.management/invoke-navcodeunit?view=businesscentral-ps-22) del Invoke-NAVCodeunit

```
$Comapny = "Cronus S.L."
$NoCodeUnit = 50000


Import-Module "C:\Program Files\Microsoft Dynamics NAV\71\Service\NavAdminTool.ps1"
Invoke-NAVCodeunit -ServerInstance JobAlmacén -CompanyName $Comapny -CodeunitId $NoCodeUnit -MethodName "ActivatePowerShell"
```
