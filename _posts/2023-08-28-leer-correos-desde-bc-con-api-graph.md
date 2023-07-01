---
layout: post
title: "Conectándose a la API Graph de Microsoft desde Business Central para obtener correos electrónicos no leídos"
summary: "Aprende cómo utilizar la API Graph de Microsoft en Business Central para obtener mensajes de correo electrónico no leídos de una bandeja de entrada específica. ¡Descubre las posibilidades que ofrece la API Graph de Microsoft!"
author: "Esteve Sanpons"
date: "2023-07-01 04:00:00 +0200"
#cSpell:disable
category: ["API_Graph", "Business_Central"]
thumbnail: /assets/img/posts/leer-correos-desde-bc-con-api-graph/imagen01.png
permalink: /blog/leer-correos-desde-bc-con-api-graph/
custom_type: Blog
#cSpell:enable
published: false
---

¡Buenas una semana más!

Esta semana es la última en este largo recorrido que nos ha llevado a conocer qué es la API Graph de Microsoft. Hemos aprendido cómo configurar la API Graph en Postman [ver aquí](/blog/api-graph-en-postman/), también a registrar una aplicación y configurar Azure [ver aquí](/blog/registrar-app-y-dar-permisos-en-azure/) Además, hemos leído un correo de nuestro 365 desde Postman [ver aquí](/blog/configurar-y-leer-en-postman-un-correo/) y la semana anterior aprendimos a pedirle el Token a la API Graph desde Business Central [ver aquí](/blog/token-api-graph-desde-bc/)

<br>

Hoy os tocará conectarnos al correo y descargar los correos sin leer que hay en nuestra bandeja de entrada.

Esto es similar a lo que hicimos con Postman, pero más divertido porque lo vamos a desarrollar en Business Central.

<br>

Sin más preámbulos, ¡vamos manos a la obra! :bowtie:

<br>

Utilizaremos la Codeunit que creamos la semana anterior.

Lo primero que vamos a hacer es crear una función y sus variables.

Esta función nos servirá para conectarnos a la API Graph.

```javascript

    procedure Connection(URL: Text; TypeConection: Integer; BodyData: Text) ReturnValue: Text
    var
        HttpCliente: HttpClient;
        HttpResponseMessages: HttpResponseMessage;
        Authorization: Text;
        Success: Boolean;
        BearerLbl: Label 'Bearer %1', Locked = true;
        Txt01Err: Label 'Request was blocked by environment', Comment = 'ESP="La solicitud fue bloqueada por el entorno"';
        Txt02Err: Label 'Request to Business Central failed\%1 %2', Comment = 'ESP="Solicitud a Business Central fallida\%1 %2"';
        Txt03Err: Label 'Request to Business Central failed\%1', Comment = 'ESP="La solicitud a Business Central falló\%1"';
    begin
        if AccessToken = '' then
            AccessToken := GetToken();

        Authorization := StrSubstNo(BearerLbl, AccessToken);

        HttpCliente.DefaultRequestHeaders().Add('Authorization', Authorization);
        HttpCliente.DefaultRequestHeaders.Add('Accept', 'application/json');
        Success := HttpCliente.Get(Url, HttpResponseMessages);

        if not Success then
            if HttpResponseMessages.IsBlockedByEnvironment then
                Error(Txt01Err)
            else
                Error(Txt03Err, GetLastErrorText());

        if not HttpResponseMessages.IsSuccessStatusCode then
            Error(Txt02Err, HttpResponseMessages.HttpStatusCode, HttpResponseMessages.ReasonPhrase);

        HttpResponseMessages.Content.ReadAs(ReturnValue);
    end;
```

<br>

Este código muestra cómo establecer una conexión HTTP y realizar una solicitud utilizando el objeto HttpClient.

El objetivo de este procedimiento, llamado "Connection", es realizar una solicitud a una URL específica y manejar posibles errores que puedan ocurrir durante la conexión y la solicitud HTTP. A continuación, explicaremos cada parte del código:

1. Parámetros de entrada: El procedimiento toma tres parámetros de entrada: URL, TypeConnection y BodyData. Estos parámetros se utilizan para especificar la dirección a la que se realizará la solicitud, el tipo de conexión y los datos que se enviarán en el cuerpo de la solicitud, respectivamente.

2. Variables locales: Se declaran varias variables locales, incluyendo "HttpCliente" (objeto HttpClient utilizado para realizar solicitudes HTTP), "HttpResponseMessages" (objeto HttpResponseMessage para almacenar la respuesta HTTP), "Authorization" (cadena de texto que contendrá el encabezado de autorización), "Success" (indicador booleano para verificar si la solicitud fue exitosa) y algunas etiquetas (utilizadas para mostrar mensajes de error en diferentes idiomas).

3. Obtención del token de acceso: Si la variable "AccessToken", está vacía, se llama a la función "GetToken()" para obtener un token válido. Esto asegura que tengamos un token válido antes de realizar la solicitud HTTP.

