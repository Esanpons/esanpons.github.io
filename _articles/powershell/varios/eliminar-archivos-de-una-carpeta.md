---
title: Eliminar Archivos de una carpeta
layout: article
summary: "Elimina todos los archivos de la carpeta seleccionada."
author: Esteve Sanpons
#cSpell:disable
category: [PowerShell]
custom_type: Boveda
permalink: /boveda/eliminar-archivos-de-una-carpeta
#cSpell:enable
date: 2023-03-22 09:33:00 +0200
LinkedIn: false
---

Elimina todos los archivos de la carpeta seleccionada.

<br>

```
$path = 'C:\Temp'
Get-ChildItem -Path $path  -Recurse | Where-Object LastWriteTime -LT (Get-Date).AddMinutes(-10) | Remove-Item
```
