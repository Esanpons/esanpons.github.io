---
layout: post
title: "Aprende a firmar documentos digitalmente en segundos des de Business Central"
summary: "Descubre cómo agregar una ventana de firma digital en tu proyecto y guardar firmas en Base64."
author: "Esteve Sanpons"
date: "2023-05-27 01:00:00 +0200"
#cSpell:disable
category: ["ControlAddin", "Business_Central"]
thumbnail: /assets/img/posts/firmar-en-bc/imagen01.jpg
permalink: /blog/firmar-en-bc/
custom_type: Blog
#cSpell:enable
---

Hola a todos, una semana más!

Hoy quiero compartir con vosotros un descubrimiento que hice recientemente y que estoy seguro de que os resultará muy útil.

¿Alguna vez habéis necesitado firmar un documento y no sabíais cómo hacerlo de manera fácil y rápida? ¡Pues no os preocupéis más! En este post os mostraré una forma sencilla de crear una ventana donde cualquier persona puede firmar digitalmente, y luego podréis guardar esa firma en el formato Base64 y agregarla donde queráis.

Bueno, dejémonos de explicaciones y vamos ¡manos a la obra! :muscle:

<br>

Lo primero que debemos hacer es crear nuestro proyecto. En este caso, utilizaremos un ControlAddin que contendrá archivos de CSS, HTML y JavaScript.
También añadiremos una página al proyecto para mostrar el lienzo donde se podrá firmar.

<br>

### Archivo HTML

Comenzaremos por el archivo HTML. Aquí encontraremos un lienzo y un botón. El botón nos permitirá guardar la firma, mientras que el lienzo será el lugar donde firmaremos.

```html
<canvas id="signatureCanvas" width="400" height="200"></canvas>
<br />
<button onclick="guardarFirma()">Guardar firma</button>
```

<br> <br>
<br>

### Archivo CSS

A continuación, pasemos al archivo CSS. Este archivo define el estilo del lienzo y asegura que no se vea afectado por gestos táctiles no deseados.

```css
#signatureCanvas {
  border: 1px solid #000;
  touch-action: none;
}
```

<br> <br>
<br>

### Archivo Scripts.js

Ahora llegamos al archivo más importante. Aquí se encuentra toda la funcionalidad necesaria para agregar el HTML al ControlAddin y hacer que todo funcione correctamente.

En este archivo, inicializamos algunas variables y creamos una función de inicialización que se encargará de agregar el HTML y configurar el lienzo donde se realizará la firma.

```javascript
var signPad;
var controlAddIn;
var Html;

function Init() {
  var getHtml = $.get(
    Microsoft.Dynamics.NAV.GetImageResource(
      "src/controlAddIn/Sign/html/HtmlSign.html"
    ),
    function (htmlExterno) {
      Html = htmlExterno;
      console.log(Html);
    }
  );

  getHtml.done(function () {
    controlAddIn = $("#controlAddIn");
    controlAddIn.empty();
    controlAddIn.html(Html);

    initSignaturePad();
  });
}

function initSignaturePad() {
  var canvas = document.getElementById("signatureCanvas");
  signPad = new SignaturePad(canvas, {
    backgroundColor: "rgb(255, 255, 255)", // Establecer el color de fondo del canvas
  });
}
```

<br> <br>

Además, en este archivo, creamos una función para guardar la firma, que estará vinculada al botón en el HTML. Esta función verifica si se ha realizado alguna firma en el lienzo y, de ser así, la guarda en formato Base64. Luego, se reinicia el lienzo para poder realizar más firmas en el futuro.

```javascript
function guardarFirma() {
  if (signPad.isEmpty()) {
    alert("Por favor, firma antes de guardar.");
  } else {
    var imageData = signPad.toDataURL().split(",")[1]; // Obtener solo los datos de la imagen sin el encabezado
    var signatureBase64 = btoa(imageData); // Decodificar los datos en base64

    console.log("Signatura en Base 64", signatureBase64);

    Microsoft.Dynamics.NAV.InvokeExtensibilityMethod(
      "SaveSign",
      [signatureBase64],
      true
    );

    initSignaturePad();
  }
}
```

<br> <br>
<br>

### Archivo ControlAddin

Pasemos ahora al archivo ControlAddin. Aquí configuraremos los archivos necesarios para el ControlAddin, incluyendo las librerías adicionales requeridas para su correcto funcionamiento.

También definiremos los eventos y las funciones necesarias para la interacción entre Business Central y el ControlAddin.

```javascript
controladdin "Sign"
{
    Scripts =
        'https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.6.0.js',
        'https://cdnjs.cloudflare.com/ajax/libs/signature_pad/1.5.3/signature_pad.min.js',
        'src/controlAddIn/Sign/js/Scripts.js';

    StyleSheets =
          'src/controlAddIn/Sign/css/Style.css';

    StartupScript =
        'src/controlAddIn/Sign/js/Startup.js';

    Images = 'src/controlAddIn/Sign/html/HtmlSign.html';

    RequestedHeight = 350;
    MinimumHeight = 1;
    MinimumWidth = 250;
    HorizontalShrink = true;
    HorizontalStretch = true;

    procedure Init();
    event ControlAddInReady();
    event SaveSign(Base64Sign: Text);
}
```

<br> <br>
<br>

### Page para firmar

Por último, crearemos la página para firmar. Esta será una página de tipo "card" donde añadiremos y ejecutaremos nuestro ControlAddin. También configuraremos los eventos de inicialización y guardado.

```javascript
page 60003 "Card Sign"
{
    PageType = Card;
    UsageCategory = Administration;
    ApplicationArea = All;
    Caption = 'Card To Sign', Comment = 'ESP="Ficha para firmar"';

    layout
    {
        area(Content)
        {
            usercontrol(Sign; "Sign")
            {
                ApplicationArea = All;

                trigger ControlAddInReady()
                begin
                    CurrPage.Sign.Init();
                end;

                trigger SaveSign(Base64Sign: Text)
                begin
                    Message(Base64Sign);
                end;
            }
        }
    }
}
```

<br> <br>

Como podéis ver, hemos creado un cuadrado con un botón dentro del mismo, donde se puede realizar una firma digital! Después de presionar el botón, la firma se guarda y se puede utilizar como deseen. En el ejemplo, la muestro a través de un mensaje.

<br>
<br>

Como siempre, podréis ver el ejemplo entero en el [Link](https://github.com/Esanpons/ControlAddIn-Basico-BC)

¡Espero que os resulte útil y emocionante! No dudéis en enviar vuestros comentarios y preguntas. ¡Hasta la próxima!
