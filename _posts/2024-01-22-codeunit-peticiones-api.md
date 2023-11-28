---
layout: post
title: "Conectando con Tesoros API: Explorando el Mundo de Business Central"
summary: "Descubre cómo conectarte con otros Business Central, desentrañando el código de conexión base y explorando los cuatro tipos esenciales de conexiones WebService. Desde la configuración de variables hasta la autorización y la función de conexión, este artículo te guía a través de una herramienta poderosa para conquistar nuevos horizontes en la programación de Business Central."
author: "Esteve Sanpons"
date: "2024-01-21 04:00:00 +0200"
#cSpell:disable
category: ["Business_Central", "API"]
thumbnail: /assets/img/posts/codeunit-peticiones-api/imagen01.png
permalink: /blog/codeunit-peticiones-api/
custom_type: Blog
#cSpell:enable
---

¡Hola a todos, programadores de "NaviWorld"!

En nuestra emocionante serie sobre objetos de tipo API en Business Central, hemos abordado diversos aspectos técnicos para potenciar nuestras habilidades de programación. En esta entrega, daremos un giro fascinante al conectarnos con otro Business Central que alberga tesoros de objetos API.

Esto lo realizaremos con una codeunit donde ubicaremos el código de conexión base para cualquier petición.

Esto nos puede servir si disponemos de varias Bases de Datos o para externalizar nuestras conexiones dentro de nuestra propia Base de Datos.

<br>

veamos el código, ¡Vamos manos a la obra! 🤩

<br>

## Peticiones WebService desde Business Central

Hay 4 tipos de conexión contra un WebService en Business Central. GET, PUT, POST y DELETE. esto lo vimos en el anterior [articulo](/blog/tipos-peticion-api/). Esto 4 tipos son los necesarios para visualizar, modificar, crear o eliminar cualquier registro en Business Central.

Primero empezaremos por la estructura base, por las variables necesarios para crear todo el proceso de conexión, este proceso tal como lo vamos a hacer puede servir tanto para conectarse a una Base de Datos de Business central como a otros endpoints externos y poder manipular la información de esos endpoint des de Business Central.

<br>

### Variables

Estas son las 5 variables que se requieren para la conexión externa des de la Base de Datos.

**HttpClient:**
`HttpClient` es una variable que proporciona una interfaz para enviar y recibir solicitudes HTTP en un programa. En el contexto de la comunicación web, esta clase es fundamental. Permite realizar operaciones HTTP como GET, POST, PUT y DELETE, siendo la herramienta esencial para interactuar con servicios web y recuperar o enviar datos.

**HttpHeaders:**
Los `HttpHeaders` son elementos clave en el proceso de comunicación HTTP. Estos se utilizan para enviar información adicional sobre la solicitud o para indicar al servidor cómo se debe formatear la respuesta. Algunos encabezados comunes incluyen:

-   **Authorization:** Se utiliza para enviar credenciales de autenticación al servidor.
-   **Content-Type:** Especifica el tipo de medio del cuerpo de la solicitud, indicando cómo debería interpretarse el contenido.
-   **User-Agent:** Contiene la cadena del agente de usuario, es decir, el nombre del navegador o la aplicación cliente que realiza la solicitud.
-   **Accept-Charset:** Indica qué conjuntos de caracteres son aceptables en la respuesta.

**HttpContent:**
`HttpContent` es un componente crucial que representa tanto el cuerpo como los encabezados de una solicitud HTTP. Este objeto se utiliza para enviar información en el cuerpo de la solicitud. Puedes adjuntar diferentes tipos de contenido, como texto plano, JSON o datos binarios, según las necesidades de la operación.

**HttpResponseMessage:**
`HttpResponseMessage` es esencial para manejar la respuesta de una solicitud HTTP. Representa el resultado de una acción GET, POST, PUT o DELETE. A través de este objeto, puedes acceder al código de estado, los encabezados de respuesta y el contenido devuelto por el servidor.

