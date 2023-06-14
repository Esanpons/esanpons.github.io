---
layout: post
title: "Simplificando la gestión de permisos con conjuntos de permisos en Navision"
summary: "Descubre cómo simplificar la gestión de permisos en tus aplicaciones de Navision utilizando conjuntos de permisos en lugar de múltiples variables booleanas. Aprende cómo implementar esta solución y mejorar la organización de los permisos para brindar un mayor control de acceso de los usuarios."
author: "Esteve Sanpons"
date: "2023-07-17 07:00:00 +0200"
#cSpell:disable
category: ["Roles", "Permisos", "Navision"]
thumbnail: /assets/img/posts/check-permimisions/imagen01.jpg
permalink: /blog/check-permimisions/
custom_type: Blog
#cSpell:enable
---

¡Hola a todos!

Estoy emocionado de compartir con ustedes un nuevo descubrimiento que he encontrado en mi búsqueda continua de recursos útiles.

¿Alguna vez os ha pasado que queréis controlar el acceso de los usuarios a ciertas partes de un sitio o mostrar u ocultar acciones según los permisos asignados?

En la mayoría de los casos, se utilizan numerosas variables booleanas en la configuración de usuarios para marcar o desmarcar los permisos correspondientes.

Si bien esta práctica puede funcionar inicialmente, a medida que el número de permisos aumenta, puede volverse caótica.

Fue en ese momento cuando me propuse buscar una alternativa más ordenada, y así nació el desarrollo que os voy a mostrar hoy.

<br>

Sin más preámbulos, ¡vamos manos a la obra! :sunglasses:

<br>

Como siempre, lo primero que debemos hacer es crear las variables y la codeunit. A continuación, les presento el código:

```javascript
OBJECT Codeunit 50098 Check Permimisions
{
  OBJECT-PROPERTIES
  {
    Date=01/02/23;
    Time=17:11:46;
    Modified=Yes;
    Version List=UTILS, Permission;
  }
  PROPERTIES
  {
    OnRun=BEGIN
          END;

  }
  CODE
  {
    VAR
      error01@1000000000 : TextConst 'ENU=You do not have permissions. Contact the Administrator;ESP=No tienes permisos. Contacte con el Administrador';
      AccessControl@1000000003 : Record 2000000053;
      User@1000000002 : Record 2000000120;
      CompName@1000000001 : Text;
  }
}
```

<br><br><br>

Empezaremos con la función principal del desarrollo.

```javascript

    PROCEDURE CheckAccessControl@1000000001(User_ID@1000000000 : Text;txtRol@1000000001 : Text) : Boolean;
    VAR
      AccessControl@1000000002 : Record 2000000053;
      User@1000000003 : Record 2000000120;
      User_Administrador@1000000004 : TextConst 'ENU=ADMINISTRADOR;ESP=ADMINISTRADOR';
      Rol_Admin@1000000005 : TextConst 'ESP=_SYSTEMADMIN';
      CompName@1000000006 : Text;
    BEGIN
      CompName := COMPANYNAME;

      //buscamos el usuario para tener el user security
      User.RESET;
      User.SETRANGE("User Name",User_ID);
      IF NOT User.FINDFIRST THEN
        ERROR('');

      // Si el usuario es el ADministrador no lo chequeara
      IF User."User Name" = User_Administrador THEN
        EXIT(TRUE);

      //Comprobamos si tiene permisos para todas las empresas
      AccessControl.RESET;
      AccessControl.SETRANGE("User Security ID", User."User Security ID");
      AccessControl.SETRANGE("Role ID", txtRol);
      AccessControl.SETFILTER("Company Name",'=%1','');
      IF AccessControl.FINDFIRST THEN
        EXIT(TRUE);

      //Comprobamos si tiene permisos en la empresa actual
      AccessControl.RESET;
      AccessControl.SETRANGE("User Security ID", User."User Security ID");
      AccessControl.SETRANGE("Role ID", txtRol);
      AccessControl.SETRANGE("Company Name",CompName);
      IF AccessControl.FINDFIRST THEN
        EXIT(TRUE);

      EXIT(FALSE);
    END;
```

<br>

Hemos agregado una variable para almacenar el nombre de la empresa, lo que facilitará su manipulación.

En la primera parte del código, buscamos al usuario en cuestión.

A continuación, verificamos si el conjunto de permisos que hemos asignado a ese usuario está presente, comenzando por la empresa actual y luego expandiéndonos a las demás empresas.

Finalmente, devolvemos un valor booleano, lo cual nos permitirá utilizar esta función para determinar si el usuario tiene o no permisos.

<br><br>

Ahora crearemos otra función que mostrará un error directamente:

```javascript
    PROCEDURE CheckConError@1000000008(User_ID@1000000001 : Text;txtRol@1000000000 : Text);
    VAR
      error01@1000000002 : TextConst 'ENU=You do not have permissions. Contact the Administrator;ESP=No tienes permisos. Contacte con el Administrador';
    BEGIN
      IF NOT CheckAccessControl(User_ID,txtRol) THEN
        ERROR(error01);
    END;
```

<br>

Esta función simplemente llama a la primera función y, en caso de que el usuario no tenga permisos, muestra el error configurado.

<br><br>

Con esto, podemos utilizar estas funciones tanto para mostrar errores directos cuando no se tienen los permisos necesarios, como para ocultar ciertos elementos o deshabilitar acciones según los permisos asignados. ¡Las posibilidades son infinitas!

<br>

Como siempre, podréis ver el ejemplo entero en el [Link](https://github.com/Esanpons/EjemplosSencillosCAL/tree/main/Check%20Permimisions)

<br>

¡Y eso es todo! Espero que esta explicación y el código os hayan sido de ayuda. Si tenéis algún comentario o pregunta, no dudéis en hacerla. ¡Hasta la próxima!
