---
layout: post
title: "Summernote y ControlAddIn: Elevando la Experiencia en tus Pedidos de Venta en Business Central"
summary: "¡Embárcate en una odisea tecnológica con Summernote y ControlAddIn en Business Central! Descubre cómo estos poderosos recursos transforman por completo la manera en que gestionas tus pedidos de venta."
author: "Esteve Sanpons"
date: "2023-12-24 04:00:00 +0200"
#cSpell:disable
category: ["JavaScript", "ControlAddin", "Business_Central", "Summernote"]
thumbnail: /assets/img/posts/summernote-multiples-descripciones/imagen01.png
permalink: /blog/summernote-multiples-descripciones/
custom_type: Blog
#cSpell:enable
---

¡Hola apasionados de la programación! Aquí estoy para llevarros a otro nivel en el universo de Business Central. 🚀 En las misiones anteriores, destripamos cómo forjar un controladdin, guardar y cargar datos, e incluso mencionar. Si te has perdido alguna de estas hazañas, ¡echa un vistazo [aquí](/categorias/Summernote/)! 👀

Pero hoy, estoy emocionado de presentaros una nueva joya para la lista de pedidos de venta. Imaginar esto: al final de tu lista de pedidos, un lugar donde puedes ver y modificar los textos enriquecidos de cada pedido, ¡todo sin abandonar la lista! 🌟

Visualicemos la meta:

```html
<input type="checkbox" id="image-checkbox-02" class="image-checkbox" />
<label for="image-checkbox-02" class="image-label">
    <img class="img-container" src="/assets/img/posts/summernote-multiples-descripciones/imagen02.png" />
</label>
<br />
```

La idea es simple pero poderosa: tener un espacio donde puedan personalizar cada pedido, editándolo y guardándolo sobre la marcha.

<br>

¿Listos para sumergirse en este nuevo desafío? Pues, ¡Vamos manos a la obra! 😤

<br>

## Archivo controladdin

Bienvenidos al epicentro de nuestro código. Aquí añadimos algunos eventos y funciones que serán la columna vertebral de nuestro espectáculo. Pero espera, ¡aquí viene lo jugoso!

Hemos de añadir en este mismo objeto la llamada al JavaScript "ScriptsComments" y CSS "Style" que a continuacion explicaremos

```javascript
  Scripts =
        'https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.6.0.js', //https://docs.microsoft.com/en-us/aspnet/ajax/cdn/overview
        'https://cdn.jsdelivr.net/npm/summernote@0.8.20/dist/summernote.min.js', //https://cdn.jsdelivr.net/npm/summernote@0.8.20/dist/
        'https://cdn.jsdelivr.net/npm/summernote@0.8.20/dist/lang/summernote-es-ES.js', //https://cdn.jsdelivr.net/npm/summernote@0.8.20/dist/
        'https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.5/umd/popper.min.js',   //este revisar en la ultima version cual estan utilizando antes de actualizar
        'https://maxcdn.bootstrapcdn.com/bootstrap/4.1.3/js/bootstrap.min.js', //este revisar en la ultima version cual estan utilizando antes de actualizar
        'EnrichedText-Summernote/src/controlAddIn/summernote/js/ScriptsDescription.js',
        'EnrichedText-Summernote/src/controlAddIn/summernote/js/ScriptsComments.js';

    StyleSheets =
         'https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css', //https://www.bootstrapcdn.com/
         'https://cdn.jsdelivr.net/npm/summernote@0.8.20/dist/summernote.min.css', //https://cdn.jsdelivr.net/npm/summernote@0.8.20/dist/
          'EnrichedText-Summernote/src/controlAddIn/summernote/css/Style.css';


    //funciones y eventos para la parte de Comments
    procedure InitHtmlComments(JsonMention: JsonArray);
    procedure AddNewSummerNoteComments(Data: Text; EntryNo: Integer; OrderNo: Code[20]);
    procedure FinishHtmlComments();
    event OnChangeComments(Data: Text; EntryNo: Integer; OrderNo: Code[20]);
    event MentionComments(UserMention: JsonObject; EntryNo: Integer);


```

<br><br>

## Archivo ScriptsComments.js

