---
title: Crear Usuario tipo Windows en BC
summary: "Crear usuario de tipo Windows con PowerShell"
layout: article
author: Esteve Sanpons
#cSpell:disable
category: [PowerShell, Usuarios]
custom_type: Boveda
#cSpell:enable
date: 2023-04-22 09:02:00 +0200
---

Crea usuarios Windows des de powershell

<br>

```
$UserName = 'testmitrabc\Administrator'
$FullUserName = 'Administrator'
$Instance = 'BC200'

Import-Module "C:\Program Files\Microsoft Dynamics 365 Business Central\200\Service\NavAdminTool.ps1"

New-NAVServerUser -ServerInstance $Instance -WindowsAccount $UserName -FullName $FullUserName -ErrorAction Inquire -Verbose
New-NavServerUserPermissionSet -WindowsAccount $UserName -ServerInstance $Instance -PermissionSetId SUPER
```
