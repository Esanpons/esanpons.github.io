---
layout: post
title: "Domina el Arte de la Edición de Texto Enriquecido en Business Central"
summary: "Descubre cómo crear una caja de texto enriquecido en Microsoft Dynamics 365 Business Central. Te mostrare paso a paso cómo aprovechar al máximo los ControlAddins y la potente librería CKEditor. Edita y presenta tus datos de manera atractiva y profesional en tu aplicación Business Central."
author: "Esteve Sanpons"
date: "2023-10-29 04:00:00 +0200"
#cSpell:disable
category: ["JavaScript", "ControlAddin", "Business_Central"]
thumbnail: /assets/img/posts/enriched-text/imagen01.png
permalink: /blog/enriched-text/
custom_type: Blog
#cSpell:enable
---

¡Saludos a todos en una nueva entrega de nuestro emocionante viaje en el mundo de los ControlAddIns! 😃

Esta vez, estoy encantado de compartir contigo una extensión de lo que hemos estado explorando en blogs anteriores. Si tienes en mente la gran cantidad de información que hemos cubierto sobre ControlAddins, estás en el lugar correcto. Si no lo has revisado aún, no te preocupes, te dejo el enlace aquí para que puedas ponerte al día: [ControlAddins](/categorias/ControlAddin/).

Hoy, vamos a llevar las cosas un paso más allá. ¡Estamos a punto de crear algo asombroso: una caja de texto enriquecida! 😲😲