Bienvenidos a la pista de baile del JavaScript "ScriptsComments.js", donde creamos variables y funciones para dar vida al HTML.

Este código juega un papel crucial en la creación de una experiencia de usuario excepcional para gestionar comentarios enriquecidos. En la antesala, declaramos nuestras variables: `controlAddIn` para el control enriquecido, `TextHtml` como nuestro lienzo dinámico, y `JsonMention` para las menciones que añaden ese toque especial.

**Iniciando el Espectáculo con InitHtmlComments:**
La función `InitHtmlComments` se ilumina en el escenario al recibir un conjunto fresco de menciones (`NewJsonMention`). Aquí, establecemos el escenario HTML y estamos listos para la acción.

**Añadiendo Magia con AddNewSummerNoteComments:**
¡Ah, la función `AddNewSummerNoteComments`! Aquí, cada comentario es una estrella en su propio derecho, con un número de pedido, texto enriquecido y botones para editar, guardar y cancelar. Una danza dinámica de HTML que cobra vida.

**Conclusión Épica con FinishHtmlComments:**
La función `FinishHtmlComments` nos lleva al grand finale, cerrando la cortina del HTML y dejando todo listo para brillar en el controladdin.

**¡Interactuando con los Comentarios! - edit, save, cancel:**
Ahora, tres héroes emergen: `edit`, `save`, y `cancel`. La función `edit` lanza la edición, revelando un Summernote mágico para dar forma al texto. `save` guarda los cambios, ejecuta su magia y `cancel` pone fin a la edición. Los botones desaparecen, como una función de teatro bien coreografiada.

**La Función Summernote - Una Joya Adicional:**
En el backstage, la función Summernote da vida a la edición, permitiendo cambios de formato, inserción de enlaces y hasta menciones con un toque especial.

Este código no es solo texto y funciones, es una narrativa para un blog. Es el guión que crea una experiencia de usuario cautivadora, como si nuestros comentarios fueran estrellas brillando en un cielo digital. ¡El escenario está listo, y el público (¡nuestros usuarios!) aplaude emocionado!

```javascript
var controlAddIn;
var TextHtml;
var JsonMention;

function InitHtmlComments(NewJsonMention) {
    JsonMention = NewJsonMention;
    controlAddIn = document.getElementById("controlAddIn");
    controlAddIn.innerHTML = "";
    TextHtml = "";
    TextHtml += '<div id="global">';
}
function AddNewSummerNoteComments(Data, EntryNo, OrderNo) {
    TextHtml += "<br>";
    TextHtml += '<hr style="border:15px;">';
    TextHtml += '<DIV align="right">Nº Pedido: ' + OrderNo + "</DIV>";
    TextHtml += "   ";
    TextHtml += '<div class="summernoteText' + EntryNo + '">' + Data + "</div>";
    TextHtml += "<br>";
    TextHtml +=
        '<button id="edit' +
        EntryNo +
        '"class="btn-primary" onclick="edit(\'' +
        EntryNo +
        '\')" type="button">Edit</button>';
    TextHtml += "   ";
    TextHtml +=
        '<button id="save' +
        EntryNo +
        '"class="btn-primary" onclick="save(\'' +
        EntryNo +
        "','" +
        OrderNo +
        '\')" type="button" style="visibility:hidden;">Save</button>';
    TextHtml += "   ";
    TextHtml +=
        '<button id="cancel' +
        EntryNo +
        '"class="btn-primary" onclick="cancel(\'' +
        EntryNo +
        '\')" type="button" style="visibility:hidden;">Cancel</button>';
}
function FinishHtmlComments() {
    TextHtml += "</div>";
    controlAddIn.innerHTML = TextHtml;
}
var edit = function (EntryNo) {
    document.getElementById("save" + EntryNo).style.visibility = "visible";
    document.getElementById("cancel" + EntryNo).style.visibility = "visible";

    $(".summernoteText" + EntryNo).summernote({
        height: 100,
        focus: true,
        lang: "es-ES",
        //Esto permitirá tabular a través de los campos en Formularios.
        tabDisable: true,

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
                Microsoft.Dynamics.NAV.InvokeExtensibilityMethod("MentionComments", [item, EntryNo]);
                return $("<a>")
                    .attr("href", item.code)
                    .text("@" + item.name)
                    .get(0);
            },
        },

        //esto es la configuracion de la bara cuando no esta el air activado
        toolbar: [
            ["style", ["style"]],
            ["font", ["bold", "italic", "underline", "strikethrough", "superscript", "subscript", "clear"]],
            ["fontname", ["fontname"]],
            ["fontsize", ["fontsize"]],
            ["color", ["color"]],
            ["Misc", ["undo", "redo"]],
            ["para", ["ul", "ol", "paragraph", "height"]],
            ["insert", ["picture", "link", "video", "table", "hr"]],
            ["view", ["codeview", "help"]],
        ],
    });
    $(".note-editable");
};

var save = function (EntryNo, OrderNo) {
    var txt = $(".summernoteText" + EntryNo).summernote("code");
    $(".summernoteText" + EntryNo).summernote("destroy");
    Microsoft.Dynamics.NAV.InvokeExtensibilityMethod("OnChangeComments", [txt, EntryNo, OrderNo]);

    document.getElementById("save" + EntryNo).style.visibility = "hidden";
    document.getElementById("cancel" + EntryNo).style.visibility = "hidden";
};

var cancel = function (EntryNo) {
    $(".summernoteText" + EntryNo).summernote("destroy");
    document.getElementById("save" + EntryNo).style.visibility = "hidden";
    document.getElementById("cancel" + EntryNo).style.visibility = "hidden";
};
```

