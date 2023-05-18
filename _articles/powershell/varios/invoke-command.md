---
title: Invoke-Command
summary: "Ejecutar otro comando de powershell"
layout: article
author: Esteve Sanpons
#cSpell:disable
category: [PowerShell]
custom_type: Boveda
permalink: /boveda/invoke-command
#cSpell:enable
date: 2023-03-22 09:31:00 +0200
---

Ejecutar otro comando de powershell

<br>

## Ejecutar Scripts des de otra maquina

Este ejemplo es para ejecutar un comando de powershell des de una maquina a otra y que se ejecute en la 2 maquina.

```
$command1 = { Stop-Service -Name 'MicrosoftDynamicsNavServer$Dynamics1100' }
$Computer = 'NombreMaquinaHaEjecutarScript'

#Paramos la instancia des de otra maquina
Invoke-Command -ComputerName $Computer -ScriptBlock $command1

Start-Sleep -Seconds 5

```
