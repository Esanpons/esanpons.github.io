---
layout: post
title: "Explorando las Maravillas de las Conexiones API en Microsoft Business Centra"
summary: "Exploraremos a fondo las conexiones a objetos de tipo API. Desde las peticiones GET para obtener datos precisos hasta la elegancia de los métodos POST y PUT para crear y modificar registros, este artículo te sumergirá en un universo de posibilidades."
author: "Esteve Sanpons"
date: "2024-01-07 04:00:00 +0200"
#cSpell:disable
category: ["Business_Central", "API", "Postman"]
thumbnail: /assets/img/posts/tipos-peticion-api/imagen01.png
permalink: /blog/tipos-peticion-api/
custom_type: Blog
#cSpell:enable
---

¡Hola lectores de NaviWorld! 👋

Es un placer estar nuevamente aquí, compartiendo conocimientos sobre la fascinante programación en Business Central de Microsoft. En esta entrega, vamos a adentrarnos en las posibilidades y funcionalidades de las conexiones a objetos de tipo API.

<br>

¡Así que, sin más preámbulos, empecemos manos a la obra! 😎

<br>

## Descubriendo las Maravillas de las Peticiones WebService

Antes de empezar para poder conectarte a Business Central se tiene que configurar en Azure una aplicación, esto lo explico en el siguiente [articulo](/blog/registrar-app-y-dar-permisos-en-azure/)

Es importante que lo revises antes para poder continuar.

También vamos a utilizar por ahora el Postman para hacer las peticiones hacia la API. Así que si no sabes como usarlo, es esto otro [articulo](/blog/api-graph-en-postman/) lo explique.

<br>

### GET - ¡Obtén Datos a Tu Manera!

Cuando hablamos de extraer datos, el método GET es nuestro aliado. La lógica de extracción, filtros y demás maravillas se ejecuta a través de la URL. ¿Quieres verlo en acción? Echa un vistazo al siguiente ejemplo:

```plaintext
https://api.businesscentral.dynamics.com/v2.0/3eec59d0-b663-4b9a-86f0-02c976cd5276/Production/mycompany/sales/v2.0/companies(3104717a-5377-ee11-817e-6045bdacaca5)/mycustomers('10000')
```

Aquí, estamos obteniendo información sobre un cliente específico. Podemos aplicar filtros para refinar nuestros resultados, como buscar clientes por nombre o cualquier otro campo mostrado en la API.

Y hablando de filtros, aquí tienes algunos trucos comunes:

-   `eq` para un filtro exacto.
-   `or` para agregar múltiples parámetros de búsqueda.
-   `ne` para buscar valores diferentes.
-   `Contains` para filtrar lo que contiene algo en particular.
-   `gt` y `lt` para valores más grandes o más pequeños, respectivamente.

Más ejemplos y detalles sobre filtros los encontrarás [aquí](https://docs.microsoft.com/en-us/dynamics365/business-central/dev-itpro/webservices/use-filter-expressions-in-odata-uris).

<br>

### POST - ¡Crea Nuevos Registros con Estilo!

Cuando se trata de añadir nuevos registros, el método POST es tu mejor amigo. Utilizamos la misma URL base, pero esta vez, agregamos un JSON con los datos necesarios para la creación del nuevo registro. ¡Simple y poderoso!

<input type="checkbox" id="image-checkbox-02" class="image-checkbox">
<label for="image-checkbox-02"  class="image-label">
    <img class="img-container" src="/assets/img/posts/tipos-peticion-api/imagen02.png">
</label>

<br>

### PUT - ¡Modifica Datos con Precisión!

¿Necesitas realizar modificaciones en un registro específico? El método PUT es la respuesta. Al igual que con el método POST, se utiliza la misma URL base, pero esta vez, se deben agregar ciertos encabezados, como "If-Match" y "Content-Type". ¡Y no olvides el JSON para la modificación!

<input type="checkbox" id="image-checkbox-03" class="image-checkbox">
<label for="image-checkbox-03"  class="image-label">
    <img class="img-container" src="/assets/img/posts/tipos-peticion-api/imagen03.png">
</label>

<br>

### DELETE - ¡Elimina con Seguridad!

Similar a las peticiones GET, el método DELETE se utiliza para eliminar un registro en particular. ¿Cómo? ¡Añadiendo la información en la URL!

<br>

### Ejecutar Funciones de la Page - ¡Acciones Especiales!

Imagina poder ejecutar funciones específicas desde las pages de tipo API. ¡Es posible! Solo necesitas agregar el cliente y el nombre de la función al final de la URL. En el código, podrías tener algo como esto:

```javascript
procedure editName(var ActionContext: WebServiceActionContext)
begin
    Rec.Name := 'NuevoNombre';
    Rec.Modify();
    ActionContext.SetObjectType(ObjectType::Page);
    ActionContext.SetObjectId(Page::"My Custom Customer API");
    ActionContext.AddEntityKey(Rec.FieldNo("No."), Rec."No.");
    ActionContext.SetResultCode(WebServiceActionResultCode::Updated);
end;
```

Esta función específica, en este caso, cambia el nombre de un cliente. ¡Y el nombre de la función es lo que aparece al final de la URL!

<br>

## Conclusión - ¡Más Allá de la Creación de Objetos API!

En resumen, en nuestro primer [articulo](/blog/crear-nueva-api/) aprendimos a crear objetos de tipo API, y ahora nos sumergimos en cómo conectarnos a ellos, explorando las diversas y poderosas opciones que ofrece la API de Business Central.

<br>

No olvides revisar el ejemplo completo en [GitHub](https://github.com/Esanpons/Connection-From-BC)

<br>

¡Hasta la próxima, programadores de NaviWorld! 🚀