En el mundo de Business Central, esto solía ser algo que parecía impensable, pero ahora está al alcance de tus manos. Y lo mejor de todo, gran parte del código necesario para lograrlo proviene de un módulo gratuito que puedes encontrar y configurar fácilmente en el siguiente [enlace](https://ckeditor.com/ckeditor-5/online-builder/).

<br>

Después de esta emocionante introducción, ¡vamos manos a la obra! 😎

<br>

## El archivo ControlAddIn

Lo primero que haremos es crear nuestro ControlAddin. Si ya sabes cómo hacerlo, genial, pero si necesitas un recordatorio, no te preocupes, en la parte superior del blog mencioné enlaces a nuestras publicaciones anteriores para que puedas consultarlas en cualquier momento.

Este ControlAddin que estamos a punto de construir tendrá cuatro eventos y tres funciones. También configuraremos las llamadas a los scripts y archivos CSS necesarios, así como las propiedades que requeriremos.

```javascript

controladdin "EnrichedText"
{
    RequestedHeight = 350;
    VerticalStretch = true;
    HorizontalStretch = true;
    Scripts = 'EnrichedText/src/controladdin/EnrichedText/js/ckeditor.js', 'EnrichedText/src/controladdin/EnrichedText/js/Script.js';
    StartupScript = 'EnrichedText/src/controladdin/EnrichedText/js/InitScript.js';
    StyleSheets = 'EnrichedText/src/controladdin/EnrichedText/css/Stylesheets.css';

    event ControlReady();
    event SaveRequested(data: Text);
    event ContentChanged();
    event OnAfterInit();

    procedure Init();
    procedure Load(data: Text);
    procedure RequestSave();

}

```

<br><br>

## La Poderosa Librería CKEditor

Vamos a comenzar con el archivo ckeditor.js, que es uno de los elementos más fundamentales de nuestro proyecto. En realidad, es el módulo que obtenemos de la página web oficial, donde podemos configurarlo según nuestras necesidades y luego descargar el código. Una vez descargado, simplemente lo incorporamos a nuestro proyecto.

En este archivo, encontrarás una cantidad impresionante de código que se encarga de crear la ventana de edición de texto enriquecido. Es prácticamente como tener un asistente que hace la mayor parte del trabajo por nosotros. 🤩

<br>

## El Archivo Script.js

En cuanto al archivo Script.js, aquí es donde pondremos la mayor parte de nuestro propio código, incluyendo las llamadas a funciones y otros aspectos importantes.

Comenzaremos con la función "Init", que se encarga de preparar la ventana de texto enriquecido. El código en esta función ya viene predeterminado, pero siempre tienes la opción de personalizarlo según tus necesidades específicas. Al final de la función, verás una llamada al evento "OnAfterInit" que nos notificará cuando la inicialización haya terminado.

Las dos funciones siguientes son bastante fáciles de comprender:

-   "Load" se utiliza para cargar el texto previamente guardado.
-   "RequestSave" se encarga de guardar los cambios realizados en la caja de texto y activa un evento para gestionar el guardado de los datos.

```javascript
var InputArea;
var Editor;

function Init() {
    var div = document.getElementById("controlAddIn");
    div.style = "max-height: 600px;min-height: 600px;";
    div.innerHTML = "";
    InputArea = document.createElement("textarea");
    InputArea.id = "Comment";
    InputArea.name = "Comment";
    div.appendChild(InputArea);

    ClassicEditor.create(document.querySelector("#Comment"))
        .then((editor) => {
            Editor = editor;
            editor.model.document.on("change:data", () => {
                Microsoft.Dynamics.NAV.InvokeExtensibilityMethod("ContentChanged", []);
            });
        })
        .catch((error) => {
            console.error(error);
        });
    Microsoft.Dynamics.NAV.InvokeExtensibilityMethod("OnAfterInit", []);
}

function Load(data) {
    Editor.setData(data);
}

function RequestSave() {
    Microsoft.Dynamics.NAV.InvokeExtensibilityMethod("SaveRequested", [Editor.getData()]);
}
```

<br><br>

## El Estilo en Style.css

Pasemos ahora al archivo Style.css, donde definimos los estilos para nuestra caja de texto enriquecido. En este caso, establecemos el tamaño de la caja de texto. Es importante que mantengas este nombre, ya que coincide con el nombre utilizado en el archivo ckeditor.js, que es donde se encuentra la mayor parte del código que hace funcionar nuestro proyecto.

```css
.ck-editor__editable {
    min-height: 300px;
    max-height: 300px;
}
```

<br><br>

## Mostrar y Editar

Ahora, la parte emocionante: mostrar y editar nuestros textos enriquecidos. Para lograrlo, crearemos un campo en la tabla de productos mediante una TableExtension.

```javascript
tableextension 60000 "Item" extends Item
{
    fields
    {
        field(60000; "Item Description"; Text[2048])
        {
            DataClassification = CustomerContent;
            Caption = 'Item Description', Comment = 'ESP="Descripción producto"';
        }
    }
}

```

<br><br>

Luego, creamos una PageExtension. Dentro de esta PageExtension, agregamos el ControlAddin después del grupo de "Ítem" y configuramos los desencadenadores que creamos anteriormente para poder utilizarlos.

El primero de ellos es "OnAfterInit".

Aquí, simplemente verificamos si el campo tiene contenido y, si es así, cargamos esos datos en la caja de texto del ControlAddin, tal como se especifica en la función del archivo Script.js.

El segundo desencadenador es "ContentChanged".

Su función es guardar los cambios a medida que vamos editando el contenido de nuestra caja de texto enriquecido. Así, se activa la función correspondiente.

El tercero es "SaveRequested".

Esta función se encarga de guardar los cambios de la caja de texto en nuestro campo de la tabla.

Finalmente, para asegurarnos de que los datos se carguen correctamente en la caja de texto al cambiar de registro, creamos un desencadenador en la pagina llamado "OnAfterGetRecord" y servira para inicializar de nuevo si no lo hemos hecho ya.

También creamos una variable global para gestionar, que se inicializa en el desencadenador "OnAfterInit".

```javascript
pageextension 60000 "Item Card" extends "Item Card"
{
    layout
    {
        addafter(Item)
        {
            usercontrol(EditCtl; "EnrichedText")
            {
                ApplicationArea = All;
                trigger OnAfterInit()
                begin
                    EditorReadv := true;
                    if Rec."Item Description" <> '' then
                        CurrPage.EditCtl.Load(Rec."Item Description");
                end;

                trigger ContentChanged()
                begin
                    CurrPage.EditCtl.RequestSave();
                end;

                trigger SaveRequested(data: Text)
                begin
                    Rec."Item Description" := CopyStr(data, 1, 2048);
                end;
            }
        }
    }

    trigger OnAfterGetRecord()
    begin
        if EditorReadv then begin
            EditorReadv := false;
            CurrPage.EditCtl.Init();
        end;
    end;

    var
        EditorReadv: Boolean;
}

```

<br><br>

Como siempre, puedes encontrar el ejemplo completo en [GitHub](https://github.com/Esanpons/ControlAddIns-Business-Central)

<br>

Este es solo el punto de partida, y desde aquí, las posibilidades son innumerables. Continuare explorando y compartiendo más descubrimientos emocionantes en futuras publicaciones. ¡Mantente atento! 😄
