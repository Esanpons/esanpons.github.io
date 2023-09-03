---
layout: post
title: "Innovación en Business Central: Generación de Imágenes con ChatGPT"
summary: "Explora cómo hemos llevado Business Central al siguiente nivel al desafiar a ChatGPT para crear imágenes personalizadas en nuestra interfaz de usuario."
author: "Esteve Sanpons"
date: "2023-09-17 04:00:00 +0200"
#cSpell:disable
category: ["Business_Central", ChatGPT, HttpClient, JavaScript, ControlAddin]
thumbnail: /assets/img/posts/chatgpt-imagen/imagen01.png
permalink: /blog/chatgpt-imagen/
custom_type: Blog
#cSpell:enable
---

¡Hola a todos los apasionados de Business Central! Hoy estamos a punto de embarcarnos en un emocionante proyecto que nos llevará más allá de las consultas habituales a ChatGPT. En lugar de obtener respuestas de texto, vamos a pedirle a nuestro confiable asistente de inteligencia artificial que cree imágenes personalizadas para nosotros y las muestre en nuestra ventana de preguntas y respuestas. ¡Vamos a sumergirnos en esta aventura juntos!

## Un Breve Repaso

Antes de profundizar en esta nueva funcionalidad, recordemos lo que hemos estado haciendo en las últimas dos semanas. Comenzamos con consultas básicas a ChatGPT, y si te lo perdiste, puedes consultar la primera parte [aquí](/blog/chatgpt-basic). Luego, llevamos las cosas al siguiente nivel al enviar datos más extensos con prompts específicos y utilizar información de nuestra base de datos para obtener respuestas aún más precisas. Este emocionante desarrollo se detalla en la segunda parte [aquí](/blog/chatgpt-avanzado).

Dicho esto, es hora de abordar el desafío de hoy: cómo obtener imágenes generadas por ChatGPT y mostrarlas en nuestra interfaz de usuario de Business Central.

<br>

Después de esta breve introducción, ¡vamos manos a la obra! 🥳

<br>

## Creación del ControlAddIn para Visualizar Imágenes

Comencemos con el ControlAddIn. Este componente desempeña un papel fundamental en nuestra aplicación, permitiéndonos mostrar de forma elegante y eficiente las imágenes generadas por ChatGPT. Echemos un vistazo a su configuración detallada:

```javascript
controladdin "Imagen"
{
    MinimumWidth = 100;
    RequestedHeight = 100;
    HorizontalShrink = true;
    HorizontalStretch = true;

    Scripts = 'src/controlAddIn/Imagenes/js/Script.js',
                'https://code.jquery.com/jquery-3.6.0.min.js';
    StartupScript = 'src/controlAddIn/Imagenes/js/Startup.js';

    procedure CargarImagenBase64(base64Image: Text);
}
```

<br><br>

En este fragmento de código, encontrarás una serie de configuraciones clave. En primer lugar, establecemos las dimensiones mínimas y máximas del ControlAddIn para asegurar que se adapte perfectamente a la interfaz de usuario de Business Central. Luego, vinculamos los scripts JavaScript necesarios, incluyendo uno personalizado (Script.js) y una biblioteca externa (jQuery) que amplían las capacidades del ControlAddIn.

La función CargarImagenBase64 es esencial. Esta función toma una imagen en formato Base64 generada por ChatGPT y se encarga de mostrarla dinámicamente en el área designada de la interfaz de usuario.

<br>

Ahora, profundicemos en el archivo JavaScript que respalda esta funcionalidad:

```javascript
function CargarImagenBase64(base64Image) {
    HtmlImg = '<img id="imgInterior"></img>';
    var controlAddIn = $("#controlAddIn");
    controlAddIn.html(HtmlImg);

    // Establece la imagen en base64 como fuente
    var img = $("#imgInterior");

    // Establece la imagen en base64 como fuente
    img.attr("src", "data:image/png;base64," + base64Image);
    img.attr("height", 100);
    img.attr("width", 100);
}
```

<br><br>

Este código se encarga de tomar una imagen en formato Base64 generada por ChatGPT y mostrarla en el área configurada por el ControlAddIn. Crea un elemento HTML "img" con un identificador único llamado "imgInterior". Este elemento de imagen será la representación visual de la imagen que deseamos mostrar en la interfaz de usuario.

Luego, utilizamos jQuery para localizar el elemento ControlAddIn en nuestra página web. El ControlAddIn es el área designada en la interfaz de usuario de Business Central donde deseamos mostrar la imagen generada por ChatGPT.

En este punto, reemplazamos el contenido actual del ControlAddIn con el elemento de imagen que creamos previamente (HtmlImg). Esto significa que el área que antes podría haber contenido otros elementos ahora contendrá nuestra imagen.

Luego, volvemos a utilizar jQuery para encontrar el elemento de imagen dentro del ControlAddIn. Este elemento de imagen tiene el identificador único "imgInterior".

En el siguiente paso, establecemos la fuente (src) de la imagen. La fuente es una representación en formato Base64 de la imagen generada por ChatGPT. Concatenamos "data:image/png;base64," con el contenido de imagenBase64, que es la representación de la imagen en formato Base64. Esto es lo que realmente muestra la imagen en la interfaz de usuario.

Finalmente, ajustamos las dimensiones de la imagen. En este caso, establecemos la altura y el ancho en 100 píxeles cada uno. Esto garantiza que la imagen se muestre con un tamaño específico en la interfaz de usuario, en este caso, una imagen cuadrada de 100x100 píxeles.

<br>

## Pagina Pregunta a ChatGPT

