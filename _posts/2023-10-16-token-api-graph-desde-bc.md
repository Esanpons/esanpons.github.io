---
layout: post
title: "Cómo obtener el token de acceso para Microsoft Graph des de Business Central"
summary: "En este blog, aprenderemos a obtener el token de acceso necesario para interactuar con la API Graph de Microsoft. Se explicará el proceso paso a paso, desde la creación de una codeunit hasta la llamada a la función de la codeunit OAuth2. Además, se proporcionará un ejemplo completo. "
author: "Esteve Sanpons"
date: "2023-10-15 04:00:00 +0200"
#cSpell:disable
category: ["API_Graph", "Business_Central"]
thumbnail: /assets/img/posts/token-api-graph-desde-bc/imagen01.png
permalink: /blog/token-api-graph-desde-bc/
custom_type: Blog
#cSpell:enable
---

Buenas una semana más, en las ultimas semanas hemos visto como configurar el API Graph en Postman [ver aquí](/blog/api-graph-en-postman/), configurar Azure y registrar una aplicación [ver aquí](/blog/registrar-app-y-dar-permisos-en-azure/) y en Postman leer un correo [ver aquí](/blog/configurar-y-leer-en-postman-un-correo/).

<br>

Hoy por fin nos adentraremos un poco más en el tema de la programación que necesitamos para pedir el token a la API Graph.
El token como explique es algo necesario para el inicio de las transacciones con API Graph.

<br>

No nos entretendremos mucho, así que ¡Vamos manos a la obra! 🧐

<br>

Lo primero de todo es crear una codeunit en nuestro proyecto.

```javascript

    codeunit 62000 "Mgt Api Graph"
    {
        trigger OnRun()

        begin

        end;

    }
```

<br><br>

Ahora crearemos la función que nos devolverá el token, y añadiremos las variables que necesitaremos.

Empezaremos por el final, los dos labels representan la URL para acceder al token y el scope que se requiere para los headers.

Como mencioné, la lista de scopes solo tendrá uno, pero la codeunit de OAuth2 nos lo pedirá.

Por último, se encuentra la codeunit estándar que nos ayudará a obtener ese preciado token.

```javascript
    local procedure GetToken() AccessToken: text
    var
        OAuth2: Codeunit OAuth2;
        Scopes: List of [Text];
        URL: Text;
        ScopeLbl: Label 'https://graph.microsoft.com/.default', Locked = true;
        UrlTokenLbl: Label 'https://login.microsoftonline.com/%1/oauth2/v2.0/token', Locked = true;
    begin
        Clear(Scopes);
        clear(OAuth2);

        Scopes.Add(ScopeLbl);

        URL := StrSubstNo(UrlTokenLbl, Tenant);
        OAuth2.AcquireTokenWithClientCredentials(ClientID, ClientSecret, URL, '', Scopes, AccessToken);

    end;
```

<br>

Primero añadiremos el scope.

Después crearemos la URL.

Y por último, haremos la llamada a la función de la codeunit OAuth2.

<br><br>

Si todo va bien, nos devolverá en la variable AccessToken el token de acceso para Microsoft Graph.

Para comprobar que todo funciona correctamente, añadiremos la llamada en el OnRun y ejecutaremos la codeunit.

```javascript

    trigger OnRun()
    var
        AccessToken:Text;
    begin
        //Añadir el dato extraído de Azure
        ClientID := '';
        ClientSecret := '';
        Tenant := '';
        UserID := '';

        AccessToken := GetToken();
        Message('el token es: ' + AccessToken);

    end;
```

<br>

Recuerda completar los datos del ClientID, ClientSecret y Tenant.

<br>

Hemos aprendido cómo obtener el token de acceso para Microsoft Graph en programación con Business Central. Creamos una codeunit y una función para solicitar el token, utilizando la codeunit OAuth2 y configurando los scopes necesarios. Después de obtener el token, realizamos una prueba ejecutando la codeunit.

<br><br>

Como siempre, podréis ver el ejemplo entero en el [GitHub](https://github.com/Esanpons/ejemplos-blog/tree/main/AL/ApiGraph)

<br>

¡Espero que os resulte útil y emocionante! No dudéis en enviar vuestros comentarios y preguntas. ¡Hasta la próxima!
