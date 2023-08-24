---
title: "Dominando Business Central: Potencia Tu Desarrollo con ControlAddIns"
summary: "Explora el emocionante mundo de los ControlAddIns en Business Central y descubre cómo elevar tus aplicaciones con JavaScript, CSS y HTML. Aprende a crear tu propio "Hola, Mundo" y desbloquea un universo de posibilidades en el desarrollo."
layout: article
#cSpell:disable
category: [JavaScript, "ControlAddin", "Business_Central"]
custom_type: Boveda
permalink: /boveda/controladdin
#cSpell:enable
author: Esteve Sanpons
date: 2023-05-01 02:00:00 +0200
---

Hoy, tengo algo que quiero compartir.

He descubierto algo realmente asombroso en el mundo del desarrollo: ¡puedes crear maravillas en JavaScript, Css y Html directamente en Business Central!

En el ejemplo que traigo, vamos a sumergirnos en algo tan básico como un "¡Hola, Mundo!".

<br>

Nuestra primera parada es la creación del proyecto y de un nuevo tipo de objeto al que llamaremos "controladdin".

Daremos vida a este objeto llamándolo "HolaMundoAddIn". En su composición se encuentran propiedades que van a definir nuestro ControlAddin, y lo más emocionante: los scripts y funciones que vamos a ejecutar.

```javascript
controladdin "HelloWorldAddIn"
{
    RequestedHeight = 300;
    MinimumHeight = 300;
    MaximumHeight = 300;
    RequestedWidth = 700;
    MinimumWidth = 700;
    MaximumWidth = 700;
    VerticalStretch = true;
    VerticalShrink = true;
    HorizontalStretch = true;
    HorizontalShrink = true;
    StartupScript = 'src\controladdin\HelloWorldAddIn\js\InitScript.js';
    StyleSheets = 'src\controladdin\HelloWorldAddIn\css\StyleSheets.css';
    Scripts = 'src\controladdin\HelloWorldAddIn\js\Scripts.js';

    event CallBack();
    procedure HelloWorld(text: Text);
}
```

<br><br>

Echen un vistazo a este ControlAddin, aquí configuraremos un evento de arranque y una función.

Por supuesto, vamos a cargar los scripts que contienen estas funciones y ajustar el tamaño y las configuraciones para que encajen a la perfección.

Ah, notarán que las dos líneas de los Scripts están en rojo porque ¡faltan los archivos! Pero no te preocupes, ¡los vamos a crear enseguida!

Primero, vayamos al script de inicio. Esto se ejecutará al iniciar el ControlAddin.

```javascript
Microsoft.Dynamics.NAV.InvokeExtensibilityMethod("CallBack", "");
```

<br><br>

En este script, simplemente llamamos al método de NAV con el mismo nombre del evento que hemos definido en el ControlAddin. Este método se ejecutará cuando cargue.

El otro script es una maravilla sencilla: creamos la función para lanzar un mensaje "¡Hola, Mundo!".

```javascript
function HelloWorld(text) {
    alert("Hello world en " + text);
}
```

<br><br>
Con todo esto dicho, ahora solo nos falta llevar todo esto a una página para ver cómo se ejecuta nuestro mensaje.

En el diseño, añadiremos un "usercontrol" que enlace con nuestro ControlAddin.

Vamos a llamar al evento "CallBack" y, aquí, invocaremos la función "HelloWorld" que creamos en JavaScript.

Para rematar, como prueba de que incluso desde un botón podemos acceder al código de nuestro ControlAddin, crearemos un botón y le asociaremos la función

```javascript
page 60061 "TestPageAddIn"
{
    Caption = 'Test Page AddIn';

    PageType = List;
    Editable = false;
    UsageCategory = Lists;
    ApplicationArea = All;
    layout
    {
        area(Content)
        {
            usercontrol(HelloWorldAddIn; HelloWorldAddIn)
            {
                ApplicationArea = All;

                trigger Callback()
                begin
                    CurrPage.HelloWorldAddIn.HelloWorld('El Inicio');
                end;
            }
        }
    }

    actions
    {
        area(Creation)
        {
            action(LlamarFuncion)
            {
                ApplicationArea = All;
                Image = "1099Form";
                ToolTip = ' ';

                trigger OnAction();
                begin
                    CurrPage.HelloWorldAddIn.HelloWorld('Action');
                end;
            }
        }
    }
}
```

<br><br>

Ya estamos. Compilamos todo y damos rienda suelta a la ejecución. Si mostramos la página que hemos configurado, veremos nuestro primer mensaje. Y si lo lanzamos desde el botón, ¡aparecerá el segundo mensaje!

¡Bueno, hasta aquí el ejemplo sencillo!

Si quieres explorar más, puedes echar un vistazo al ejemplo completo en mi [GitHub](https://github.com/Esanpons/ControlAddIns-Business-Central)
