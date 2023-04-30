---
title: Eliminar Archivos de una carpeta
layout: article
summary: "Elimina todos los archivos de la carpeta seleccionada."
author: Esteve Sanpons
category: PowerShell
usemathjax: true
date: 2023-04-22 09:33:00 +0200
---

Elimina todos los archivos de la carpeta seleccionada.

<br>

```
$path = 'C:\Temp'
Get-ChildItem -Path $path  -Recurse | Where-Object LastWriteTime -LT (Get-Date).AddMinutes(-10) | Remove-Item
```
