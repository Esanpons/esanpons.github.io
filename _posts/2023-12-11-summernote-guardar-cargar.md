---
layout: post
title: "Dominando el Arte del Guardado y la Carga con Summernote en ControlAddIn de Business Central"
summary: "Descubre cómo establecer una conexión sólida entre Summernote y la tabla de cabeceras de venta en Business Central. Este artículo te guiará paso a paso para garantizar que cada edición que realices en este editor de texto enriquecido se guarde y cargue de manera eficiente."
author: "Esteve Sanpons"
date: "2023-12-10 04:00:00 +0200"
#cSpell:disable
category: ["JavaScript", "ControlAddin", "Business_Central", Summernote]
thumbnail: /assets/img/posts/summernote-guardar-cargar/imagen01.png
permalink: /blog/summernote-guardar-cargar/
custom_type: Blog
#cSpell:enable
---

¡Hola a todos! Aquí volvemos con las últimas novedades sobre los controladdin y Summernote. Esta vez, estamos abordando un módulo del que ya hablamos anteriormente podemos verlo en el siguiente [enlace](/categorias/Summernote/).

Así que, para poneros al tanto, teníamos un problemilla con un texto enriquecido en la ficha del pedido. Resulta que cuando lo editábamos, ¡sorpresa!, no se guardaba y desaparecía al salir.

Pero no temáis, en este post vamos a zambullirnos en la programación para solucionarlo. Vamos a implementar la función que guardará y cargará el texto en la tabla de las cabeceras de venta.

Para llevar esto a cabo, echaremos mano del campo 200 llamado "Work Description". Este campo es un blob, y la tabla ya tiene funciones propias para guardar y recuperar datos de este blob.

<br>

¡Sin más preámbulos!, ¡Vamos manos a la obra! 🤩

<br>

## Archivo controladdin

Primero, echamos un vistazo al objeto controladdin para incorporar los eventos y funciones nuevas. El primero servirá para modificar el texto desde la página, y el segundo, para hacer lo propio desde el controladdin hacia la tabla y guardarlo en el blob.

```javascript
procedure SetDataDescription(Data: Text);
event OnChangeDescription(Data: Text);
```

<br><br>

## Archivo ScriptsDescription.js

En nuestro archivo de scripts, programamos la función "SetDataDescription". Es fácil y sencillo, solo agregamos el texto a nuestro summernote.

```javascript
function SetDataDescription(Data) {
    $("#summernote").summernote("code", Data);
}
```

<br><br>

Aquí también añadimos la llamada al evento cuando cambiamos el texto. Esto se hace como una opción dentro del propio summernote. Le devolvemos el texto o los datos que hemos añadido de la siguiente manera.

```javascript
callbacks: {
    onChange: function (contents, $editable) {
        Microsoft.Dynamics.NAV.InvokeExtensibilityMethod("OnChangeDescription", [contents]);
    }
},
```

<br><br>

## Página de pedidos de venta

Finalmente, nos dirigimos a nuestra página de pedidos de venta y añadimos el evento y la función a nuestro código. Modificamos el evento ControlAddInReady para insertar el texto que hayamos guardado en la tabla. El segundo evento es para añadir y guardar cualquier cambio que realicemos en nuestro campo de texto.

```javascript
trigger ControlAddInReady()
begin
    NuevoDato := RecuperarDatoDescripcionTrabajo();
    PagActual.SummernoteDescripcion.AñadirNuevoSummernoteDescripcion(NuevoDato, CrearJson());
end;

trigger OnChangeDescription(Datos: Texto)
begin
    NuevoDato := Datos;
    RecuperarDatoDescripcionTrabajo(NuevoDato);
end;
```

<br>

Además, creamos un nuevo trigger de "OnAfterGetRecord" para que cuando cambiemos de pedidos, los datos se actualicen automáticamente. Para lograr esto, utilizamos la función "SetDataDescription".

```javascript
trigger OnAfterGetRecord()
begin
    NuevoDato := '';
    NuevoDato := RecuperarDatoDescripcionTrabajo();

    PagActual.SummernoteDescripcion.SetDataDescription(NuevoDato);
end;

var
    NuevoDato: Texto;
```

<br><br>

¡Y listo! Si seguís estos pasos, veréis cómo los datos que creéis se almacenan y cargan en vuestro controladdin de texto enriquecido.

<br>

Como siempre, puedes encontrar el ejemplo completo en [GitHub](https://github.com/Esanpons/ControlAddIns-Business-Central)

<br>

En este emocionante recorrido a través del código, hemos abordado y superado el desafío de un texto enriquecido reacio a quedarse guardado. Utilizando el campo 200 "Work Description" y sus funciones integradas, programamos eventos y funciones clave en el objeto controladdin y en la página de pedidos de venta. Ahora, vuestros datos se almacenan y cargan con éxito en el controladdin de texto enriquecido. ¡Una solución simple para un problema común! ¡Hasta la próxima aventura de programación! 🚀
