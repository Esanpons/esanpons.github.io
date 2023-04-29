---
title: Crear Usuario tipo Windows en BC

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
date: 2023-04-22 09:02:00 +0200
---

<!-- outline-start -->

Crea usuarios Windows des de powershell

<br>
<!-- outline-end -->

```
$UserName = 'testmitrabc\Administrator'
$FullUserName = 'Administrator'
$Instance = 'BC200'

Import-Module "C:\Program Files\Microsoft Dynamics 365 Business Central\200\Service\NavAdminTool.ps1"

New-NAVServerUser -ServerInstance $Instance -WindowsAccount $UserName -FullName $FullUserName -ErrorAction Inquire -Verbose
New-NavServerUserPermissionSet -WindowsAccount $UserName -ServerInstance $Instance -PermissionSetId SUPER
```