**HttpRequestMessage:**
En algunos casos, la creación explícita de un objeto `HttpRequestMessage` puede no ser necesaria en AL. En su lugar, puedes interactuar directamente con métodos y propiedades del objeto `HttpClient` para establecer la URL, los encabezados y el cuerpo de la solicitud. Esto simplifica la construcción de solicitudes al permitirte trabajar directamente con el cliente HTTP para definir los detalles de la comunicación.

<br>

### Autorización

He creado dos tipos de autorización la básica, que a dia de hoy para Business Central ya no funciona, pero que puede servir para otros entornos y la de token que se añadirá el token tal cual en la autorización.

```javascript
    procedure CreateAuthorization(UserName: Text; Password: Text) ReturnValue: Text
    var
        Base64Convert: Codeunit "Base64 Convert";
    begin
        Authorization := 'Basic ' + Base64Convert.toBase64(StrSubstNo('%1:%2', UserName, Password));
        ReturnValue := Authorization;
    end;

    procedure CreateAuthorization(Token: Text) ReturnValue: Text
    begin
        Authorization := 'Bearer ' + Token;
        ReturnValue := Authorization;
    end;
```

<br>

### Función de conexión

El código proporcionado es una función llamada `CallWebService` escrita en AL, el lenguaje de programación utilizado en Microsoft Dynamics 365 Business Central. Esta función se encarga de realizar solicitudes a servicios web utilizando diferentes métodos como GET, PUT, PATCH, POST y DELETE. También permite incluir datos en formato JSON en las solicitudes y maneja las respuestas del servicio web.

<br>

#### Inicialización de Variables

```al
var
    HttpClient: HttpClient;
    HttpHeaders: HttpHeaders;
    HttpContent: HttpContent;
    ContentHttpHeaders: HttpHeaders;
    HttpResponseMessage: HttpResponseMessage;
    HttpRequestMessage: HttpRequestMessage;
    ResultLbl: Label 'Result: ', Comment = 'ESP="Resultado: "';
```

En esta sección, se definen varias variables que serán utilizadas a lo largo de la función. Estas incluyen instancias de clases proporcionadas por el framework AL para manejar operaciones HTTP, como `HttpClient`, `HttpHeaders`, `HttpContent`, `HttpResponseMessage`, y `HttpRequestMessage`. También se define una etiqueta (`ResultLbl`) para formatear mensajes de resultado.

<br>

#### Configuración de Encabezados

```al
HttpHeaders := HttpClient.DefaultRequestHeaders();

if Authorization <> '' then
    HttpHeaders.Add('Authorization', Authorization);
```

Aquí, se configuran los encabezados de la solicitud HTTP. Si se proporciona un token de autorización (`Authorization`), se agrega al encabezado.

<br>

#### Manejo de Solicitudes HTTP

```al
case RequestType of
    'Get':
        HttpClient.Get(URL, HttpResponseMessage);
    'Put':
        // ... (Ver explicación detallada más adelante)
    'Patch':
        // ... (Ver explicación detallada más adelante)
    'Post':
        // ... (Ver explicación detallada más adelante)
    'Delete':
        // ... (Ver explicación detallada más adelante)
    'Function':
        // ... (Ver explicación detallada más adelante)
end;
```

El código utiliza una estructura de casos (`case`) para manejar diferentes tipos de solicitudes HTTP, como GET, PUT, PATCH, POST, DELETE y una solicitud personalizada llamada 'Function'. Cada bloque de código dentro de los casos demuestra cómo se realiza una solicitud HTTP específica.

<br>

#### Detalles de Solicitudes Específicas (PUT, PATCH, POST, DELETE, Function)

##### PUT

```al
'Put':
    begin
        HttpHeaders.Add('If-Match', '*');

        HttpContent.WriteFrom(jsonText);

        HttpContent.GetHeaders(ContentHttpHeaders);
        ContentHttpHeaders.Clear();
        ContentHttpHeaders.Add('Content-Type', 'application/json');

        HttpClient.Put(URL, HttpContent, HttpResponseMessage);
    end;
```

En el caso de una solicitud PUT, se añade un encabezado específico ('If-Match: \*'). Luego, se escriben los datos JSON en el contenido de la solicitud (`HttpContent`). Finalmente, se configuran los encabezados de contenido y se realiza la llamada PUT utilizando `HttpClient`.

