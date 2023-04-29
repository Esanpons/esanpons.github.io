---
title: Crear Usuario tipo Windows en NAV

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
date: 2023-04-22 09:25:00 +0200
---

<!-- outline-start -->

Crea usuarios Windows des de powershell

<br>
<!-- outline-end -->

```
$UserName = 'testmitrabc\Administrator'
$FullUserName = 'Administrator'
$Instance = 'DynamicsNAV110'

Import-Module "C:\Program Files\Microsoft Dynamics NAV\110\Service\NavAdminTool.ps1"

New-NAVServerUser -ServerInstance $Instance -WindowsAccount $UserName -FullName $FullUserName -ErrorAction Inquire -Verbose
New-NavServerUserPermissionSet -WindowsAccount $UserName -ServerInstance $Instance -PermissionSetId SUPER
```
