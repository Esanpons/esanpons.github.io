---
title: Sincronizacion tenant
summary: "Script para sincronizar tenant"
layout: article
author: Esteve Sanpons
category: [PowerShell]
date: 2023-04-22 09:20:00 +0200
---

Sincronizacion tenant

<br>

```
$Ins = 'Nav71'

Import-Module "C:\Program Files\Microsoft Dynamics NAV\71\Service\NavAdminTool.ps1"
Sync-NAVTenant -ServerInstance $Ins


```
