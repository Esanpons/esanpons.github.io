---
layout: post
title: "Desentrañando las Páginas API en Business Central: Tu Pasaporte a la Programación Personalizada"
summary: "Descubre el fascinante universo de las Páginas API en Business Central de Microsoft. Desde la creación de tu propia página hasta la personalización de cada detalle, este artículo te guiará a través de las claves esenciales para aprovechar al máximo las capacidades de programación en Business Central."
author: "Esteve Sanpons"
date: "2023-12-31 04:00:00 +0200"
#cSpell:disable
category: ["Business_Central", "API"]
thumbnail: /assets/img/posts/crear-nueva-api/imagen01.png
permalink: /blog/crear-nueva-api/
custom_type: Blog
#cSpell:enable
---

¡Hola a todos los entusiastas de Business Central en "NaviWorld"!

Hoy vamos a sumergirnos en el fascinante mundo de las API de Business Central de Microsoft. Si estás emocionado por desentrañar los secretos detrás de la programación en esta plataforma, ¡has llegado al lugar adecuado!

He visto importante de explicar un poco como funcionan las API de Business Central y por ello, os voy a traer durante las siguientes semanas unos blogs donde explicare con algunos detalles todo lo necesario para este tema, que es de lo más interesante.

En en Learn de Microsfot para Business Central podéis encontrar la información que os voy a explicar [enlace](https://learn.microsoft.com/es-es/training/modules/work-with-api/)

<br>

Sin mas ¡Vamos manos a la obra! 🚀

<br>

**Creación de la Página tipo API**

Imagina tener el poder de crear tus propias páginas API en Business Central sin la necesidad de publicarlas como lo harías con los servicios web OData tradicionales. Es posible, y te voy a guiar a través de ello.

Comencemos con la creación de un objeto de tipo API. Puedes optar por una Página o una Consulta, y simplemente establecer la propiedad PageType o QueryType en API, según tu elección. Pero aquí viene lo emocionante: ¡no necesitas publicar estas páginas API como lo harías con otros servicios! 🎉

Para empezar, usaremos el fragmento `tpage` y luego estableceremos la propiedad `PageType` en `API`. Pero no nos detenemos ahí. Aquí tienes algunas propiedades adicionales que puedes configurar para personalizar tu API:

-   **APIVersion:** Define la versión de la API. Puedes crear varias versiones de la misma API para evolucionar sin afectar los servicios existentes. La versión puede tener valores como beta, v1.0, v1.1, v1.2, v2.0, etc.

-   **APIPublisher:** Es el nombre de la empresa que ha creado las API. Se utiliza en la URL para conectarse a la API y también para agrupar todas las API del mismo editor.

-   **APIGroup:** Un grupo utilizado para agrupar lógicamente varias API. Este valor también se usa en la URL para conectarse a la API.

-   **EntityName:** El valor singular de la entidad que la API devuelve (Cliente, Artículo, Automóvil, Proveedor, Artista, Película, etc.).

-   **EntitySetName:** Parámetro que representa el valor plural de la entidad que devuelve la API (Clientes, Artículos, Automóviles, Proveedores, Artistas, Películas, etc.).

-   **DelayedInsert:** Debe establecerse siempre en true en las páginas de la API. Garantiza que los valores solo se inserten en la base de datos cuando todos los valores se aprovisionan desde la solicitud de la API.

-   **ODataKeyFields:** Indica el campo que se usará como clave. Permite especificar el campo que se utilizará para buscar un registro determinado.

-   **SourceTable:** Donde se indica la tabla relacionada si es necesario.

-   **InsertAllowed, DeleteAllowed, ModifyAllowed:** Propiedades que indican si se permiten operaciones de inserción, eliminación o modificación de registros dentro de la página.

Comencemos con la creación de un objeto de tipo API. Puedes optar por una Página o una Consulta, y simplemente establecer la propiedad PageType o QueryType en API, según tu elección. Pero aquí viene lo emocionante: ¡no necesitas publicar estas páginas API como lo harías con otros servicios! 🎉

Aquí hay un ejemplo práctico:

```javascript
page 51100 "My Custom Customer API"
{
    PageType = API;
    APIVersion = 'v2.0';
    APIPublisher = 'mycompany';
    APIGroup = 'sales';
    EntityName = 'mycustomer';
    EntitySetName = 'mycustomers';
    DelayedInsert = true;
    ODataKeyFields = "No.";
    SourceTable = Customer;
    InsertAllowed = true;
    DeleteAllowed = true;
    ModifyAllowed = true;

    layout
    {
        area(Content)
        {
            repeater(GroupName)
            {
                field(custNo; Rec."No.")
                {
                    ApplicationArea = All;
                }
                field(name; Rec.Name)
                {
                    ApplicationArea = All;
                }
                field(email; Rec."E-Mail")
                {
                    ApplicationArea = All;
                }
            }
        }
    }
}

```

<br><br>

Este código te permite crear una página de API personalizada para gestionar clientes, con funciones de inserción, eliminación y modificación.

<br>

**Conexiones**

Ahora, hablemos sobre cómo estructurar la URL, un elemento clave cuando trabajas con las API de Business Central.

La base de la URL se compone de la conexión con el cliente y la base de datos, por ejemplo:

https://api.businesscentral.dynamics.com/v2.0

<br>

Luego, incorporamos el tenant y el nombre del entorno, información que puedes encontrar fácilmente en la URL de tu Business Central. Así se vería:

/3eec59d0-b663-4b9a-86f0-02c976cd5276/Production

El tenant y el enviorment los podemos encontrar fácilmente en la URL de nuestro Business Central

<input type="checkbox" id="image-checkbox-02" class="image-checkbox">
<label for="image-checkbox-02"  class="image-label">
    <img class="img-container" src="/assets/img/posts/crear-nueva-api/imagen02.png">
</label>

<br>

Después, añadimos las configuraciones que hemos establecido en nuestra página de API:

/mycompany/ventas/v2.0/

<br>

A continuación, especificamos la compañía de la cual queremos obtener datos:

/companies(3104717a-5377-ee11-817e-6045bdacaca5)/

Este dato lo podemos encontrar utilizando el inspector de la pagina en la lista de empresas de nuestro sistema:

<input type="checkbox" id="image-checkbox-03" class="image-checkbox">
<label for="image-checkbox-03"  class="image-label">
    <img class="img-container" src="/assets/img/posts/crear-nueva-api/imagen03.png">
</label>

<br>

Finalmente, completamos la URL con el nombre de la entidad, quedando así:

https://api.businesscentral.dynamics.com/v2.0/3eec59d0-b663-4b9a-86f0-02c976cd5276/Production/mycompany/sales/v2.0/companies(3104717a-5377-ee11-817e-6045bdacaca5)/mycustomers

<br>

**Profundizando en el Código**

Ahora, adentrémonos en el código proporcionado. Lo que hemos creado es una página de API personalizada para gestionar clientes. Observa cómo cada propiedad tiene un propósito específico. Por ejemplo, `APIVersion` nos permite crear varias versiones de la misma API para facilitar la evolución sin afectar a los servicios existentes.

La propiedad `DelayedInsert` garantiza que los valores se inserten en la base de datos solo cuando se aprovisionan desde la solicitud de la API, brindando mayor control sobre las transacciones.

Y así sucesivamente, cada propiedad tiene su importancia y funcionalidad, brindándote la flexibilidad necesaria para adaptar la API a tus necesidades específicas.

**Conclusiones**

En resumen, hoy hemos aprendido a crear páginas de tipo API en Business Central y a construir URLs para acceder a estas API de manera efectiva. Este conocimiento te coloca en el asiento del conductor, permitiéndote personalizar y optimizar tus experiencias de programación en Business Central.

<br>

Como siempre, puedes encontrar el ejemplo completo en [GitHub](https://github.com/Esanpons/Connection-From-BC)

<br>

¡Espero que encuentres esta inmersión en las API de Business Central tan emocionante como yo! En las próximas semanas, seguiré compartiendo más detalles y trucos para que puedas aprovechar al máximo esta potente plataforma. ¡Hasta la próxima, apasionados desarrolladores!
