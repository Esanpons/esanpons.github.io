---
layout: post
title: "¡Da Vida a tus Datos con Gráficos Impresionantes en Business Central!"
summary: "Descubre cómo transformar tus datos aburridos en visualizaciones sorprendentes en Business Central utilizando ControlAddIns. En este emocionante recorrido, te guiaré a través del código que te permitirá crear gráficos impactantes con JavaScript, CSS y HTML."
author: "Esteve Sanpons"
date: "2029-01-01 08:00:00 +0200"
#cSpell:disable
category: ["JavaScript", "ControlAddin", "Business_Central"]
thumbnail: /assets/img/posts/graficos-basicos-en-bc/imagen01.png
permalink: /blog/graficos-basicos-en-bc/
custom_type: Blog
#cSpell:enable
---

¡Saludos, a todos los entusiastas de la programación y seguidores incondicionales de Business Central! ¡Hoy estamos listos para embarcarnos en una emocionante aventura llena de código y gráficos espectaculares!

Así que, si estás emocionado por sumergirte en esta travesía emocionante, prepárate para descubrir cómo darle un toque especial a tus aplicaciones utilizando ControlAddIns.

Si has estado siguiendo mis blogs anteriores, ya sabrás que los ControlAddIns son como los ingredientes secretos que añaden sabor a tus desarrollos en Business Central. Pero hoy, vamos a llevar las cosas a otro nivel al explorar cómo elevar tus aplicaciones con gráficos sorprendentes creados con JavaScript, CSS y HTML. ¡Estás a punto de aprender cómo transformar datos aburridos en gráficos visuales impresionantes que te harán destacar!

Ahora, antes de sumergirnos en el mundo del código, asegúrate de tener tu carpeta de ControlAddIn bien organizada, porque vamos a dar vida a esos datos y convertirlos en algo sorprendente.

<br>

Venga dejémonos de explicaciones y vamos manos a la obra. 😁

<br>

Antes de zambullirnos en el código, asegúrate de tener tu carpeta de ControlAddIn en orden, porque vamos a darle vida a esos datos y hacer que cobren forma.

-   controladdin
    -   Graphics
        -   html
            -   init.html
        -   js
            -   script.js
        -   Geolocalizacion.ControlAddin.al
-   page
    -   Graphic.ControlAddin.Page.al

<br>

APero primero, echemos un vistazo a la configuración clave en el archivo ControlAddIn. Aquí es donde se configuran las bases para crear verdaderas obras maestras visuales.

```javascript
controladdin "Graphic"
{
    RequestedHeight = 250;
    RequestedWidth = 500;
    StartupScript = 'src/controlAddIn/Graphics/js/StarUp.js';
    Scripts =
        'src/controlAddIn/Graphics/js/Script.js',
        'https://cdn.jsdelivr.net/npm/chart.js@latest/dist/Chart.min.js',
        'https://code.jquery.com/jquery-3.6.0.min.js';
    Images = 'src/controlAddIn/Graphics/html/Init.html';

    event ControlAddInReady();
    procedure Init(etiquetas: JsonArray; data: JsonArray; TituloData: Text);

}
```

<br><br>

¿Lo ves? No solo estamos configurando las dimensiones para que nuestros gráficos luzcan geniales, sino que también estamos trayendo algunos aliados de peso: ¡jQuery y Chart.js! ¡Son como la pareja dinámica que hará que tus gráficos brillen como nunca antes!

Dentro del archivo HTML, simplemente añadimos una línea que define dónde se ubicará nuestro gráfico.

```javascript
<canvas id="grafica"></canvas>
```

<br><br>

Y ahora, el plato principal: el archivo Script.js. Aquí es donde despegamos en nuestra aventura gráfica. Creamos la función Init, que es el punto de partida de esta emocionante travesía. ¿Adivina qué? ¡Esta función es tu pasaporte para personalizar tus gráficos al máximo! 🎨

```javascript
var datos;

function Init(etiquetas, data, TituloData) {
    var controlAddIn = $("#controlAddIn");
    controlAddIn.empty();

    datos = {
        label: TituloData,
        data: data,
        backgroundColor: "rgba(54, 162, 235, 0.2)", // Color de fondo
        borderColor: "rgba(54, 162, 235, 1)", // Color del borde
        borderWidth: 1, // Ancho del borde
    };

    $.get(Microsoft.Dynamics.NAV.GetImageResource("src/controlAddIn/Graphics/html/Init.html"), function (htmlExterno) {
        controlAddIn.html(htmlExterno);
        console.log(htmlExterno);
        CrearGrafica(etiquetas);
    });
}
```

