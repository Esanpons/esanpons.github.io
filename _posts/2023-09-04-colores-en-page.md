---
layout: post
title: "Añade Color y Estilo a tus Páginas"
summary: "Descubre cómo destacar información relevante y mejorar la presentación de tus páginas y campos. Aprende a aplicar estilos y colores de manera sencilla y efectiva, utilizando AL y Enums."
author: "Esteve Sanpons"
date: "2023-09-03 07:00:00 +0200"
#cSpell:disable
category: ["Navision", "Business_Central"]
thumbnail: /assets/img/posts/colores-en-page/imagen01.png
permalink: /blog/colores-en-page/
custom_type: Blog
#cSpell:enable
publised:false
---

¡Hola a todos! Espero que estéis teniendo una excelente semana llena de aprendizaje y desarrollo.

<br><br>

En esta ocasión, voy a compartir una técnica para agregar color a las páginas o campos en Dynamics 365 Business Central (anteriormente conocido como Navision) de una manera sencilla y atractiva.

Seguramente, en más de una ocasión, os han solicitado de añadir color a una página o campo en particular, o incluso que el color cambie dependiendo de ciertas condiciones. Pues bien, en este blog, os mostraré cómo lograrlo de forma eficiente utilizando tanto C/AL como AL. Para esta demostración, utilizaré AL.

En Dynamics 365 Business Central, contamos con diferentes estilos que podemos aplicar a nuestros campos para resaltar información relevante. A continuación, podéis ver un ejemplo de los estilos disponibles:

<input type="checkbox" id="image-checkbox-02" class="image-checkbox">
<label for="image-checkbox-02"  class="image-label">
    <img class="img-container" src="/assets/img/posts/colores-en-page/imagen02.png">
</label>

<br>

Esto nos brinda un total de 11 opciones para personalizar el aspecto de nuestros campos y mejorar la experiencia del usuario.

<br>

Bueno, después de la explicación, vamos manos a la obra 🤗

<br>

Comencemos por la propiedad "StyleExpr", que nos permitirá aplicar los estilos según nuestras necesidades. Os mostraré un ejemplo práctico utilizando la tabla de productos como base para nuestra página, en la que agregaremos cuatro campos.

En este caso, queremos que cuando un producto esté bloqueado, todos los campos asociados se muestren en color rojo. Para lograrlo, crearemos una variable que almacenará el estilo que deseamos aplicar. Luego, asignaremos esta variable a la propiedad "StyleExpr" de los campos correspondientes.

```javascript
page 50100 "Color Item List"
{
    PageType = List;
    SourceTable = Item;
    ApplicationArea = All;
    UsageCategory = Lists;
    Caption = 'Item List With Color', Comment = 'ESP="Lista de productos con color"';

    layout
    {
        area(content)
        {
            repeater(Group)
            {
                field("No."; Rec."No.")
                {
                    ToolTip = 'Specifies the value of the field', comment = 'ESP="Especifica el valor del campo"';
                    ApplicationArea = All;
                    StyleExpr = StyleExpresion;
                }
                field(Description; Rec.Description)
                {
                    ToolTip = 'Specifies the value of the field', comment = 'ESP="Especifica el valor del campo"';
                    ApplicationArea = All;
                    StyleExpr = StyleExpresion;
                }
                field(Blocked; Rec.Blocked)
                {
                    ToolTip = 'Specifies the value of the field', comment = 'ESP="Especifica el valor del campo"';
                    ApplicationArea = All;
                    StyleExpr = StyleExpresion;
                }
                field(Inventory; Rec.Inventory)
                {
                    ToolTip = 'Specifies the value of the field', comment = 'ESP="Especifica el valor del campo"';
                    ApplicationArea = All;
                    StyleExpr = StyleExpresion;
                }
            }
        }
    }

    var
        StyleExpresion: Text;
}
```

<br><br>

Una vez definida la estructura de la página, nos dirigimos al trigger "OnAfterGetRecord", donde definiremos la lógica para verificar si el producto está bloqueado o no, y rellenaremos la variable "StyleExpresion" con el nombre del estilo correspondiente. En este caso, utilizaremos el estilo "Attention", que es el color rojo sin negrita.

```javascript
    trigger OnAfterGetRecord()
    begin
        StyleExpresion := '';

        if Rec.Blocked then
            StyleExpresion := 'Attention';
    end;

```

<br><br><br><br>

¡Listo! Ahora, cada vez que se recorra una línea en la página, se verificará si el producto está bloqueado o no, y se aplicará el estilo adecuado a los campos.

<br>

Para darle un toque adicional de limpieza y evitar "Hardcodes", he creado un enum llamado "ColourStyleExpr" con todos los colores disponibles. De esta forma, podremos acceder a los colores de manera más intuitiva.

```javascript
enum 50100 ColourStyleExpr
{
    Extensible = true;

    value(0; "Ambiguous")
    {
        Caption = 'Ambiguous', Locked = true;
    }
    value(1; "Attention")
    {
        Caption = 'Attention', Locked = true;
    }
    value(2; "AttentionAccent")
    {
        Caption = 'AttentionAccent', Locked = true;
    }
    value(3; "Favorable")
    {
        Caption = 'Favorable', Locked = true;
    }
    value(4; "None")
    {
        Caption = 'None', Locked = true;
    }
    value(5; "Standard")
    {
        Caption = 'Standard', Locked = true;
    }
    value(6; "StandardAccent")
    {
        Caption = 'StandardAccent', Locked = true;
    }
    value(7; "Strong")
    {
        Caption = 'Strong', Locked = true;
    }
    value(8; "StrongAccent")
    {
        Caption = 'StrongAccent', Locked = true;
    }
    value(9; "Subordinate")
    {
        Caption = 'Subordinate', Locked = true;
    }
    value(10; "Unfavorable")
    {
        Caption = 'Unfavorable', Locked = true;
    }
}

```

<br><br>

Con esto, ahora podemos acceder a los nombres de los colores directamente desde el enum en lugar de utilizar cadenas de texto en el código.

```javascript
    trigger OnAfterGetRecord()
    var
        ColourStyleExpr: Enum ColourStyleExpr;
    begin
        StyleExpresion := '';

        if Rec.Inventory < 10 then
            StyleExpresion := format(Enum::"ColourStyleExpr"::Ambiguous);

        if Rec.Blocked then
            StyleExpresion := Format(ColourStyleExpr::Attention);
    end;

```

<br><br>

Con estos cambios, el código se verá más limpio y organizado, y será más fácil realizar futuras modificaciones!

<br>

En este blog, hemos explorado una técnica sencilla y efectiva para mejorar la presentación de páginas y campos en Dynamics 365 Business Central. Utilizando AL, hemos aprendido cómo aplicar estilos a los campos para destacar información relevante y mejorar la experiencia del usuario.

<br>

Como siempre, podréis ver el ejemplo entero en el [Link](https://github.com/Esanpons/ejemplos-blog/tree/main/AL/ColorEnCamposDePage)

<br>

¡Y eso es todo! Espero que esta explicación y el código os hayan sido de ayuda. Si tenéis algún comentario o pregunta, no dudéis en hacerla. ¡Hasta la próxima!

<br>

¡Y ahí lo tienes! Espero que esta guía y el código os hayan sido útiles. Si tenéis algún comentario o consulta, no dudéis en compartirlo. ¡Hasta la próxima aventura en Dynamics 365 Business Central!
