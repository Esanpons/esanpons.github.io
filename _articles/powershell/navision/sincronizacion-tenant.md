---
title: Sincronización tenant
summary: "Script para sincronizar tenant"
layout: article
author: Esteve Sanpons
#cSpell:disable
category: [PowerShell]
custom_type: Boveda
permalink: /boveda/sincronizacion-tenant
#cSpell:enable
date: 2023-03-22 09:20:00 +0200
LinkedIn: false
---

Sincronización tenant

<br>

```
$Ins = 'Nav71'

Import-Module "C:\Program Files\Microsoft Dynamics NAV\71\Service\NavAdminTool.ps1"
Sync-NAVTenant -ServerInstance $Ins


```