<br>

##### PATCH

```al
'Patch':
    begin
        Clear(HttpClient);
        Clear(HttpHeaders);
        Clear(HttpContent);
        Clear(ContentHttpHeaders);
        Clear(HttpResponseMessage);
        Clear(HttpRequestMessage);

        HttpContent.WriteFrom(jsonText);
        HttpContent.GetHeaders(ContentHttpHeaders);
        ContentHttpHeaders.Clear();
        ContentHttpHeaders.Add('Content-Type', 'application/json');

        HttpRequestMessage.GetHeaders(HttpHeaders);
        if Authorization <> '' then
            HttpHeaders.Add('Authorization', Authorization);

        HttpRequestMessage.Content(HttpContent);
        HttpRequestMessage.SetRequestUri(URL);
        HttpRequestMessage.Method('PATCH');

        HttpClient.Send(HttpRequestMessage, HttpResponseMessage);
    end;
```

Para una solicitud PATCH, se realiza una limpieza de variables y se configuran los encabezados y el contenido de la solicitud. Luego, se utiliza `HttpRequestMessage` para configurar la solicitud como PATCH antes de realizar la llamada con `HttpClient`.

<br>

##### POST

```al
'Post':
    begin
        HttpContent.WriteFrom(jsonText);

        HttpContent.GetHeaders(ContentHttpHeaders);
        ContentHttpHeaders.Clear();
        ContentHttpHeaders.Add('Content-Type', 'application/json');

        HttpClient.Post(URL, HttpContent, HttpResponseMessage);
    end;
```

En el caso de una solicitud POST, se escribe el contenido JSON, se configuran los encabezados y se realiza la llamada POST utilizando `HttpClient`.

<br>

##### DELETE

```al
'Delete':
    begin
        HttpClient.Delete(URL, HttpResponseMessage);
        if HttpResponseMessage.IsSuccessStatusCode then
            exit(ResultLbl + format(HttpResponseMessage.IsSuccessStatusCode));
    end;
```

Para las solicitudes DELETE, se realizan llamadas DELETE. Además, se maneja el resultado de estas operaciones, en este caso, se verifica si la operación fue exitosa antes de salir de la función.

<br>

##### DELETE

```al
'Function':
    begin
        HttpClient.Post(URL, HttpContent, HttpResponseMessage);
        if HttpResponseMessage.IsSuccessStatusCode then
            exit(ResultLbl + format(HttpResponseMessage.IsSuccessStatusCode));
    end;
```

Las funciones como hemos explicado en anteriores artículos lo que realizara sera ejecutar una función que hay en una pagina de tipo API.

<br>

#### Lectura de la Respuesta

```al
HttpResponseMessage.Content().ReadAs(ReturnValue);
```

Finalmente, se lee el contenido de la respuesta HTTP (`HttpResponseMessage`) y se almacena en la variable `ReturnValue`.

<br>

Este código proporciona una estructura clara para realizar solicitudes a servicios web desde Business Central. La modularidad de la función `CallWebService` facilita la adaptación a diferentes casos de uso y tipos de solicitudes HTTP.

<br>

## Leyendo la Respuesta: Descifrando el Tesoro

Finalmente, leemos el contenido de la respuesta HTTP y lo almacenamos en la variable ReturnValue.

Este código proporciona una estructura clara y modular para realizar solicitudes a servicios web desde Business Central. La versatilidad y capacidad para adaptarse a diferentes casos de uso y tipos de solicitudes HTTP. Con esta herramienta en tu cinturón de utilidades, ¡prepárate para conquistar nuevos horizontes en la programación de Business Central!

<br>

Como de costumbre, el ejemplo completo está disponible en [GitHub](https://github.com/Esanpons/Connection-From-BC).

<br>

¡Y así concluye nuestro emocionante viaje de hoy! Espero que este artículo haya iluminado tu camino en la programación de Business Central. Mantente al tanto para más aventuras en el fascinante mundo de la programación.
