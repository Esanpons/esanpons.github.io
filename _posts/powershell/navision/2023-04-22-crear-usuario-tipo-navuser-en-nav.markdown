---
title: Crear Usuario tipo NavUser en NAV

# post specific
# if not specified, .name will be used from _data/owner/[language].yml
author: Esteve Sanpons
# multiple category is not supported
category: PowerShell
# multiple tag entries are possible
tags: [PowerShell, Usuarios]
# disable comments on this page
comments_disable: true

# publish date
date: 2023-04-22 09:26:00 +0200
---

<!-- outline-start -->

Crea usuarios NavUser des de power shell

<br>
<!-- outline-end -->

```
$Pass = 'Pass.001'
$Instance = 'BC200'
$UserName = 'Esanpons'

Import-Module "C:\Program Files\Microsoft Dynamics NAV\110\Service\NavAdminTool.ps1"

New-NAVServerUser -ServerInstance $Instance -UserName $UserName -Password (ConvertTo-SecureString $Pass -AsPlainText -Force) ChangePasswordAtNextLogOn -ErrorAction Inquire -Verbose
New-NavServerUserPermissionSet -UserName $UserName -ServerInstance $Instance -PermissionSetId SUPER
```