<br><br>

Init: Aquí es donde comienza todo. Esta función recibe tres ingredientes clave: etiquetas (las etiquetas para el eje x del gráfico), datos (los valores para el eje y) y TituloData (ese título deslumbrante que quieres en la cima del gráfico). ¡Esta es tu oportunidad de hacer que tus gráficos sean únicos y espectaculares!

Dentro de Init, creamos el objeto datosGrafico. Este objeto lleva consigo todos los detalles esenciales del gráfico: el título, los datos, el color de fondo y el color del borde. Pero aquí viene la chispa: ¡usamos jQuery para traer un archivo HTML externo y lo insertamos en el elemento con el id controlAddIn! ¡Así de fácil! Y para darle el toque final, llamamos a la función CrearArte para que el espectáculo gráfico comience.

Ahora, ¿cómo hacemos que los datos cobren vida en forma de gráfico? ¡Aquí es donde entra en juego la función CrearGrafica:

```javascript
function CrearGrafica(etiquetas) {
    // Obtener una referencia al elemento canvas del DOM
    const grafica = document.querySelector("#grafica");

    new Chart(grafica, {
        type: "bar", // Tipo de gráfica
        data: {
            labels: etiquetas,
            datasets: [datos],
        },
        options: {
            scales: {
                yAxes: [
                    {
                        ticks: {
                            beginAtZero: true,
                        },
                    },
                ],
            },
        },
    });
}
```

<br><br>

CrearGrafica: Este es el momento culminante, donde los datos dan un salto al escenario en forma de un gráfico visualmente impactante. Aquí es donde Chart.js hace su entrada triunfal. Usando esta biblioteca, creamos un objeto Chart en el lienzo (canvas) de nuestro HTML. Y aquí viene lo emocionante: ¡configuramos el tipo de gráfico que deseamos! ¿Barras, líneas, burbujas o quizás una torta tentadora? ¡Tienes el control para experimentar y hacer que tus gráficos sean verdaderas obras maestras!

En el objeto Chart, establecemos los datos del gráfico. Las etiquetas son para el eje x y el datosGrafico que creamos en Init se utiliza para el eje y. Además, afinamos las opciones para que el eje y comience desde cero y se ajuste a la escala adecuada. ¡Y ahí lo tienes, el momento en que los datos se transforman en arte gráfico!

<br><br>

Solo tenemos que añadirlo a una page para poder ver el resultado

```javascript
page 60000 "Graphics"
{

    PageType = Card;
    UsageCategory = Administration;
    ApplicationArea = All;
    Caption = 'Graphics', comment = 'ESP="graficos"';

    layout
    {
        area(Content)
        {

            usercontrol(Graphics; "Graphic")
            {
                ApplicationArea = All;
                trigger ControlAddInReady()
                var
                    JArrayEtiquetas: JsonArray;
                    JArrayData: JsonArray;
                    TitulosLbl: Label 'Sales', Comment = 'ESP="Ventas"';
                begin
                    JArrayData.Add(1500);
                    JArrayData.Add(200);
                    JArrayData.Add(8250);
                    JArrayData.Add(5001);

                    JArrayEtiquetas.Add('Enero');
                    JArrayEtiquetas.Add('Febreo');
                    JArrayEtiquetas.Add('Marzo');
                    JArrayEtiquetas.Add('Abril');

                    CurrPage.Graphics.Init(JArrayEtiquetas, JArrayData, TitulosLbl);
                end;
            }
        }
    }

}
```

<br><br>

He preferido que los datos lo podamos insertar des de Business Central.

El resultado es espectacular.

<input type="checkbox" id="image-checkbox-02" class="image-checkbox">
<label for="image-checkbox-02"  class="image-label">
    <img class="img-container" src="/assets/img/posts/graficos-basicos-en-bc/imagen02.png">
</label>
<br><br>

Como siempre, puedes encontrar el ejemplo completo en [GitHub](https://github.com/Esanpons/ControlAddIns-Business-Central)

<br>

Recuerda, lo que has explorado aquí es solo el principio. Puedes personalizar, experimentar y explorar diferentes tipos de gráficos para que se adapten a tus necesidades y proyectos. ¡Tienes el poder de crear gráficos que deslumbren y causen impacto! Así que, ¡hasta la próxima, creador de arte gráfico, y que tus gráficos sean legendarios!
