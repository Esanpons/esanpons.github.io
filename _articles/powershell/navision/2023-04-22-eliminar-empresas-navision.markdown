---
title: Eliminar empresas Navision

# post specific
# if not specified, .name will be used from _data/owner/[language].yml
author: Esteve Sanpons
# multiple category is not supported
category: PowerShell
# multiple tag entries are possible
tags: [PowerShell, Navision]
# disable comments on this page
comments_disable: true

# publish date
date: 2023-04-22 09:23:00 +0200
---

<!-- outline-start -->

Eliminar empresas Navision

<br>
<!-- outline-end -->

```
$Ins = "Nav71"
$CompanyName = 'Cronus S.L.'


Import-Module "C:\Program Files\Microsoft Dynamics NAV\71\Service\NavAdminTool.ps1"
Remove-NAVCompany -ServerInstance $Ins -CompanyName $CompanyName -Force

```
