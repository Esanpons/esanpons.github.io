---
layout: post
title: "Mejorando la seguridad y personalización en Business Central con ControlAddin"
summary: "Descubre cómo utilizar ControlAddin para ocultar áreas y elementos en Business Central, brindando mayor seguridad y personalización a los usuarios, optimizando así su experiencia y eficiencia en la plataforma."
author: "Esteve Sanpons"
date: "2023-06-12 07:00:00 +0200"
#cSpell:disable
category: ["JavaScript", "ControlAddin", "Business_Central", "Role_Center"]
thumbnail: /assets/img/posts/vaciar-area-de-trabajo-bc/Imagen01.jpg
permalink: /blog/vaciar-area-de-trabajo-bc/
custom_type: Blog
#cSpell:enable
---

Hola a todos, una semana más!

<br>

¿Alguna vez te has encontrado en la situación de tener que dar acceso a alguien a nuestro Business Central, pero esa persona no puede o no debe tener acceso a todo? Tienes que estar constantemente ajustando los roles y permisos para que esa persona solo vea algunas áreas de trabajo.

Esta semana voy a mostrarte cómo, a través de ControlAddins, puedes ocultar áreas e ítems específicos, de manera que los usuarios que accedan a esa área de trabajo tengan menos opciones para explorar y resolverlo todo.

El objetivo de hoy es modificar un área de trabajo estándar y eliminar algunos ítems y botones para restringir la navegación por la aplicación.
Hemos eliminado la mayoría de las configuraciones y búsquedas para que el usuario solo pueda moverse dentro del área de trabajo.

<input type="checkbox" id="image-checkbox-02" class="image-checkbox">
<label for="image-checkbox-02"  class="image-label">
    <img class="img-container" src="/assets/img/posts/vaciar-area-de-trabajo-bc/Imagen02.png">
</label>

<br><br><br>

Como siempre después de la breve explicación ¡Vamos manos a la obra! 👏

<br>

Lo primero, como siempre en estos casos, es crear el proyecto con sus carpetas.

-   controladdin
    -   CleanUpRoleCenter
        -   js
            -   scripts.js
            -   startup.js
        -   CleanUpRoleCenter.ControlAddin.al
-   page
    -   CleanUpRoleCenterPart.Page.al
-   pageextension
    -   OrderProcessorRoleCenter.PageExt.al

<br><br>

Comenzamos con el archivo del ControlAddin:

```javascript

controladdin "Clean Up Role Center"
{
    RequestedHeight = 1;
    RequestedWidth = 1;
    MaximumHeight = 1;
    MaximumWidth = 1;

    StartupScript =
        'src/controladdin/CleanUpRoleCenter/js/Startup.js';

    Scripts = 'https://code.jquery.com/jquery-3.7.0.min.js',
    'src/controladdin/CleanUpRoleCenter/js/Scripts.js';

    event ControlAddInReady();
    procedure RemoveItem(IdItem: Text);
    procedure RemoveParentItem(IdItem: Text);
    procedure RemoveParentParentItem(IdItem: Text);
    procedure HideItem(IdItem: Text);
    procedure HideParentItem(IdItem: Text);
    procedure HideParentParentItem(IdItem: Text);
}
```

<br><br>

El archivo "startup.js" lo usamos únicamente para llamar al evento de inicialización y así poder acceder a las funciones.

```javascript
Microsoft.Dynamics.NAV.InvokeExtensibilityMethod("ControlAddInReady", null);
```

<br><br>

Pasamos al archivo principal donde estará toda nuestra funcionalidad, "scripts.js":

```javascript
function RemoveItem(IdItem) {
    //Es para eliminar cualquier item dentro el controlAddIn
    $(IdItem).remove();
}

function RemoveParentItem(IdItem) {
    //Es para eliminar cualquier item por encima del controlAddIn
    parent.$(IdItem).remove();
}

function RemoveParentParentItem(IdItem) {
    //Es para eliminar cualquier item dos veces por encima del controlAddIn
    parent.parent.$(IdItem).remove();
}

function HideItem(IdItem) {
    //Es para ocultar cualquier item dentro el controlAddIn
    $(IdItem).hide();
}

function HideParentItem(IdItem) {
    //Es para ocultar cualquier item por encima del controlAddIn
    parent.$(IdItem).hide();
}

function HideParentParentItem(IdItem) {
    //Es para ocultar cualquier item dos veces por encima del controlAddIn
    parent.parent.$(IdItem).hide();
}

function WeMakeTheControlAddinInTakeUpLessSpace(IdControlAddIn) {
    // Hacemos que el controlAddIn ocupe menos espacio
    parent.$(IdControlAddIn).removeClass();
    parent
        .$(IdControlAddIn)
        .attr(
            "style",
            "width: 1px !important; height: 1px !important; min-width: 1px !important; min-height: 1px !important; max-width: 1px !important; max-height: 1px !important;"
        );
}
```

