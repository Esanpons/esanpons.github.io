---
layout: post
title: "Dominando las Conexiones API en Business Central: Un Viaje Práctico"
summary: "En la última entrega de nuestra serie sobre programación en API de Business Central, realizamos pruebas de la integración API para la gestión eficiente de clientes."
author: "Esteve Sanpons"
date: "2021-01-28 04:00:00 +0200"
#cSpell:disable
category: ["Business_Central", "API"]
thumbnail: /assets/img/posts/pruebas-api/imagen01.png
permalink: /blog/pruebas-api/
custom_type: Blog
#cSpell:enable
---

¡Hola lectores de NaviWorld! 👋 Una semana más, estamos de vuelta con la última entrega de nuestra serie sobre programación en Business Central. Si has seguido cada entrada, felicidades, ¡te has convertido en un maestro de la integración API en BC!

### Resumen Rápido

Para ponernos al día, en las últimas semanas, hemos cubierto (la creación de URL)[/blog/crear-nueva-api], (tipos de peticiones)[/blog/tipos-peticion-api], (funcionalidades para las peticiones entre Business Central)[/blog/codeunit-url-api] y cómo (trabajar con variables HTTP en Business Central)[/blog/codeunit-peticiones-api].

Hoy, para cerrar este emocionante recorrido, vamos a poner a prueba toda esta funcionalidad y la integraremos en la página de clientes con acciones.

<br>

¡Menos explicaciones, más acción! ¡Manos a la obra! 💪

<br>

### Estructura Base de la Página extendida de Clientes

Comencemos estableciendo la base de la páginaextension y las variables necesarias:

```javascript
pageextension 51100 "Customer" extends "Customer List"
{
    actions
    {
        addfirst(navigation)
        {
        }
    }

    var
        MyCreateUrl: Codeunit "MyCreateUrl";
        MyConection: Codeunit "MyConection";
        URL: Text;
        NewCustNo: Text;
        Result: Text;
        jsontext: Text;
}
```

En este código, he definido las herramientas esenciales para nuestro viaje: `MyCreateUrl` y `MyConection` para la creación de URL y la conexión a la API, respectivamente.

<br>

### Creando un Nuevo Registro (POST)

Vamos a adentrarnos en el emocionante proceso de crear un nuevo cliente mediante una solicitud POST. Para comenzar, he desarrollado una función que genera el JSON necesario, que actuará como el cuerpo de nuestra solicitud. Este ejemplo ilustra cómo sería la creación de un nuevo cliente. Echemos un vistazo al código:

```javascript
local procedure ReturnTestJsonPageAPI(NewName: Text) JsonText: Text
var
    JObject: JsonObject;
begin
    JObject.Add('name', 'CustomerName: ' + NewName);
    JObject.WriteTo(JsonText);
end;
```

¡Estupendo! Ahora, pasemos a una función esencial para nuestras operaciones: procesar el JSON devuelto y extraer el número de cliente. Aquí está cómo funciona:

```javascript
local procedure ReturnCustomerNo(JsonText: Text) CustomerNo: Text
var
    Jtoken: JsonToken;
    JObject: JsonObject;
begin
    Jtoken.ReadFrom(JsonText);
    JObject := Jtoken.AsObject();
    JObject.Get('custNo', Jtoken);
    CustomerNo := Jtoken.AsValue().AsText();
end;
```

Ahora, sumérgete en la acción de creación de un nuevo cliente. He incorporado un botón llamado `PostPageAPI`, diseñado para enviar una solicitud POST a nuestra API. Esta acción resultará en la creación de un cliente ficticio, y además, te mostrará el número de cliente generado por Business Central.

```javascript
action(PostPageAPI)
{
    ApplicationArea = All;
    Promoted = true;
    PromotedCategory = Process;
    Caption = 'Post Page API', Locked = true;
    ToolTip = 'Post Page API', Locked = true;
    Image = SendApprovalRequest;

    trigger OnAction()
    begin
        Clear(MyConection);
        Clear(MyCreateUrl);

        MyCreateUrl.InitBaseURLPageAPI('mycustomers');
        URL := MyCreateUrl.GetURL();

        MyConection.CreateAuthorization('admin', '123456');
        jsontext := MyConection.CallWebService(URL, 'Post', ReturnTestJsonPageAPI('CREATE'));
        NewCustNo := ReturnCustomerNo(JsonText);
        Message(JsonText);
    end;
}
```

<br>

### Visualizando el Cliente (GET)

