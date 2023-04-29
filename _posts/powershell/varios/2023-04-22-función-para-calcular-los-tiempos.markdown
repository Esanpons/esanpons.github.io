---
title: Función para calcular los tiempos

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
date: 2023-04-22 09:32:00 +0200
---

<!-- outline-start -->

Esta función se tiene que ejecutar antes de hacer nada mas.
Loque hace es que a partir de su ejecución calculara cuanto tarda cada script lanzado despues y te lo mostrara.

<br>
<!-- outline-end -->

```
function Prompt
{
  $time = ((Get-History)[-1].EndExecutionTime - (Get-History)[-1].StartExecutionTime)
  $promptString = ("$time | " + $(Get-Location) + ">")
  Write-Host $promptString -NoNewline -ForegroundColor cyan
  return " "
}

```