<br>
Las funciones son sencillas. Dependiendo de la función y necesidad que elijas, eliminarán o ocultarán el ítem o clase que pases como parámetro. Es importante mencionar el uso de "parent" debido a que el ControlAddin se encuentra en un "iframe" y este a su vez en otro. Dependiendo de dónde se encuentre el objeto, tendrás que llamar a una función u otra.

<br><br>

Y ahora viene la parte más complicada, tenemos que encontrar el ID, clase o identificador de cada uno de los ítems o partes que queremos quitar de nuestra área de trabajo. He explicado esto en el siguiente [Link](/boveda/como-saber-el-id-de-las-partes-de-una-web).
Así que omitiremos este punto y pasaremos directamente a crear la página.

```javascript
page 60005 "Clean Up Role Center Part"
{
    Caption = 'Clean Up Role Center', Comment = 'ESP="Limpiar área de trabajo"';
    PageType = CardPart;
    UsageCategory = None;

    layout
    {
        area(Content)
        {
            usercontrol(CleanUpRoleCenter; "Clean Up Role Center")
            {
                ApplicationArea = All;
                trigger ControlAddInReady()
                begin
                  // Hacemos que el controlAddIn ocupe menos espacio
                  CurrPage.CleanUpRoleCenter.WeMakeTheControlAddinInTakeUpLessSpace('#b15');

                  // Esto elimina de la barra de navegación de Business Central el botón de cambiar de empresa
                  CurrPage.CleanUpRoleCenter.RemoveParentParentItem('#BC_EnvironmentLauncher_container');

                  // Esto elimina de la barra de navegación de Business Central el botón de búsqueda
                  CurrPage.CleanUpRoleCenter.RemoveParentParentItem('#search_container');

                  // Eliminamos el título del controladdin
                  CurrPage.CleanUpRoleCenter.RemoveParentItem('.ms-nav-band-header');

                  // Esto elimina de la barra de navegación de Business Central el botón de notificaciones
                  CurrPage.CleanUpRoleCenter.RemoveParentParentItem('#notifications_container');

                  // Esto elimina de la barra de navegación de Business Central el botón de configuración
                  CurrPage.CleanUpRoleCenter.RemoveParentParentItem('#O365_MainLink_Settings_container');

                  // Esto elimina de la barra de navegación de Business Central el botón de ayuda
                  CurrPage.CleanUpRoleCenter.RemoveParentParentItem('#O365_MainLink_Help_container');

                  // Esto elimina de la barra de navegación de Business Central el botón de la cuenta de Office
                  CurrPage.CleanUpRoleCenter.RemoveParentParentItem('#O365_MainLink_Me');

                  // Esto elimina de la barra de navegación de Business Central el botón de iniciador de aplicaciones
                  CurrPage.CleanUpRoleCenter.RemoveParentParentItem('#O365_MainLink_NavMenu');

                  // Ocultamos el botón de navegar a otros perfiles
                  CurrPage.CleanUpRoleCenter.HideParentItem('.profile-action-container--2aSoMSmscwR9-5kzHiuG7g');
                end;
            }
        }
    }
}
```

<br><br>

Finalmente, añadimos la página que acabamos de crear en la primera posición del área de trabajo de la cual queremos eliminar estos campos.

```javascript
pageextension 60001 "Order Processor Role Center" extends "Order Processor Role Center"
{
    layout
    {
        addfirst(rolecenter)
        {
            part("CleanUpRoleCenterPart"; "Clean Up Role Center Part")
            {
                ApplicationArea = All;
            }
        }
    }
}
```

<br><br>

¡Ahora, tal y como mostré al principio, podemos tener un área de trabajo más segura e invitar a usuarios externos a nuestro Business Central!

En resumen, hemos visto cómo utilizar ControlAddin para ocultar áreas y elementos en Business Central, brindando a los usuarios un acceso restringido y mejorando la seguridad de la aplicación. Con esta funcionalidad, es posible personalizar y adaptar el entorno de trabajo según las necesidades de cada usuario, evitando la navegación innecesaria y mejorando la eficiencia en la utilización de la plataforma.

<br><br>

Como siempre, podréis ver el ejemplo entero en el [Link](https://github.com/Esanpons/ControlAddIn-Basico-BC)

<br>

Espero que este post haya sido útil y os ayude a poder tener mas seguridad en vuestro Business central. Si tenéis alguna pregunta o comentario, no dudéis en compartirlo. ¡Hasta la próxima semana!