Ahora, pasemos a visualizar el cliente que acabamos de crear. He implementado una acción `GetPageAPI` que realiza una solicitud GET a la API, recuperando y mostrando la información del cliente recién creado.

```javascript
action(GetPageAPI)
{
    ApplicationArea = All;
    Promoted = true;
    PromotedCategory = Process;
    Caption = 'GET Page API', Locked = true;
    ToolTip = 'GET Page API', Locked = true;
    Image = SendApprovalRequest;

    trigger OnAction()
    begin
        Clear(MyConection);
        Clear(MyCreateUrl);

        MyCreateUrl.InitBaseURLPageAPI('mycustomers');
        MyCreateUrl.SetUrlFilterKeyPageAPI(NewCustNo);
        URL := MyCreateUrl.GetURL();

        MyConection.CreateAuthorization('admin', '123456');
        Result := MyConection.CallWebService(URL, 'Get', '');
        Message(Result);
    end;
}
```

<br>

### Modificando el Cliente (PUT)

Vamos al siguiente nivel y actualicemos la información del cliente. Con la acción `PutPageAPI`, hacemos una solicitud PUT con un JSON modificado, demostrando cómo actualizar datos de cliente.

```javascript
action(PutPageAPI)
{
    ApplicationArea = All;
    Promoted = true;
    PromotedCategory = Process;
    Caption = 'Put Page API', Locked = true;
    ToolTip = 'Put Page API', Locked = true;
    Image = SendApprovalRequest;

    trigger OnAction()
    begin
        Clear(MyConection);
        Clear(MyCreateUrl);

        MyCreateUrl.InitBaseURLPageAPI('mycustomers');
        MyCreateUrl.SetUrlFilterKeyPageAPI(NewCustNo);
        URL := MyCreateUrl.GetURL();

        MyConection.CreateAuthorization('admin', '123456');
        Result := MyConection.CallWebService(URL, 'Put', ReturnTestJsonPageAPI('MOD'));
        Message(Result);
    end;
}
```

<br>

### Eliminando lo Creado (DELETE)

En este paso, eliminamos el cliente creado anteriormente. La acción `DeletePageAPI` realiza una solicitud DELETE, eliminando el registro de cliente correspondiente.

```javascript
action(DeletePageAPI)
{
    ApplicationArea = All;
    Promoted = true;
    PromotedCategory = Process;
    Caption = 'Delete Page API', Locked = true;
    ToolTip = 'Delete Page API', Locked = true;
    Image = SendApprovalRequest;

    trigger OnAction()
    begin
        Clear(MyConection);
        Clear(MyCreateUrl);

        MyCreateUrl.InitBaseURLPageAPI('mycustomers');
        MyCreateUrl.SetUrlFilterKeyPageAPI(NewCustNo);
        URL := MyCreateUrl.GetURL();

        MyConection.CreateAuthorization('admin', '123456');
        Result := MyConection.CallWebService(URL, 'Delete', '');
        Message(Result);
    end;
}
```

<br>

### Ejecución de Funciones desde la Página API

Finalmente, he añadido una acción llamada `Functions` que demuestra cómo ejecutar funciones específicas directamente desde la página API.

```javascript
action(Functions)
{
    ApplicationArea = All;
    Promoted = true;
    PromotedCategory = Process;
    Caption = 'Functions Page API', Locked = true;
    ToolTip = 'Functions Page API', Locked = true;
    Image = SendApprovalRequest;

    trigger OnAction()
    begin
        Clear(MyConection);
        Clear(MyCreateUrl);

        MyCreateUrl.InitBaseURLPageAPI('mycustomers');
        MyCreateUrl.SetUrlFilterKeyPageAPI(NewCustNo);
        MyCreateUrl.SetUrlFuntionPageAPI('editName');
        URL := MyCreateUrl.GetURL();

        MyConection.CreateAuthorization('admin', '123456');
        Result := MyConection.CallWebService(URL, 'Function', '');
        Message(Result);
    end;
}
```

<br>

Con estas adiciones, nuestra página de clientes se convierte en una herramienta completa para gestionar clientes a través de la API. Ahora, puedes crear, visualizar, modificar y eliminar clientes de manera eficiente. Esto son solo ejemplo de como podemos hacer peticiones en nuestros Business central.

<br>

Como es habitual, el ejemplo completo se encuentra disponible en [GitHub](https://github.com/Esanpons/Connection-From-BC).

<br>

Recuerda, aquí está la lógica de programación. Así que, amigos, sigamos construyendo soluciones increíbles en Business Central. ¡Nos vemos en el próximo artículo de NaviWorld! 🚀
