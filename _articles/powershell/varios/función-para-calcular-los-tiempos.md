---
title: Función para calcular los tiempos
summary: "Función para poder calcular los tiempos en los scrips"
layout: article
author: Esteve Sanpons
#cSpell:disable
category: [PowerShell]
custom_type: Boveda
#cSpell:enable
date: 2023-04-22 09:32:00 +0200
---

Esta función se tiene que ejecutar antes de hacer nada mas.
Lo que hace es que a partir de su ejecución calculara cuanto tarda cada script lanzado después y te lo mostrara.

<br>

```
function Prompt
{
  $time = ((Get-History)[-1].EndExecutionTime - (Get-History)[-1].StartExecutionTime)
  $promptString = ("$time | " + $(Get-Location) + ">")
  Write-Host $promptString -NoNewline -ForegroundColor cyan
  return " "
}

```