<br><br>

## Archivo Style.css

Y ahora, en el escenario del CSS, añadimos estilo y color para que todo luzca como un festival de colores en el código.

Nuestro archivo CSS, una obra maestra en sí mismo, despliega la alfombra roja para una experiencia visual deslumbrante.

**#global - La Fortaleza Visual:**
Comencemos con `#global`. Este maestro de la estética define una fortaleza visual con una altura de 300px, un borde sutil de 1px sólido en un tono de #ddd, y un toque mágico de desplazamiento vertical (scroll) que añade dinamismo a nuestra ventana de comentarios.

**#mensajes - Elevación Automática:**
Ahora, el encantador `#mensajes`. Aquí, la altura automática ofrece una flexibilidad visual, permitiendo que nuestros mensajes alcancen alturas elevadas sin restricciones, adaptándose a la diversidad de contenidos que puedan emerger.

**.texto - Elegancia en Cada Píxel:**
En la clase `.texto`, la elegancia se despliega con cada píxel. El espacio acogedor de padding de 4px y el fondo blanco como la nieve (#fff) añaden un toque de sofisticación y claridad a cada mensaje. Es como darle un suéter cómodo a nuestros textos.

**.btn-primary - Botones de Distinción:**
Finalmente, los botones entran en escena con la clase `.btn-primary`. Estos no son botones comunes; son heraldos de distinción. Con su fondo blanco, texto negro y un borde sólido de 2px en un tono #008CBA, cada botón es una llamada a la acción que no pasa desapercibida.

Este CSS no es solo un conjunto de reglas; es la paleta que pinta la experiencia visual de nuestro blog. Es el vestuario que viste a nuestros elementos HTML con estilo y elegancia, creando un espectáculo visual que nuestros usuarios recordarán. ¡Es tiempo de desatar la creatividad en el mundo del diseño web!

```css
#global {
    height: 300px;
    border: 1px solid #ddd;
    overflow-y: scroll;
}

#mensajes {
    height: auto;
}

.texto {
    padding: 4px;
    background: #fff;
}

.btn-primary {
    background-color: white;
    color: black;
    border: 2px solid #008cba;
}
```

<br><br>

## Página lista de pedidos de venta

Ahora, la parte emocionante: la extensión de la lista de pedidos. Añadimos el controladdin, creamos una función épica para llenarlo todo y, por supuesto, eventos que hacen que todo esto cobre vida.

Esta extensión de página (pageextension) es la coreografía detrás de una lista de órdenes de venta que deslumbra con su funcionalidad mejorada.

**Diseño Consciente en el Layout:**
El baile comienza en el layout, extendiendo la lista de órdenes de venta. Un nuevo actor, `SummernoteComments`, entra en escena, llevando consigo una experiencia mejorada de comentarios. Este controladdin es la estrella, proporcionando un lienzo para textos enriquecidos.

**Ritmo de Eventos - OnAfterGetRecord:**
Después de cada actuación, el evento `OnAfterGetRecord` se enciende. Aquí, entra en juego `CreateDescriptions()`, preparando el escenario para la función principal.

**Grupo de ControlAddins - El Corazón del Espectáculo:**
En el bloque `group(ControlAddins2)`, bajo la mirada de 'Descriptions', `SummernoteComments` cobra vida. Sus eventos, desde el inicial hasta `ControlAddInReady`, marcan el compás de la experiencia del usuario.

**Funciones de Cambio y Mención - OnChangeComments y MentionComments:**
En el centro del escenario, las funciones `OnChangeComments` y `MentionComments` desatan su magia. La primera, al recibir cambios, los transmite a la tabla de ventas, mientras que la segunda responde a menciones, creando un efecto especial con mensajes.

**Creación Dinámica de Contenidos - CreateDescriptions:**
El clímax llega con `CreateDescriptions()`. Este maestro de ceremonias recorre la tabla de ventas, recopilando datos y construyendo un espectáculo de comentarios enriquecidos. La función `CreateJson()` agrega una lista de menciones, preparando a los protagonistas para su gran entrada.

Esta de página no es solo código; es la partitura que dirige una experiencia de usuario cautivadora. Cada línea de código es un paso de danza, creando un espectáculo visual y funcional que eleva la lista de órdenes de venta a nuevas alturas. ¡Prepárense para aplaudir a esta estrella en ascenso en el universo de Business Central!

```javascript
pageextension 50001 "List Sales Order" extends "Sales Order List"
{
    layout
    {
        addafter(Control1)
        {
            group(ControlAddins2)
            {
                Caption = 'Descriptions';
                usercontrol(SummernoteComments; "SummerNote")
                {
                    ApplicationArea = All;
                    trigger ControlAddInReady()
                    begin
                        CreateDescriptions();
                    end;

                    trigger OnChangeComments(Data: Text; EntryNo: Integer; OrderNo: Code[20])
                    var
                        SalesHeader: Record "Sales Header";
                    begin
                        SalesHeader.get(SalesHeader."Document Type"::Order, OrderNo);
                        SalesHeader.SetWorkDescription(Data);
                    end;

                    trigger MentionComments(UserMention: JsonObject; EntryNo: Integer)
                    var
                        JToken: JsonToken;
                        JValue: JsonValue;
                    begin
                        UserMention.Get('code', JToken);
                        JValue := JToken.AsValue();
                        Message(JValue.AsText());
                    end;
                }
            }
        }
    }

    trigger OnAfterGetRecord()
    begin
        CreateDescriptions();
    end;

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

    local procedure CreateDescriptions()
    var
        SalesHeader: Record "Sales Header";
        i: Integer;
    begin
        i := 0;

        CurrPage.SummernoteComments.InitHtmlComments(CreateJson());

        SalesHeader.Reset();
        SalesHeader.CopyFilters(Rec);
        SalesHeader.FindSet();
        repeat
            NewData := SalesHeader.GetWorkDescription();
            if NewData <> '' then begin
                i += 1;
                CurrPage.SummernoteComments.AddNewSummerNoteComments(NewData, i, SalesHeader."No.");
            end;

        until SalesHeader.Next() = 0;

        CurrPage.SummernoteComments.FinishHtmlComments();
    end;

    var
        NewData: Text;
}


```

<br><br>

<br>

Como siempre, puedes encontrar el ejemplo completo en [GitHub](https://github.com/Esanpons/ControlAddIns-Business-Central)

<br>

En resumen, hemos explorado un emocionante viaje en el universo de Business Central, aprovechando al máximo Summernote y ControlAddIn. Desde la creación dinámica de comentarios enriquecidos hasta el despliegue estético con CSS, hemos descubierto cómo potenciar los pedidos de venta con una experiencia de usuario única. ¡Prepárate para elevar tus aplicaciones con esta combinación de herramientas!
