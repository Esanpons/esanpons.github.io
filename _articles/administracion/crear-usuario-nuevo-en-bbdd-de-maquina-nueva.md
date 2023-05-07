---
title: "Crear usuario nuevo en BBDD de máquina nueva"
summary: "Es para crear usuarios de windows en una BBDD"
layout: article
#cSpell:disable
category: ["Administracion", "Usuarios"]
custom_type: Boveda
permalink: /boveda/crear-usuario-nuevo-en-bbdd-de-maquina-nueva
#cSpell:enable
author: Esteve Sanpons
date: 2022-08-13 08:00:00 +0200
---

###### Es para crear usuarios de windows en una BBDD

<br>

Lo primero es añadir el usuario de Windows a la BBDD
Para ello nos vamos al usuario y de damos a propiedades.

<img class="img-container"  src="/assets/img/articles/crear-usuario-nuevo-en-bbdd-de-maquina-nueva/imagen1.png">
<br><br><br><br>

Después lo añadimos en la BBDD y le ponemos owner.

<img class="img-container"  src="/assets/img/articles/crear-usuario-nuevo-en-bbdd-de-maquina-nueva/imagen2.png">
<br><br><br><br>

Ahora vamos al servicio y añadimos el usuario administrador de la maquina a la instancia para que la arranque.

<img class="img-container"  src="/assets/img/articles/crear-usuario-nuevo-en-bbdd-de-maquina-nueva/imagen3.png">
<br><br><br><br>

Arrancamos instancia.

Si el usuario no existía en la BBDD, tiene que llamarse igual, aunque este con dominio diferente.
En el ejemplo que he puesto la BBDD ya tiene el usuario “administrator” pero con otro dominio, por lo que el paso siguiente nos dará error.
<br><br><br><br>

Ahora lo que hacemos es ejecutar el siguiente powershell

```powershell
$UserName = '501846-BD01\administrator'
$FullUserName = 'Administrator'
$Instance = 'DynamicsNAV100'

Import-Module "C:\Program Files\Microsoft Dynamics NAV\100\Service\Service\NavAdminTool.ps1"
New-NAVServerUser -ServerInstance $Instance -WindowsAccount $UserName -FullName $FullUserName -ErrorAction Inquire -Verbose
New-NavServerUserPermissionSet -WindowsAccount $UserName -ServerInstance $Instance -PermissionSetId SUPER
```

<br><br>

Cambiamos las variables del principio para que sean iguales a los parámetros que necesitamos y en la 4 linea en el import también cambiamos la ruta donde esta actualmente el powershell para importar las funciones de Navision. Esta línea se puede borrar si se hace con el powershell de Navision.
