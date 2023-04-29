---
title: Reiniciar Instancias Si Están Paradas

# post specific
# if not specified, .name will be used from _data/owner/[language].yml
author: Esteve Sanpons
# multiple category is not supported
category: PowerShell
# multiple tag entries are possible
tags: [PowerShell, Instancias]
# disable comments on this page
comments_disable: true

# publish date
date: 2023-04-22 09:21:00 +0200
---

<!-- outline-start -->

Lo primero es para crear una funcion para ejecutar para cualquier instancia.
la llamada a ala fucnion s etiene que poner despues de la funcion.
Se puede hacer mas de una llamada en el mismo script.

<br>
<!-- outline-end -->

```
#funcion para reiniciar la instancia si no esta Running
Function ResetInstancia {
    Param([string] $NameIns)
    $NameInstancia ='MicrosoftDynamicsNavServer$' + $NameIns
    $FiltroInstancia = '*' + $NameIns + '*'

    $Ins = Get-Service -Displayname $FiltroInstancia

    if ($Ins.Status -ne 'Running'){
        $Ins.Status
        Restart-Service -Name $NameInstancia
	Sync-NAVTenant -ServerInstance $NameIns
    }
}

#Instancia Nav110
$NameService = 'Nav110'
ResetInstancia($NameService)



```
