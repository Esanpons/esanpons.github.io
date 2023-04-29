---
title: Crear Usuario tipo NavUser en BC

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
date: 2023-04-22 09:10:00 +0200
---

<!-- outline-start -->

Crea usuarios NavUser des de power shell

<br>
<!-- outline-end -->

```
$Pass = 'Pass.001'
$Instance = 'BC200'
$UserName = 'Esanpons'

Import-Module "C:\Program Files\Microsoft Dynamics 365 Business Central\200\Service\NavAdminTool.ps1"

New-NAVServerUser -ServerInstance $Instance -UserName $UserName -Password (ConvertTo-SecureString $Pass -AsPlainText -Force) ChangePasswordAtNextLogOn -ErrorAction Inquire -Verbose
New-NavServerUserPermissionSet -UserName $UserName -ServerInstance $Instance -PermissionSetId SUPER
```
