---
title: Eliminar empresas Navision
summary: "eliminamos empresas con PowerShell"
layout: article
author: Esteve Sanpons
#cSpell:disable
category: [PowerShell, Navision]
custom_type: Boveda
permalink: /boveda/eliminar-empresas-navision
#cSpell:enable
date: 2023-03-22 09:23:00 +0200
LinkedIn: false
---

Eliminar empresas Navision

<br>

```
$Ins = "Nav71"
$CompanyName = 'Cronus S.L.'


Import-Module "C:\Program Files\Microsoft Dynamics NAV\71\Service\NavAdminTool.ps1"
Remove-NAVCompany -ServerInstance $Ins -CompanyName $CompanyName -Force

```
