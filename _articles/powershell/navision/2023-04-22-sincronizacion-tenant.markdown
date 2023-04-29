---
title: Sincronizacion tenant

# post specific
# if not specified, .name will be used from _data/owner/[language].yml
author: Esteve Sanpons
# multiple category is not supported
category: PowerShell
# multiple tag entries are possible
tags: [PowerShell]
# disable comments on this page
comments_disable: true

# publish date
date: 2023-04-22 09:20:00 +0200
---

<!-- outline-start -->

Sincronizacion tenant

<br>
<!-- outline-end -->

```
$Ins = 'Nav71'

Import-Module "C:\Program Files\Microsoft Dynamics NAV\71\Service\NavAdminTool.ps1"
Sync-NAVTenant -ServerInstance $Ins


```
