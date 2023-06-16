---
title: "Ejecutar PowerShell desde Navision"
summary: "Funcionalidad para ejecutar PowerShell des de Navision"
layout: article
author: Esteve Sanpons
#cSpell:disable
category: [Navision]
custom_type: Boveda
permalink: /boveda/ejecutar-powershell-navision
#cSpell:enable
date: 2023-06-05 21:00:00 +0200
LinkedIn: true
---

Funcionalidad para ejecutar PowerShell des de Navision

En versiones OnPrem de Business Central viene de base una dll que se llama “PowerShellRunner”.

Esta dll la podemos encontrar si tenemos instalado un Business Central ni que sea el 14, como en este caso hemos hecho.
La ruta de la dll la podemos encontrar en:

> C:\Program Files\Microsoft Dynamics 365 Business Central\140\service\Add-in\PowerShellRunner

Si no tuvierais una opción de poder tener una BC14 OnPrem instalada os adjunto un [Link](https://github.com/Esanpons/PowerShellRunner/tree/main/Add-ins/PowerShellRunner) donde he colgado esta dll.

Lo primero será crear una codeunit donde añadiremos la función con la variable del “PowerShellRunner”.

```javascript

PowerShellRunner@1000000000 : DotNet "'Microsoft.Dynamics.Nav.PowerShellRunner, Version=14.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.Microsoft.Dynamics.Nav.PowerShellRunner";

```

Esta seria la funcion al completo:

```javascript

PROCEDURE PowerShellInvokeCommand@1000000014(Computer@1000000001 : Text;PathFile@1000000002 : Text) : Text;
    VAR
      PowerShellRunner@1000000000 : DotNet "'Microsoft.Dynamics.Nav.PowerShellRunner, Version=14.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.Microsoft.Dynamics.Nav.PowerShellRunner";
      txt01@1000000004 : TextConst 'ESP=Ha finalizado';
    BEGIN
      //Inicializamos variable
      CLEAR(PowerShellRunner);
      PowerShellRunner := PowerShellRunner.CreateInSandbox;

      //a¤adimos el pc y la ruta del power shell
      PowerShellRunner.AddCommand('Invoke-Command');
      PowerShellRunner.AddParameter('Computer',Computer);
      PowerShellRunner.AddParameter('File',PathFile);

      //parametro necesario para devolver el error
      PowerShellRunner.WriteEventOnError := TRUE;

      //ejecutamos y recojemos los resultados
      PowerShellRunner.BeginInvoke;
      PowerShellRunner.Results;

      //esperamos ya que puede darse que el powershell tarde mas que navision
      SLEEP(15000);

      //si hay error lo mostramos
      IF PowerShellRunner.HadErrors THEN
        ERROR(PowerShellRunner.ExceptionMessage);
    END;

```

Como podremos ver se pasan dos parámetros a la función, estos parámetros son, el nombre del PC y la ruta donde está el Power Shell a ejecutar.
Inicializamos la variable.
Después añadimos la ruta y el pc des de donde se ejecutará el Power Shell.
El parámetro “WriteEventOnError” lo tenemos que poner en TRUE para que nos indique si hay error y que error hay.

Ejecutamos y que nos devuelva el resultado.

Y esto es todo, cuando ejecutemos este código nos ejecutara automáticamente el Power Shell que le habremos indicado.

GitHub de los ejemplos en el [Link](https://github.com/Esanpons/PowerShellRunner)
