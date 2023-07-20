---
layout: post
title: "Dominando RecordRef y FieldRef: Potencia tus Desarrollos."
summary: "Descubre cómo utilizar las poderosas variables RecordRef y FieldRef en Dynamics 365 para trabajar dinámicamente con registros y campos. Aprende a modificar y obtener datos de manera eficiente, ampliando tus capacidades de desarrollo con ejemplos prácticos y sencillos de implementar."
author: "Esteve Sanpons"
date: "2023-09-10 04:00:00 +0200"
#cSpell:disable
category: ["Navision", "Business_Central"]
thumbnail: /assets/img/posts/recref-and-fieldref/imagen01.jpg
permalink: /blog/recref-and-fieldref/
custom_type: Blog
publised: false
#cSpell:enable
---

Saludos a todos! En esta ocasión, quiero presentar un tema que resultará útil tanto para Navision como para Business Central. Me gustaría centrarme en la programación en AL, aunque también puede ser relevante para aquellos que trabajen con C/AL.

Hoy vamos a sumergirnos en el fascinante mundo de RecordRef y FieldRef, dos tipos de variables que nos permitirán trabajar con registros y campos de manera dinámica en Business Central.

Para obtener información detallada sobre RecordRef, podéis consultar la documentación oficial en el siguiente [enlace](https://learn.microsoft.com/es-es/dynamics365/business-central/dev-itpro/developer/methods-auto/recordref/recordref-get-method). De manera similar, para FieldRef, podéis acceder a la documentación en el siguiente [enlace](https://learn.microsoft.com/es-es/dynamics365/business-central/dev-itpro/developer/methods-auto/fieldref/fieldref-value-method).

<br>

Sin mas, vamos manos a la obra 🥳

<br>

Voy a mostraros tres ejemplos molones para que os hagáis una idea de cómo funcionan estas variables.

<br>

Lo primero que haremos es crear la codeunit y las variables necesarias.

```javascript
    codeunit 50001 "RecRef And FieldRef"
    {
        trigger OnRun()
        begin

        end;

        var
            Customer: Record "Customer";
            MyRecordRef: RecordRef;
            MyFieldRef: FieldRef;
            TxtResult: Text;
            BooResult: Boolean;
            txt01Lbl: Label 'First Msg: client name is: %1', Comment = 'ESP="Primer Msg: el nombre del cliente es: %1"';
            txt02Lbl: Label 'New name', Comment = 'ESP="Nuevo nombre"';
            txt03Lbl: Label 'Second Msg: client name is: %1', Comment = 'ESP="Segundo Msg: el nombre del cliente es: %1"';
            txt04Lbl: Label 'After filtering, exists: %1', Comment = 'ESP="Despues de filtrar, existe: %1"';
    }


```

<br><br>

A continuación, se presentan las variables declaradas en el código:

-   Customer: Es una variable que almacenará una tabla de tipo cliente.
-   MyRecordRef: Es una variable de tipo "RecordRef". Un RecordRef es una referencia a un registro en Business Central que permite trabajar con registros de manera dinámica.
-   MyFieldRef: Es una variable de tipo "FieldRef". Un FieldRef es una referencia a un campo en Business Central que permite trabajar con campos de manera dinámica.
-   TxtResult: Es una variable de tipo "Text" que almacenará un valor de texto.
-   BooResult: Es una variable de tipo "Boolean" que almacenará un valor verdadero o falso.

Luego, hay varias variables que son de tipo "Label" que se utilizan para proporcionar texto multilingüe, nos ayudaran a la hora de mostrar mensajes.

<br><br>

### Modificar valor de cualquier campo des de una tabla especifica

Creamos la función de modificar un campo, esta función de ejemplo nos traerá ya la tabla filtrada para ser modificada.

Esta función nos permitirá el modificar cualquier campo de la tabla que le pongamos como parámetro con cualquier valor que le pasemos.

Empezamos con los parámetros:

-   Una variable tipo Variant para añadir la tabla de origen
-   Un campo tipo Integer para el numero de campo
-   Una variable tipo Variant para el dato a añadir.

```javascript
    local procedure ModificarValorDeCualquierCampoDesDeUnaTablaEspecifica(RecVariant: Variant; FieldNo: Integer; VariantValue: Variant)
    begin
        Clear(MyRecordRef);
        Clear(MyFieldRef);

        MyRecordRef.GetTable(RecVariant);
        MyFieldRef := MyRecordRef.Field(FieldNo);
        MyFieldRef.Validate(VariantValue);
        MyRecordRef.Modify(true);
    end;

```

<br><br>

El código es bastante sencillo: primero limpiamos las variables utilizadas, luego inicializamos el RecordRef con la tabla de origen y el FieldRef con el campo a modificar. A continuación, validamos el nuevo valor y finalmente guardamos los cambios en la tabla.

<br><br>

### Devolver valor de cualquier campo des de una tabla especifica

Crearemos una función para devolver un valor especifico de un campo en una tabla ya filtrada.

Empezamos con los parámetros:

-   Una variable tipo Variant para añadir la tabla de origen
-   Un campo tipo Integer para el numero de campo

También tenemos que poner como variable de devolución que sea de tipo texto, así podremos devolver el valor en tipo texto.

```javascript
    local procedure DevolverValorDeCualquierCampoDesDeUnaTablaEspecifica(RecVariant: Variant; FieldNo: Integer): Text
    begin
        Clear(MyRecordRef);
        Clear(MyFieldRef);

        MyRecordRef.GetTable(RecVariant);
        MyFieldRef := MyRecordRef.Field(FieldNo);

        exit(format(MyFieldRef.Value));

    end;

```

<br><br>

Al igual que antes, limpiamos las variables y las inicializamos con la tabla y el campo correspondientes. La diferencia es que aquí simplemente retornamos el valor del campo usando el método "Value" de FieldRef.

<br><br>

### Buscar y retornar el valor de un campo a través de ID de tabla

Este ejemplo os lo he querido añadir también porque se puede hacer muchas cosas con un ejemplo tan sencillo como este.

Se le podría sacar partido a un desarrollo multi tabla como podría ser búsqueda de datos de los pedidos de compras y ventas.

Lo primero creamos la función con los parámetros:

-   IdTable es para añadir el numero de la tabla a la que vamos a buscar.
-   Un campo tipo Integer para el numero de campo
-   El ultimo parámetro es para añadir el valor o filtro para el campo.

Por último, en este ejemplo lo que devolveremos es si ha encontrado o no algún valor

```javascript
   local procedure BuscarRetornarElValorDeUnCampoAtravesDeIdDeTabla(IdTable: Integer; FieldNo: Integer; FilterValueText: Text): Boolean
    begin
        Clear(MyRecordRef);
        Clear(MyFieldRef);

        MyRecordRef.Open(IdTable);
        MyFieldRef := MyRecordRef.Field(FieldNo);
        MyFieldRef.SetFilter(FilterValueText);

        exit(MyRecordRef.FindFirst());
    end;

```

<br><br>

Siguiendo la misma estructura, limpiamos y preparamos las variables. En esta ocasión, utilizamos el método "Open" de RecordRef para abrir la tabla según su ID. A continuación, establecemos el filtro del campo y, por último, retornamos un valor Booleano indicando si se encontraron registros que cumplan con el filtro.

<br><br>

Estos ejemplos ofrecen un amplio abanico de posibilidades para el desarrollo en Dynamics 365 y Navision. Podéis adaptarlos y aplicarlos en diferentes situaciones, como búsquedas de datos en pedidos de compras y ventas.

Espero que esta guía os haya sido útil para entender mejor el funcionamiento de RecordRef y FieldRef.

<br>

En este blog, exploramos las ventajas de utilizar RecordRef y FieldRef en Dynamics 365 y Navision. Estas variables nos permiten trabajar dinámicamente con registros y campos, facilitando la modificación y obtención de datos de manera eficiente. Con ejemplos prácticos, descubrimos cómo aplicar estas funcionalidades en diferentes escenarios, ampliando las posibilidades de desarrollo en Business Central.

<br><br>

Como siempre, podréis ver el ejemplo entero en el [Link](https://github.com/Esanpons/ejemplos-blog/tree/main/AL/ApiGraph)

<br>

¡Te deseo éxito en tus proyectos, explorando nuevas posibilidades, y no dudes en compartir tus opiniones y dudas! ¡Espero que encuentres esta información valiosa y emocionante, y nos vemos en futuras publicaciones!
