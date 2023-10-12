---
layout: post
title: "Textos Enriquecidos a la Perfección: Summernote en Business Central"
summary: "Explorando las Maravillas de Summernote en Business Central: Un Viaje de Simplificación y Potencial Ilimitado para Enriquecer tus Textos y Facilitar tu Trabajo Diario"
author: "Esteve Sanpons"
date: "2023-12-03 04:00:00 +0200"
#cSpell:disable
category: ["JavaScript", "ControlAddin", "Business_Central"]
thumbnail: /assets/img/posts/summernote-inicio/imagen01.png
permalink: /blog/summernote-inicio/
custom_type: Blog
#cSpell:enable
---

Buenas una semana mas

Hoy estoy aquí entusiasmado porque os traigo una auténtica joya que va a llevar nuestra experiencia con el texto enriquecido a otro nivel. Si recordáis, en un [post](/blog/enriched-text) anterior os hablé del módulo gratuito "ckeditor-5". Pero después de exprimirlo y trastearlo a fondo, decidí aventurarme a explorar otras alternativas.

No malinterpretéis, "ckeditor-5" es genial, pero necesitaba algo más fácil de usar, cambiar, y que fuera menos complicado, que no me hiciera sudar la gota gorda. Y así, con toda la emoción, os presento a "Summernote". Una opción fresca, sencilla y con un potencial impresionante. ¿Queréis echarle un vistazo? Aquí tenéis el [enlace](https://summernote.org/).

Aunque a primera vista puede parecer que tiene menos opciones que su contraparte, os aseguro que todo lo que necesitáis está empacado en su código. ¡Y ojo! Me encontré con algunas características que no hallé en la otra alternativa o que me resultaron más complicadas de programar.

<br>

## ¿Qué es Summernote y qué maravillas puede hacer?

Vamos por partes. Summernote es una biblioteca JavaScript que nos permite integrar un editor de texto enriquecido directamente en nuestras aplicaciones web. ¿Y qué significa eso? Pues que podréis darle a vuestros usuarios la capacidad de dar rienda suelta a su creatividad al manipular el texto. Negritas, cursivas, listas, enlaces, ¡lo que se os ocurra!

Y lo mejor de todo, "Summernote" simplifica este proceso.

<br>

Ahora ya os he explicado todo y después de la charleta, ¡Vamos manos a la obra! 🥳

<br>

## Archivo ControlAddIn: El Corazón de la extensión

Primero, dejadme daros una breve introducción. El ControlAddIn es como el núcleo de nuestro proyecto. Es el cerebro que permite que todo funcione de manera armoniosa. En este caso, estamos incorporando Summernote, y el ControlAddIn es el lugar donde todo comienza.

Ahora, echemos un vistazo al código. Aquí estamos definiendo nuestro ControlAddIn y dándole todas las instrucciones que necesita para hacer su trabajo de manera eficiente

```javascript
controladdin "SummerNote"
{
    Scripts =
        'https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.6.0.js', //https://docs.microsoft.com/en-us/aspnet/ajax/cdn/overview
        'https://cdn.jsdelivr.net/npm/summernote@0.8.20/dist/summernote.min.js', //https://cdn.jsdelivr.net/npm/summernote@0.8.20/dist/
        'https://cdn.jsdelivr.net/npm/summernote@0.8.20/dist/lang/summernote-es-ES.js', //https://cdn.jsdelivr.net/npm/summernote@0.8.20/dist/
        'https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.5/umd/popper.min.js',   //este revisar en la ultima version cual estan utilizando antes de actualizar
        'https://maxcdn.bootstrapcdn.com/bootstrap/4.1.3/js/bootstrap.min.js', //este revisar en la ultima version cual estan utilizando antes de actualizar
        'EnrichedText-Summernote/src/controlAddIn/summernote/js/ScriptsDescription.js';

    StyleSheets =
         'https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css', //https://www.bootstrapcdn.com/
         'https://cdn.jsdelivr.net/npm/summernote@0.8.20/dist/summernote.min.css'; //https://cdn.jsdelivr.net/npm/summernote@0.8.20/dist/

    StartupScript =
        'EnrichedText-Summernote/src/controlAddIn/summernote/js/Startup.js';

    RequestedHeight = 300;
    MinimumHeight = 1;
    HorizontalStretch = true;

    event ControlAddInReady();

    //funciones y eventos para la parte del Description
    procedure AddNewSummerNoteDescription(Data: Text);

}

```

<br>

Analicemos esto detalladamente:

-   **Scripts y StyleSheets**: Aquí estamos enlazando todas las bibliotecas y estilos necesarios para que Summernote funcione correctamente. ¿Os acordáis de esos tiempos en los que teníamos que descargar cada archivo por separado? Ahora, todo está disponible en la nube. Es importante mencionar el archivo "ScriptsDescription.js" que es donde incluiremos nuestro codigo personalizado.

-   **StartupScript**: Este script se ejecuta cuando el ControlAddIn se inicia. En este caso, estamos simplemente invocando el script de inicio del ControlAddIn.

-   **RequestedHeight**, MinimumHeight, HorizontalStretch: Estas propiedades controlan el tamaño y comportamiento de nuestro ControlAddIn en la interfaz de usuario.

-   **event ControlAddInReady()**: Este evento se activa cuando el ControlAddIn está listo.

-   **procedure AddNewSummerNoteDescription(Data: Text)**: Aquí definimos una función que nos permite agregar nuevo contenido a Summernote. ¿Veis ese "Data: Text"? Aquí es donde entra el texto que queremos que Summernote muestre.

<br><br>

## Archivo Startup.js: El Punto de Partida

En este archivo, simplemente añadimos el evento de inicio del ControlAddIn. Algo sencillo, pero esencial.

```javascript
Microsoft.Dynamics.NAV.InvokeExtensibilityMethod("ControlAddInReady", null);
```

<br><br>

## Archivo ScriptsDescription.js: Donde ocurre todo

Vamos a ahondar en el archivo donde realmente se destila todo: el ScriptsDescription.js.

Aquí es donde la narrativa del Summernote toma forma y le damos vida a nuestro editor de texto enriquecido.

Antes de zambullirnos en el código, permitidme explicar qué estamos a punto de hacer. Este archivo es como el director de la orquesta, indicando a Summernote cómo comportarse y qué hacer cuando le entregamos un nuevo trozo de información. Así que, sin más preámbulos, echémosle un vistazo:

```javascript
function AddNewSummerNoteDescription(Data) {
    var controlAddIn = document.getElementById("controlAddIn");
    controlAddIn.innerHTML = '<textarea class="summernote" id="summernote"></textarea>';
    CreateSummerNoteDescription(Data);
}

function CreateSummerNoteDescription(Data, JsonMention) {
    //Initialize editor only once when DOM is loaded
    $(document).ready(function () {
        $(".summernote").summernote({
            height: $(window).height() - 100,
            focus: true,
            lang: "es-ES",
            //Esto permitirá tabular a través de los campos en Formularios.
            tabDisable: true,
        });
        $(".note-editable").html(Data);
    });
}
```

<br>

Vamos por partes:

-   **AddNewSummerNoteDescription(Data)**: Esta función es la que llamamos desde nuestro ControlAddIn cuando queremos agregar nuevo contenido a Summernote. Primero, obtenemos el elemento ControlAddIn, luego le añadimos un área de texto Summernote con el famoso ID "summernote" y finalmente, llamamos a la función CreateSummerNoteDescription(Data).

-   **CreateSummerNoteDescription(Data)**: Esta es la función que realmente inicia el Summernote. Aquí estamos inicializando el editor de texto enriquecido solo cuando el DOM está completamente cargado. Configuramos algunas propiedades básicas, como la altura, el idioma y la desactivación de la tecla tabulador. Y finalmente, ¡añadimos el contenido que le pasamos al Summernote!

<br><br>

## Integrando en una Página: Pedido de Venta

Esta parte es crucial porque es donde conectamos nuestro brillante editor de texto enriquecido Summernote con una página específica de Business Central.

Aquí estamos creando una extensión de página específica para el "Pedido de Venta". Estamos diciendo: "Oye, queremos agregar algo especial a la página de detalles del pedido de venta".

Aquí tenéis el código y la explicación:

```javascript
pageextension 50000 "Card Sales Order" extends "Sales Order"
{
    layout
    {
        addafter(General)
        {
            group(ControlAddins1)
            {
                Caption = 'Description';
                usercontrol(SummernoteDescription; "SummerNote")
                {
                    ApplicationArea = All;

                    trigger ControlAddInReady()
                    begin
                        CurrPage.SummernoteDescription.AddNewSummerNoteDescription(NewData);
                    end;
                }
            }
        }
    }
}


```

<br>

Explicación detallada:

-   **usercontrol(SummernoteDescription; "SummerNote"):** Aquí es donde estamos incorporando nuestro control Summernote y le damos el nombre "SummernoteDescription".

-   **trigger ControlAddInReady():** Este evento se activa cuando el ControlAddIn (Summernote en este caso) está listo para funcionar.

-   **CurrPage.SummernoteDescription.AddNewSummerNoteDescription(NewData);:** Esto es como decir: "¡Oye, SummernoteDescription (nuestro control Summernote), agrega algo nuevo!".

<br>

Como siempre, puedes encontrar el ejemplo completo en [GitHub](https://github.com/Esanpons/ControlAddIns-Business-Central)

<br>

Ahí lo tenéis, un vistazo detallado a la implementación de Summernote en Business Central. Este editor de texto enriquecido abre un mundo de posibilidades creativas para vuestros usuarios. Estoy emocionado por las oportunidades que esto nos brinda. ¡Estad atentos para más trucos y consejos, porque esto no acaba aquí!
