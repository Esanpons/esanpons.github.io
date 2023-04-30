---
title: Crear Usuario tipo Windows en NAV
summary: "Crea usuarios Windows des de powershell"
layout: article
author: Esteve Sanpons
category: [PowerShell, Usuarios]
date: 2023-04-22 09:25:00 +0200
---

Crea usuarios Windows des de powershell

<br>

```
$UserName = 'testmitrabc\Administrator'
$FullUserName = 'Administrator'
$Instance = 'DynamicsNAV110'

Import-Module "C:\Program Files\Microsoft Dynamics NAV\110\Service\NavAdminTool.ps1"

New-NAVServerUser -ServerInstance $Instance -WindowsAccount $UserName -FullName $FullUserName -ErrorAction Inquire -Verbose
New-NavServerUserPermissionSet -WindowsAccount $UserName -ServerInstance $Instance -PermissionSetId SUPER
```