4. Construcción del encabezado de autorización: El encabezado de autorización se construye agregando el token de acceso al texto de la etiqueta "BearerLbl" utilizando la función "StrSubstNo()". Esto crea una cadena de texto con el formato "Bearer {AccessToken}" que se utilizará como valor del encabezado "Authorization" en la solicitud HTTP.

5. Configuración de encabezados adicionales: Se configuran los encabezados adicionales en el objeto HttpCliente. Se agrega un encabezado "Accept" con el valor "application/json" para indicar que se espera una respuesta en formato JSON.

6. Realización de la solicitud HTTP: Se realiza la solicitud HTTP utilizando el método "Get" del objeto HttpCliente. Se pasan la URL y el objeto HttpResponseMessages como parámetros. El resultado de la solicitud se almacena en la variable "Success" para verificar si fue exitosa o no.

7. Manejo de errores: Se realizan comprobaciones condicionales para manejar posibles errores durante la solicitud. Si la solicitud fue bloqueada por el entorno, se muestra un mensaje de error específico. De lo contrario, se muestra un mensaje de error general junto con el texto del último error obtenido.

8. Verificación del código de estado: Se verifica si el código de estado de la respuesta HTTP indica éxito utilizando el método "IsSuccessStatusCode" del objeto HttpResponseMessages. Si el código de estado no indica éxito, se muestra un mensaje de error que incluye el código de estado y la frase de motivo de la respuesta HTTP.

9. Lectura del contenido de la respuesta: Finalmente, el contenido de la respuesta HTTP se lee y se almacena en la variable de retorno "ReturnValue" utilizando el método "ReadAs" del objeto HttpResponseMessages.

Este fragmento de código ilustra cómo realizar una conexión HTTP y manejar los posibles errores que pueden ocurrir durante la solicitud. Utilizando el objeto HttpClient de Business Central, podemos interactuar con la API Graph de Microsoft y obtener datos relevantes.

<br><br><br>

Ahora para ejecutar todo esto lo añadimos a trigger "OnRun".

```javascript
    trigger OnRun()
    var
        URL: Text;
        JsonRespuesta: Text;
        UrlMailsUserLbl: Label 'https://graph.microsoft.com/v1.0/users/%1/mailFolders/inbox/messages?$count=true', Locked = true;
        FiltersURLLbl: Label '&$filter=isread%20eq%20false', Locked = true;
    begin
        URL := StrSubstNo(UrlMailsUserLbl, UserID);
        URL += FiltersURLLbl;

        JsonRespuesta := Connection(URL, 1, '');
        Message('El Json de respuesta es: ' + JsonRespuesta);
    end;

```

<br>

Vamos a examinar cada parte del código:

1.  Variables locales: Se declaran varias variables locales, incluyendo "URL" (cadena de texto que contendrá la URL para llamar a la API de Graph), "JsonRespuesta" (cadena de texto que almacenará la respuesta JSON obtenida), y algunas etiquetas (utilizadas para construir la URL de la solicitud).

2.  Construcción de la URL de la solicitud: Se construye la URL para llamar a la API de Graph utilizando las etiquetas "UrlMailsUserLbl" y "FiltersURLLbl". La etiqueta "UrlMailsUserLbl" contiene la URL base de la API de Graph para obtener los mensajes de correo electrónico de un usuario. Se utiliza la función "StrSubstNo()" para reemplazar "%1" en la etiqueta con el valor de "UserID". Luego, se concatena la etiqueta "FiltersURLLbl" para filtrar solo los mensajes no leídos.

3.  Llamada a la función "Connection()": Se realiza una llamada a la función "Connection()" con la URL construida en el paso anterior. El valor de retorno de esta función se asigna a la variable "JsonRespuesta".

4.  Mostrar el resultado: Se muestra un mensaje en pantalla con el contenido de la respuesta JSON obtenida, concatenando el mensaje con la cadena "El Json de respuesta es: ".

<br>

El resultado es que ahora podemos recibir los correos en nuestro Business Central.

Como mencioné al principio, esto es solo un ejemplo, y podemos hacer muchas más cosas, como ver los archivos en OneNote o SharePoint, así como ver, crear y modificar los mensajes en Teams.

Imagínate las posibilidades que hay.

<br><br>

En resumen, hemos visto cómo utilizar la API de Graph de Microsoft para obtener mensajes de correo electrónico no leídos de una bandeja de entrada específica. Utilizando la potencia de la API de Graph de Microsoft, podemos integrar y acceder a diferentes servicios de Microsoft, como el correo electrónico, desde Business Central, lo que permite automatizar procesos y mejorar la eficiencia empresarial. Espero que esta guía te haya sido útil y te inspire a explorar más posibilidades con la API de Graph de Microsoft.

<br><br>

Como siempre, podréis ver el ejemplo entero en el [Link](https://github.com/Esanpons/ApiGraph)

<br>

Recuerda que este es solo un ejemplo y que puedes adaptarlo y expandirlo según tus necesidades específicas. ¡Buena suerte con tus proyectos, explorando nuevas oportunidades y no dudes en enviarme tus comentarios y preguntas! ¡Espero que os resulte útil, emocionante y hasta la próxima!
