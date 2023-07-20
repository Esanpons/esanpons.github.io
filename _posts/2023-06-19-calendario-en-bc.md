---
layout: post
title: "Creación de calendarios personalizados en Business Central utilizando JavaScript"
summary: "Aprende cómo utilizar JavaScript y la librería FullCalendar para crear calendarios personalizados en tu aplicación de Business Central. Descubre cómo visualizar y gestionar datos de manera eficiente utilizando esta poderosa herramienta. Mejora la experiencia de usuario y maximiza el potencial de tu ERP con calendarios interactivos y personalizables."
author: "Esteve Sanpons"
date: "2023-06-19 04:00:00 +0200"
#cSpell:disable
category: ["JavaScript", "ControlAddin", "Business_Central"]
thumbnail: /assets/img/posts/calendario-controladdin-en-bc/imagen01.jpg
permalink: /blog/calendario-controladdin-en-bc/
custom_type: Blog
#cSpell:enable
---

¡Hola a todos!

¡Bienvenidos una semana más a mi blog!

Hoy vamos a explorar cómo crear y utilizar calendarios en JavaScript para visualizar datos desde una ventana de Business Central.

Para este propósito, utilizaremos la librería FullCalendar, que puedes encontrar en el siguiente enlace: [Link](https://fullcalendar.io/)

<br>

Con esta librería y realizando algunas modificaciones en Business Central, podemos tener un calendario como el que se muestra en la siguiente imagen:

<input type="checkbox" id="image-checkbox-02" class="image-checkbox">
<label for="image-checkbox-02"  class="image-label">
    <img class="img-container" src="/assets/img/posts/calendario-controladdin-en-bc/imagen02.png">
</label>

<br><br><br>

Vamos a poner ¡manos a la obra! 💪

<br>

Lo primero que haremos es crear la estructura del ControlAddin, como siempre. En el proyecto, añadimos los objetos de la siguiente manera:

-   controladdin
    -   Calendar
        -   css
            StyleSheets.css
        -   html
            -   main.html
        -   js
            -   script.js
            -   startup.js
        -   Calendar.ControlAddin.al
-   page
    -   Calendar.Page.al

<br><br>

El primer archivo que rellenaremos será el del propio ControlAddin:

```javascript
controladdin "Calendar"
{
    MinimumHeight = 500;
    MinimumWidth = 250;
    HorizontalShrink = true;
    HorizontalStretch = true;
    VerticalShrink = true;
    VerticalStretch = true;

    StartupScript = 'src\controladdin\Calendar\js\startup.js';

    StyleSheets = 'src\controladdin\Calendar\css\StyleSheets.css',
                    'https://cdnjs.cloudflare.com/ajax/libs/fullcalendar/3.10.2/fullcalendar.min.css';

    Scripts = 'https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.min.js',
                'https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.22.2/moment.min.js',
                'https://cdnjs.cloudflare.com/ajax/libs/fullcalendar/3.10.2/fullcalendar.min.js',
                'https://cdn.jsdelivr.net/npm/fullcalendar@3.10.2/dist/locale/es.js',
                'src\controladdin\Calendar\js\Scripts.js';

    Images = 'src\controladdin\Calendar\html\main.html';

    event ControlAddInReady();
    procedure InitCalendar();

}
```

<br>

En este archivo, podemos ver que la mayoría de los componentes son scripts que necesitaremos para la creación del calendario. También se define el evento de inicio y la función para crear el calendario.

<br><br><br>

A continuación, tenemos el archivo HTML, que es sencillo y contiene únicamente los elementos <div> del calendario y las leyendas de colores:

```html
<div id="legend1" class="legend"></div>
<div id="legend2" class="legend"></div>
<div id="calendar"></div>
```

<br><br><br>

El archivo CSS se utiliza para los estilos de las leyendas, colocándolas en el centro y un dato al lado del otro:

```css
.legend {
    display: flex;
    justify-content: center;
    margin-bottom: 20px;
}

.legend-item {
    display: flex;
    align-items: center;
    margin-right: 20px;
}

.legend-color {
    width: 20px;
    height: 20px;
    margin-right: 5px;
}

.legend-label {
    font-size: 14px;
}
```

<br><br><br>

Ahora viene la parte interesante, que es el archivo JavaScript. En este archivo, para explicar el proceso, crearemos 3 objetos JSON como variables. Sin embargo, lo ideal sería obtener estos datos directamente de alguna tabla de Business Central.

Primero, creamos estas variables. La primera se utilizará para rellenar y dar formato con colores a las entradas del calendario, y las otras dos para las leyendas de colores:

```javascript
var events = [
    {
        title: "Event 1",
        start: "2023-06-01",
        end: "2023-06-03",
        color1: "red",
        color2: "blue",
        textColor: "white",
    },
    {
        title: "Event 2",
        start: "2023-06-05",
        end: "2023-06-05",
        color1: "green",
        color2: "yellow",
        textColor: "black",
    },
];

var legendItems = [
    {
        name: "Color 2: Event 1",
        color: "blue",
    },
    {
        name: "Color 2: Event 2",
        color: "yellow",
    },
];

var legendItems2 = [
    {
        name: "Color 1: Event 1",
        color: "red",
    },
    {
        name: "Color 1: Event 2",
        color: "green",
    },
];
```

<br><br>

Ahora vamos a crear la función para inicializar el calendario:

```javascript
function InitCalendar() {
    var getHtml = $.get(
        Microsoft.Dynamics.NAV.GetImageResource("src/controladdin/Calendar/html/main.html"),
        function (htmlExterno) {
            Html = htmlExterno;
            console.log(Html);
        }
    );

    getHtml.done(function () {
        controlAddIn = $("#controlAddIn");
        controlAddIn.empty();
        controlAddIn.html(Html);

        var calendarOptions = {
            height: $(window).height() * 0.93,
            initialView: "dayGridMonth",
            locale: "es",
            timeZone: "Europe/Madrid",
            events: events,
            eventRender: function (event, element) {
                var gradient =
                    "linear-gradient(to right, " +
                    event.color1 +
                    ", " +
                    event.color1 +
                    " 15px, " +
                    event.color2 +
                    " 15px, " +
                    event.color2 +
                    ")";
                element.css("background", gradient);

                var titleElement = element.find(".fc-title");
                titleElement.css("padding-left", "25px");
                titleElement.css("color", event.textColor);
            },
        };

        $("#calendar").fullCalendar(calendarOptions);

        // Generar la leyenda de colores dinámicamente
        var legendElement = $("#legend1");
        legendItems.forEach(function (item) {
            var legendItem = $("<div>", { class: "legend-item" });
            var legendColor = $("<div>", { class: "legend-color" }).css("background-color", item.color);
            var legendLabel = $("<div>", { class: "legend-label" }).text(item.name);
            legendItem.append(legendColor, legendLabel);
            legendElement.append(legendItem);
        });

        var legendElement2 = $("#legend2");
        legendItems2.forEach(function (item) {
            var legendItem = $("<div>", { class: "legend-item" });
            var legendColor = $("<div>", { class: "legend-color" }).css("background-color", item.color);
            var legendLabel = $("<div>", { class: "legend-label" }).text(item.name);
            legendItem.append(legendColor, legendLabel);
            legendElement2.append(legendItem);
        });
    });
}
```

<br>

En esta función, lo primero que hacemos es cargar el HTML y luego inicializar el ControlAddin. Después, creamos una variable con las opciones para el calendario, como el tamaño, la vista inicial, el idioma y el JSON que creamos anteriormente.

En esta parte, me gustaría añadir que la idea era que el fondo del evento fuera de dos colores y que tuvieran un color de letra diferente.

Por último, el código genera dinámicamente las leyendas de colores según los JSON que tenemos como variables.

<br><br><br>

La página es muy sencilla, ya que solo tenemos que llamar al ControlAddin para que pinte y coloree todo lo que hemos configurado:

```javascript

page 60004 "Calendar"
{
    PageType = Card;
    ApplicationArea = All;
    UsageCategory = Administration;

    layout
    {
        area(Content)
        {
            usercontrol(Calendar; Calendar)
            {
                ApplicationArea = All;
                trigger ControlAddInReady()
                begin
                    CurrPage.Calendar.InitCalendar();
                end;
            }
        }
    }
}

```

<br><br><br>

El resultado final es el que se muestra en la imagen anterior. Este enfoque tiene muchas posibilidades y se puede obtener mucho beneficio en cualquier ámbito de nuestro ERP.

En resumen, en este post hemos explorado cómo crear y utilizar calendarios mediante JavaScript para visualizar datos desde una ventana de Business Central. Para lograrlo, hemos utilizado la librería FullCalendar.

En general, este enfoque ofrece muchas posibilidades y puede ser beneficioso en diferentes contextos dentro de nuestro Business Central.

<br><br>

Como siempre, podréis ver el ejemplo entero en el [Link](https://github.com/Esanpons/ControlAddIn-Basico-BC)

<br>

¡Espero que esta guía os sea útil y os inspire a implementar calendarios personalizados en Business Central! Si tenéis alguna pregunta o comentario, no dudéis en comentarlo. ¡Hasta la próxima semana!
