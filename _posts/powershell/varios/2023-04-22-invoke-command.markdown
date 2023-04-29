---
title: Invoke-Command

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
date: 2023-04-22 09:31:00 +0200
---

<!-- outline-start -->

Ejecutar otro comando de powershell

<br>
<!-- outline-end -->

## Ejecutar Scripts des de otra maquina

Este ejemplo es para ejecutar un comando de powershel des de una maquina a otra y que se ejecute en la 2 maquina.

```
$command1 = { Stop-Service -Name 'MicrosoftDynamicsNavServer$Dynamics1100' }
$Computer = 'NombreMaquinaHaEjecutarScript'

#Paramos la instancia des de otra maquina
Invoke-Command -ComputerName $Computer -ScriptBlock $command1

Start-Sleep -Seconds 5

```
