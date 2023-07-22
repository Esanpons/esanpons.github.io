---
layout: post
title: "Añade Color y Estilo a tus Páginas"
summary: "Descubre cómo destacar información relevante y mejorar la presentación de tus páginas y campos. Aprende a aplicar estilos y colores de manera sencilla y efectiva, utilizando AL y Enums."
author: "Esteve Sanpons"
date: "2023-08-06 07:00:00 +0200"
#cSpell:disable
category: ["Navision", "Business_Central"]
thumbnail: /assets/img/posts/colores-en-page/imagen01.png
permalink: /blog/colores-en-page/
custom_type: Blog
#cSpell:enable
---

¡Hola a todos! Confío en que estéis disfrutando de una semana productiva y llena de nuevos conocimientos.

<br>

Hoy, quiero compartir con vosotros un método para darle un toque de color a las páginas o campos en Dynamics 365 Business Central (antes conocido como Navision) de una forma fácil y visualmente atractiva.

Es probable que en algún momento os hayan pedido añadir color a una página o campo específico, o incluso que el color cambie en función de ciertas condiciones. Pues bien, en este blog, os enseñaré cómo conseguirlo de manera eficaz utilizando tanto C/AL como AL. Para esta demostración, me centraré en AL.

En Dynamics 365 Business Central, disponemos de varios estilos que podemos aplicar a nuestros campos para destacar información importante. Aquí tenéis un ejemplo de los estilos que podéis usar:

<input type="checkbox" id="image-checkbox-02" class="image-checkbox">
<label for="image-checkbox-02"  class="image-label">
    <img class="img-container" src="/assets/img/posts/colores-en-page/imagen02.png">
</label>

<br>

Esto nos proporciona un total de 11 opciones para personalizar la apariencia de nuestros campos y mejorar la experiencia del usuario.

<br>

Bueno, tras esta introducción, vamos manos a la obra 🤗

<br>

Empezaremos con la propiedad "StyleExpr", que nos permitirá aplicar los estilos según nuestras necesidades. Os mostraré un ejemplo práctico utilizando la tabla de productos como base para nuestra página, en la que incorporaremos cuatro campos.

En este caso, queremos que cuando un producto esté bloqueado, todos los campos asociados se muestren en color rojo. Para conseguirlo, crearemos una variable que almacenará el estilo que queremos aplicar. Después, asignaremos esta variable a la propiedad "StyleExpr" de los campos correspondientes.

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

Una vez definida la estructura de la página, nos dirigimos al trigger "OnAfterGetRecord", donde estableceremos la lógica para verificar si el producto está bloqueado o no, y rellenaremos la variable "StyleExpresion" con el nombre del estilo correspondiente.

En este caso, utilizaremos el estilo "Attention", que es el color rojo sin negrita.

```javascript
    trigger OnAfterGetRecord()
    begin
        StyleExpresion := '';

        if Rec.Blocked then
            StyleExpresion := 'Attention';
    end;

```

<br><br><br><br>

¡Hecho! Ahora, cada vez que se recorra una línea en la página, se comprobará si el producto está bloqueado o no, y se aplicará el estilo adecuado a los campos.

<br><br>

Para darle un toque extra de limpieza y evitar "Hardcodes", he creado un enum llamado "ColourStyleExpr" con todos los colores disponibles.

De esta forma, podremos acceder a los colores de manera más intuitiva.

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

En este blog, hemos explorado un método sencillo y efectivo para mejorar la presentación de páginas y campos en Dynamics 365 Business Central. Utilizando AL, hemos aprendido cómo aplicar estilos a los campos para destacar información importante y mejorar la experiencia del usuario.

<br>

Como siempre, podréis ver el ejemplo completo en el [GitHub](https://github.com/Esanpons/ejemplos-blog/tree/main/AL/ColorEnCamposDePage)

<br>

¡Y eso es todo! Espero que esta explicación y el código os hayan sido de ayuda. Si tenéis algún comentario o pregunta, no dudéis en hacerla. ¡Hasta la próxima!

<br>

¡Y eso es todo por hoy! Espero que esta explicación y el código os hayan sido de utilidad. Si tenéis alguna duda o comentario, no dudéis en compartirlo. ¡Nos vemos en la próxima entrada sobre Dynamics 365 Business Central!
