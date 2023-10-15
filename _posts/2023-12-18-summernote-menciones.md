---
layout: post
title: "Potenciando Business Central: Menciones y Desplegables con SummerNote"
summary: "En este viaje de programación, exploramos cómo potenciar Business Central mediante la integración de "SummerNote". Desde habilitar menciones enriquecidas en tus textos hasta la creación de desplegables personalizados"
author: "Esteve Sanpons"
date: "2023-12-17 04:00:00 +0200"
#cSpell:disable
category: ["JavaScript", "ControlAddin", "Business_Central", Summernote]
thumbnail: /assets/img/posts/summernote-menciones/imagen01.png
permalink: /blog/summernote-menciones/
custom_type: Blog
#cSpell:enable
---

¡Saludos, equipo de desarrolladores! En las últimas publicaciones, hemos estado dándole caña al tema del texto enriquecido, y quiero hablaros sobre un módulo que está haciendo olas en nuestras implementaciones de Business Central: ¡el "SummerNote"!

Ya hemos desglosado la [creación del controladdin](/blog/summernote-inicio), y también nos zambullimos en cómo [guardar y cargar datos](/blog/summernote-guardar-cargar). Pero hoy, mis amigos, vamos a llevar las cosas al siguiente nivel.

**¡Menciones y Desplegables en Texto Enriquecido!**

Vamos a darle un giro interesante. ¿Alguna vez quisiste mencionar a alguien en tu texto para, por ejemplo, enviarle un correo o agregar una nota? Bueno, eso es exactamente lo que vamos a hacer.

<br>

Como siempre después de la breve explicación ¡Vamos manos a la obra! 😋

<br>

## Archivo controladdin

En este archivo, estamos dando un paso importante. Hemos introducido un nuevo evento llamado "MentionDescription". Este evento se encargará de manejar las menciones que ocurran en nuestro texto enriquecido. Además, hemos añadido un parámetro adicional a la función de creación "AddNewSummerNoteDescription". Esto nos permitirá incorporar las menciones y gestionarlas de manera más eficaz.

```javascript
event MentionDescription(UserMention: JsonObject);
procedure AddNewSummerNoteDescription(Data: Text; JsonMention: JsonArray);
```

<br>

Este paso es crucial para habilitar la funcionalidad de menciones en nuestro texto enriquecido. Ahora, el controladdin está listo para manejar y procesar estas menciones de manera adecuada.

<br><br>

## Archivo ScriptsDescription.js

Este archivo contiene la esencia del comportamiento del "SummerNote". Hemos ampliado el constructor del "SummerNote" para incluir la sección de "Insinuaciones". En términos simples, esto es lo que aparecerá cuando despleguemos nuestras opciones. Hemos agregado funciones que buscan, plantillas que formatean y contenido que destaca.

```javascript
function AddNewSummerNoteDescription(Data, JsonMention) {
    var controlAddIn = document.getElementById("controlAddIn");
    controlAddIn.innerHTML = '<textarea class="summernote" id="summernote"></textarea>';
    CreateSummerNoteDescription(Data, JsonMention);
}

function CreateSummerNoteDescription(Data, JsonMention) {
    //Initialize editor only once when DOM is loaded
    $(document).ready(function () {
        $(".summernote").summernote({
            height: $(window).height() - 100,
            focus: true,
            lang: "es-ES",
            //Esto permitirá tabular a través de los campos en Formularios.
            tabDisable: true,
            callbacks: {
                //Bind onChange callback with our OnChange BC event
                onChange: function (contents, $editable) {
                    Microsoft.Dynamics.NAV.InvokeExtensibilityMethod("OnChangeDescription", [contents]);
                },
            },

            hint: {
                mentions: JsonMention,
                match: /\B@(\w*)$/,
                search: function (keyword, callback) {
                    callback(
                        $.grep(this.mentions, function (item) {
                            return item.name.toLowerCase().indexOf(keyword.toLowerCase()) == 0;
                        })
                    );
                },
                template: function (item) {
                    return item.name;
                },
                content: function (item) {
                    Microsoft.Dynamics.NAV.InvokeExtensibilityMethod("MentionDescription", [item]);
                    return $("<a>")
                        .attr("href", item.code)
                        .text("@" + item.name)
                        .get(0);
                },
            },
        });
        $(".note-editable").html(Data);
    });
}
```

<br>

Este código garantiza que el SummerNote funcione como un encanto al proporcionar una experiencia de usuario fluida al seleccionar menciones en el texto enriquecido.

<br><br>

## Página de pedidos de venta

En la página de pedidos, la todo continúa. Al hacer el despliegue inicial, llamamos a una función que genera un conjunto de Json con nombres y códigos para cada persona. Este enfoque desde Business Central nos brinda un control total sobre las menciones y desplegables.

```javascript
trigger ControlAddInReady()
begin
    NewData := Rec.GetWorkDescription();
    CurrPage.SummernoteDescription.AddNewSummerNoteDescription(NewData, CreateJson());
end;
```

<br>

La función "CreateJson" es como el director de orquesta. Añade nombres y códigos al Json, preparándolos para su uso en el SummerNote. Este enfoque nos da la flexibilidad de gestionar estas menciones desde nuestras tablas o cualquier otro lugar necesario.

```javascript

    procedure CreateJson() ReturnArrayJson: JsonArray
    var
        JObject: JsonObject;
    begin
        Clear(JObject);
        JObject.Add('name', 'Esteve Sanpons');
        JObject.Add('code', 'esanpons');

        ReturnArrayJson.Add(JObject);

        Clear(JObject);
        JObject.Add('name', 'Roberto Rodriguez');
        JObject.Add('code', 'rrodriguez');

        ReturnArrayJson.Add(JObject);

        Clear(JObject);
        JObject.Add('name', 'Fernando Martin');
        JObject.Add('code', 'fmartin');

        ReturnArrayJson.Add(JObject);
    end;

```

<br>

Finalmente, en la página de pedidos, manejamos el evento después de obtener el registro. Extraemos el dato del Json y lo mostramos en pantalla. Pero aquí es donde podríamos realizar acciones más emocionantes según nuestras necesidades.

```javascript

    trigger OnAfterGetRecord()
    begin

        NewData := '';
        NewData := Rec.GetWorkDescription();

        CurrPage.SummernoteDescription.SetDataDescription(NewData);
        if CurrPage.Editable then
            CurrPage.SummernoteDescription.EnableDescription()
        else
            CurrPage.SummernoteDescription.DisableDescription();

    end;

```

<br>

Este código es la guinda del pastel. Nos permite interactuar con los datos obtenidos de las menciones, abriendo un abanico de posibilidades para personalizar nuestras acciones.

<br>

Como siempre, puedes encontrar el ejemplo completo en [GitHub](https://github.com/Esanpons/ControlAddIns-Business-Central)

<br>
¡Espero que esta expansión proporcione una visión más clara de cada componente! Si tenéis más preguntas, no dudéis en lanzarlas en los comentarios. ¡Sigamos programando con entusiasmo!
