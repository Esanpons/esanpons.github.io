---
title: Crear Usuario tipo Windows en NAV
summary: "Crea usuarios Windows des de powershell"
layout: article
author: Esteve Sanpons
#cSpell:disable
category: [PowerShell, Usuarios]
custom_type: Boveda
permalink: /boveda/crear-usuario-tipo-windows-en-nav
#cSpell:enable
date: 2023-03-22 09:25:00 +0200
LinkedIn: false
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
