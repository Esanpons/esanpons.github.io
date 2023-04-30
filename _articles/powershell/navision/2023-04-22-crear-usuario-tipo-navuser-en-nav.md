---
title: Crear Usuario tipo NavUser en NAV
summary: "Crea usuarios NavUser des de power shell"
layout: article
author: Esteve Sanpons
tags: [PowerShell, Usuarios]
date: 2023-04-22 09:26:00 +0200
---

Crea usuarios NavUser des de power shell

<br>

```
$Pass = 'Pass.001'
$Instance = 'BC200'
$UserName = 'Esanpons'

Import-Module "C:\Program Files\Microsoft Dynamics NAV\110\Service\NavAdminTool.ps1"

New-NAVServerUser -ServerInstance $Instance -UserName $UserName -Password (ConvertTo-SecureString $Pass -AsPlainText -Force) ChangePasswordAtNextLogOn -ErrorAction Inquire -Verbose
New-NavServerUserPermissionSet -UserName $UserName -ServerInstance $Instance -PermissionSetId SUPER
```
