---
title: Crear Usuario tipo NavUser en BC
summary: "Crear usuario de tipo user y pass con PowerShell"
layout: article
author: Esteve Sanpons
category: [PowerShell, Usuarios]
date: 2023-04-22 09:10:00 +0200
---

Crea usuarios NavUser des de power shell

<br>

```
$Pass = 'Pass.001'
$Instance = 'BC200'
$UserName = 'Esanpons'

Import-Module "C:\Program Files\Microsoft Dynamics 365 Business Central\200\Service\NavAdminTool.ps1"

New-NAVServerUser -ServerInstance $Instance -UserName $UserName -Password (ConvertTo-SecureString $Pass -AsPlainText -Force) ChangePasswordAtNextLogOn -ErrorAction Inquire -Verbose
New-NavServerUserPermissionSet -UserName $UserName -ServerInstance $Instance -PermissionSetId SUPER
```
