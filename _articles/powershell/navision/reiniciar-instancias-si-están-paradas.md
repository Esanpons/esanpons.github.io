---
title: Reiniciar Instancias Si Están Paradas
summary: "Script para reiniciar instancias con PowerShell"
layout: article
author: Esteve Sanpons
category: [PowerShell, Instancias]
custom_type: Boveda
date: 2023-04-22 09:21:00 +0200
---

Lo primero es para crear una funcion para ejecutar para cualquier instancia.
la llamada a ala fucnion se tiene que poner despues de la funcion.
Se puede hacer mas de una llamada en el mismo script.

<br>

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