La página de preguntas a ChatGPT es el epicentro de la acción. Aquí es donde interactuamos con nuestro asistente de inteligencia artificial y le pedimos que genere imágenes personalizadas. Vamos a revisar las modificaciones que realizamos en esta parte del código:

```javascript
usercontrol(AddImage; "Imagen")
{
    ApplicationArea = All;
    Visible = RequestImage;
}
```

<br><br>

En esta sección, configuramos un elemento de interfaz de usuario llamado "AgregarImagen", que es el ControlAddIn que mencionamos anteriormente. Lo hacemos visible o invisible según el valor de la variable RequestImage. Esta capacidad de controlar la visibilidad nos permite mostrar la imagen generada por ChatGPT solo cuando sea necesario.

La función ActiveImage también es importante. Cuando se llama a esta función, se activa la variable RequestImage, lo que indica que estamos listos para recibir una imagen de ChatGPT.

```javascript
procedure ActiveImage()
    begin
        RequestImage := true;
    end;
```

<br><br>

Además, en el código de la página, hemos ajustado la lógica para que los campos de solicitud y otros elementos solo sean visibles si RequestImage es verdadero. Esto garantiza una experiencia de usuario limpia y enfocada en la generación de imágenes.

Esto lo añadimos en el OpenPage de la pagina.

```javascript
if RequestImage then begin
    VisibleFieldsRequest := false;
    Prompt := '';
end;
```

<br><br>

Finalmente, en el botón de envío, hemos agregado una llamada a la función del ControlAddIn para cargar y mostrar la imagen generada por ChatGPT:

```javascript
 if RequestImage then
    CurrPage.AddImage.CargarImagenBase64(Response);
```

<br><br>

Con esto tendríamos estas modificaciones ahora solo queda la llamada contra ChatGPT.

<br>

## Codeunit para Solicitar a ChatGPT

La parte más crucial de nuestro código reside en el Codeunit que maneja las solicitudes a ChatGPT. Aquí es donde configuramos los parámetros y gestionamos las respuestas, asegurándonos de recibir imágenes en lugar de texto. Veamos cómo lo logramos:

Primero, en la función SetParams, hemos agregado un nuevo parámetro NewRequestImage para habilitar la solicitud de imágenes.

Lo primero que vamos a hacer aquí es añadir un nuevo parámetro en la función "SetParams".

```javascript
procedure SetParams(NewPrompt: Text; NewRequest: Text; NewRequestImage: Boolean)
    begin
        Prompt := NewPrompt;
        Request := NewRequest;
        RequestImage := NewRequestImage;
    end;
```

<br><br>

A continuación, en la función SetBody, hemos introducido lógica condicional para construir una solicitud diferente cuando estamos solicitando imágenes. Esto incluye la especificación de tamaño y formato de respuesta:

```javascript
if RequestImage then begin
    bodyJson.Add('prompt', Request);
    bodyJson.Add('n', 1);
    bodyJson.Add('size', '1024x1024');
    bodyJson.Add('response_format', 'b64_json');
end
```

<br><br>

También hemos ajustado la URL a la que se envía la solicitud según si solicitamos imágenes o datos, esto esta en la función "Post".

```javascript
URL := URLTxt;
if RequestImage then
    URL := URLImageTxt;
```

<br><br>

Por último, en la función "ReadResponse", hemos implementado una lógica especial para manejar las respuestas de ChatGPT cuando se trata de imágenes. Esto incluye la extracción de la imagen en formato Base64 de la respuesta:

```javascript
 if RequestImage then begin
    JsonObjResponse.Get('data', JsonTokResponse);
    JsonArr := JsonTokResponse.AsArray();
    JsonArr.Get(0, JsonTokChoices);
    JsonObjChoices := JsonTokChoices.AsObject();
    JsonObjChoices.Get('b64_json', JsonTokTextValue);
    ReturnValue := JsonTokTextValue.AsValue().AsText();
end
```

<br>

## Poniendo Todo en Acción

Finalmente, hemos creado un botón llamado "Crear Imágenes" que llama a todas estas funciones y procesos para que todo funcione en armonía. Este botón es el disparador que inicia la pagina de consultas de ChatGPT.

```javascript
action("Create Images")
{
    ToolTip = 'Create Images', Comment = 'ESP="Crear imágenes"';
    Caption = 'Create Images', Comment = 'ESP="Crear imágenes"';
    ApplicationArea = All;
    Image = CreateBinContent;

    trigger OnAction()
    var
        AskChatGPT: Page "Ask ChatGPT";
    begin
        AskChatGPT.ActiveImage();
        AskChatGPT.Run()
    end;
}
```

<br><br>

El resultado final es simplemente espectacular.

<input type="checkbox" id="image-checkbox-02" class="image-checkbox">
<label for="image-checkbox-02"  class="image-label">
    <img class="img-container" src="/assets/img/posts/chatgpt-imagen/imagen02.png">
</label>

<br><br>

En resumen, este código enriquecido y ampliado nos permite interactuar de manera efectiva con ChatGPT en Business Central, solicitando la generación de imágenes personalizadas y mostrándolas de manera dinámica en la interfaz de usuario. ¡Espero que esta descripción detallada te ayude a comprender mejor cómo funciona esta emocionante funcionalidad!

<br>

Como siempre, podrás encontrar el ejemplo completo en el [GitHub](https://github.com/Esanpons/chatgpt-in-business-central/tree/Imaagen)

<br>

Si tienes alguna pregunta o inquietud, no dudes en compartirla. ¡Hasta la próxima, y sigamos avanzando e innovando juntos!
