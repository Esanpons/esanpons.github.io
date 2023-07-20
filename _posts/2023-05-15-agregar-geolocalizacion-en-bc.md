---
layout: post
title: "Cómo agregar geolocalización a un ControlAddin de Business Central"
summary: "Agregar geolocalización a un ControlAddin de Business Central para obtener la ubicación del usuario mediante Javascript y mostrarla en la interfaz."
author: "Esteve Sanpons"
date: "2023-05-15 08:00:00 +0200"
#cSpell:disable
category: ["JavaScript", "ControlAddin", "Business_Central"]
thumbnail: /assets/img/posts/agregar-geolocalizacion-en-bc/imagen01.png
permalink: /blog/agregar-geolocalizacion-en-bc/
custom_type: Blog
#cSpell:enable
---

Buenas a todos.

La geolocalización se ha convertido en una funcionalidad cada vez más importante en las aplicaciones de negocio, ya que permite la identificación precisa de la ubicación de un usuario. En este blog, te mostrare cómo agregar la geolocalización a un ControlAddin de AL Business Central utilizando código JavaScript.

Antes de comenzar, es importante tener en cuenta que para utilizar la geolocalización en un ControlAddin de AL Business Central, debes incluir el código JavaScript, html necesario.

Bueno, después de la breve explicación, vamos manos a la obra 😎

Lo primero que haremos es crear la estructura de los archivos.
En el proyecto añadimos los objetos tal cual te los muestro a continuación.

-   controladdin
    -   Geolocalizacion
        -   html
            -   index.html
        -   js
            -   script.js
            -   startup.js
        -   Geolocalizacion.ControlAddin.al
-   page
    -   Geolocalizacion.Page.al

<br>

Vamos a empezar por el ControlAddin, este sera sencillo y solo ara referencia a los archivos js y html y a la librería que requerimos que es jquery.

También crearemos en evento y la función para ser llamados.

```javascript

controladdin "Geolocalizacion"
{
    RequestedHeight = 250;
    RequestedWidth = 500;
    StartupScript = 'src/controlAddIn/Geolocalizacion/js/StarUp.js';
    Scripts =
        'src/controlAddIn/Geolocalizacion/js/Script.js',
        'https://code.jquery.com/jquery-3.6.0.min.js';

    Images = 'src/controlAddIn/Geolocalizacion/html/index.html';

    event ControlAddInReady();

    procedure init();
}

```

<br>

El archivo startup es sencillo y como siempre es lo mismo.

```javascript
Microsoft.Dynamics.NAV.InvokeExtensibilityMethod("ControlAddInReady", null);
```

<br>

El html esta vez también sera sencillo, lo único que hacemos es agregar un div para poder ver el resultado y un boton para ejecutar todo.

```html
<div id="ubicacion"></div>
<button id="boton-ubicacion">Obtener ubicación</button>
```

<br>

Ahora viene la parte interesante, el JavaScript que geolocaliza nuestra posición.

```javascript
function init() {
    var controlAddIn = $("#controlAddIn");
    controlAddIn.empty();

    $.get(
        Microsoft.Dynamics.NAV.GetImageResource("src/controlAddIn/Geolocalizacion/html/index.html"),
        function (htmlExterno) {
            controlAddIn.html(htmlExterno);
            console.log(htmlExterno);
            obtenerUbicacion();
        }
    );
}

function obtenerUbicacion() {
    const ubicacionDiv = document.getElementById("ubicacion");

    navigator.geolocation.getCurrentPosition(
        (position) => {
            const { latitude, longitude } = position.coords;
            ubicacionDiv.innerHTML = `Tu ubicación es: Latitud ${latitude}, Longitud ${longitude}`;
        },
        (error) => {
            console.error(error);
            alert("Hubo un error al obtener la ubicación.");
        },
        {
            enableHighAccuracy: true,
            maximumAge: 0,
            timeout: 5000,
        }
    );
}

const boton = document.getElementById("boton-ubicacion");
boton.addEventListener("click", obtenerUbicacion);
```

En el código anterior, la función init() se encarga de inicializar el ControlAddin y de obtener la ubicación del usuario. La función obtenerUbicacion() utiliza la API de geolocalización del navegador para obtener la posición actual del usuario y mostrarla en la página.

es muy importante que le permitamos que nos geolocalice cuando nos lo pida el explorador yaq ue si no dará error.

<br>

Ahora, vamos a proceder a crear la página donde se utilizará nuestro ControlAddin. Para ello, creamos un objeto Page con el nombre "Geolocalización".

En este objeto, definimos el tipo de página, la categoría de uso, el área de aplicación, el título y una descripción. Además, agregamos un layout para poder visualizar nuestro ControlAddin.

```javascript

page 60002 "Geolocalizacion"
{
    PageType = Card;
    UsageCategory = Administration;
    ApplicationArea = All;
    Caption = 'Geolocation', comment = 'ESP="Geolocalizacion"';

    layout
    {
        area(Content)
        {

            usercontrol(Geolocalizacion; "Geolocalizacion")
            {
                ApplicationArea = All;
                trigger ControlAddInReady()
                begin
                    CurrPage.Geolocalizacion.Init();
                end;
            }
        }
    }
}

```

<br>

Con esto, nuestra página estará lista para utilizar el ControlAddin que hemos creado previamente.

Ahora, podemos desplegar nuestra extensión en Business Central y ver cómo funciona nuestro ControlAddin.

En resumen, hemos creado un ControlAddin utilizando JavaScript y lo hemos integrado en Business Central. Con esto, hemos podido utilizar la geolocalización en nuestra aplicación y mostrarla en una página de Business Central.

<br>
<br>

Como siempre podréis ver el ejemplo entero en el [Link](https://github.com/Esanpons/ControlAddIn-Basico-BC)
