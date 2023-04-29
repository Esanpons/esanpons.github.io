---
title: Eliminar Archivos de una carpeta

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
date: 2023-04-22 09:33:00 +0200
---

<!-- outline-start -->

Elimina todos los archivos de la carpeta seleccionada.

<br>
<!-- outline-end -->

```
$path = 'C:\Temp'
Get-ChildItem -Path $path  -Recurse | Where-Object LastWriteTime -LT (Get-Date).AddMinutes(-10) | Remove-Item
```
