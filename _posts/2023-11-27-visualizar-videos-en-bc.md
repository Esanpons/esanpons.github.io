---
layout: post
title: "Transformando la Gestión de Archivos en Business Central: ¡Arrastra y Suelta para la Eficiencia!"
summary: "Descubre cómo revolucionar la administración de archivos en Dynamics 365 Business Central mediante la implementación de una funcionalidad de arrastrar y soltar. En este artículo, exploramos el código y los componentes esenciales que te permitirán simplificar la gestión de documentos adjuntos."
author: "Esteve Sanpons"
date: "2023-11-26 04:00:00 +0200"
#cSpell:disable
category: ["JavaScript", "ControlAddin", "Business_Central"]
thumbnail: /assets/img/posts/visualizar-videos-en-bc/imagen01.png
permalink: /blog/visualizar-videos-en-bc/
custom_type: Blog
#cSpell:enable
---

¡Bienvenidos a un nuevo artículo en nuestro blog sobre el desarrollo en Business Central! Hoy, vamos a sumergirnos en el emocionante mundo de la integración de contenido de video en tus aplicaciones personalizadas a través de un ControlAddIn.

Este componente personalizado te permitirá llevar la visualización de videos a otro nivel en Business Central, ya sea desde YouTube o tus propios recursos locales.

<br>

Así que, sin más preámbulos, ¡Vamos manos a la obra! 😍

<br>

## ControlAddIn "ViewVideo"

El fichero del ControlAddIn "ViewVideo" es el corazón de nuestro proyecto. Aquí encontrarás todos los ajustes esenciales que definen cómo se comportará nuestro control. Es como el cerebro detrás de la operación de mostrar videos en tu aplicación. Establece su nombre, tamaño inicial, estilo, scripts, recursos y eventos clave, como la función "CargarVideo" que utilizaremos para cargar contenido de video.

-   `event ControlAddInReady()`: Aquí, definimos un evento especial que se desencadena cuando el ControlAddIn está listo para funcionar.
-   `procedure CargarVideo()`: Esta función es esencial; la utilizaremos más adelante para cargar el contenido del video en el control.

```javascript
controladdin "ViewVideo"
{
    RequestedHeight = 1000;
    RequestedWidth = 300;
    VerticalStretch = true;
    VerticalShrink = true;
    HorizontalStretch = true;
    HorizontalShrink = true;

    Scripts = 'ViewVideo/src/controlAddIn/ViewVideo/js/Script.js',
                'https://code.jquery.com/jquery-3.6.0.min.js';
    StartupScript = 'ViewVideo/src/controlAddIn/ViewVideo/js/Startup.js';
    Images = 'ViewVideo/src/controlAddIn/ViewVideo/html/HtmlHeader.html', 'ViewVideo/src/controlAddIn/ViewVideo/video/VideoEjemplo.mp4';

    event ControlAddInReady();

    procedure CargarVideo();
}


```

<br><br>

## Archivo Script.js

El archivo JavaScript es donde reside la magia que permite cargar videos en el control. Echemos un vistazo a lo que contiene:

-   `var Html` y `var controlAddIn`: Estas son variables globales que utilizamos para almacenar el contenido HTML y el control ControlAddIn.
-   `function CargarVideo()`: Esta función es la estrella del espectáculo; se encarga de cargar el contenido del video. Utiliza jQuery para cargar contenido HTML externo desde 'ViewVideo/src/controlAddIn/ViewVideo/html/HtmlHeader.html' y establecer la fuente del video desde 'ViewVideo/src/controlAddIn/ViewVideo/video/VideoEjemplo.mp4'.

```javascript
var Html;
var controlAddIn;

function CargarVideo() {
    $("#controlAddIn").load(
        Microsoft.Dynamics.NAV.GetImageResource("ViewVideo/src/controlAddIn/ViewVideo/html/HtmlHeader.html"),
        function (htmlExterno) {
            Html = htmlExterno;

            var video = $("#video-local");
            video.attr(
                "src",
                Microsoft.Dynamics.NAV.GetImageResource("ViewVideo/src/controlAddIn/ViewVideo/video/VideoEjemplo.mp4")
            );
        }
    );
}
```

<br><br>

## Archivo HTML

El archivo HTML proporciona la estructura que se utilizará para mostrar el video en el control. Veamos lo que contiene:

-   `<video>`: Aquí definimos el reproductor de video local. Tiene controles de reproducción y una fuente de video con un atributo "id" que se actualizará automáticamente desde el archivo JavaScript.

-   `<iframe>`: Aquí encontramos el reproductor de video de YouTube incrustado, con un ejemplo de video de YouTube.

```html
<h2>Video Insertado en el proyecto</h2>
<video controls width="560" height="315">
    <source id="video-local" , src="" type="video/mp4" />
</video>

<h2>Video de YouTube</h2>
<iframe
    width="560"
    height="315"
    src="https://www.youtube.com/embed/mPJNGqKCUGg?si=uSyyn3m_Vy89uUi4"
    title="Video de YouTube"
    frameborder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowfullscreen
></iframe>
```

<br><br>

## PageExtension CustomerCard extends "Customer Card"

Por último, hemos integrado nuestro control "ViewVideo" en la página "Customer Card" mediante una extensión de página. Cuando el control esté listo (`ControlAddInReady()`), llamamos a la función `CargarVideo()` para cargar el contenido del video en la página del cliente.

```javascript
pageextension 50100 CustomerCard extends "Customer Card"
{
    layout
    {
        addfirst(General)
        {
            usercontrol(ViewVideo; ViewVideo)
            {
                ApplicationArea = All;

                trigger ControlAddInReady()
                begin
                    CurrPage.ViewVideo.CargarVideo();
                end;
            }
        }
    }
}
```

<br><br>

Como siempre, puedes encontrar el ejemplo completo en [GitHub](https://github.com/Esanpons/ControlAddIns-Business-Central)

<br>

En resumen, nuestro ControlAddIn "ViewVideo" es una herramienta poderosa para enriquecer sus aplicaciones de Business Central con la capacidad de mostrar videos locales y de YouTube. Si tienes alguna pregunta o comentario sobre este emocionante desarrollo, ¡no dudes en dejarlo en los comentarios! Estamos aquí para ayudarte en tu viaje de desarrollo en Business Central.
