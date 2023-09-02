---
layout: post
title: "División Inteligente de Líneas"
summary: "Descubre cómo realizar una división precisa de líneas en tus documentos. Aprende una solución ingeniosa para agregar nuevos elementos entre líneas existentes sin complicaciones."
author: "Esteve Sanpons"
date: "2023-07-31 04:00:00 +0200"
#cSpell:disable
category: ["Navision", "Business_Central"]
thumbnail: /assets/img/posts/split-line/imagen01.png
permalink: /blog/split-line/
custom_type: Blog
#cSpell:enable
---

¡Bienvenidos nuevamente a otro artículo lleno de conocimiento y soluciones prácticas! En esta ocasión, exploraremos una funcionalidad sumamente útil, no es otra que la división de líneas dentro de documentos en Navision y business Central.

Imaginemos una situación común: has registrado un pedido en tu sistema y, posteriormente, te das cuenta de que necesitas agregar un comentario o una nota entre dos líneas del albarán. Desafortunadamente, en la mayoría de los sistemas, una vez que se ha generado el albarán, modificar las líneas existentes puede resultar complicado o incluso imposible.

¿Cómo podemos resolver este desafío? ¡Aquí es donde entra en acción la herramienta de hoy! En este artículo, aprenderemos cómo dividir una línea y añadir una nueva en medio de dos líneas existentes en un albarán.

¡Sí, es posible y te mostrare cómo hacerlo!

<br>

Así que, ¡Vamos manos a la obra! 😎

<br>

Lo primero que haremos es crear una función con los parámetros necesarios y las variables requeridas para llevar a cabo esta tarea.

```javascript

codeunit 50000 "DividirLinea"
{
    procedure AddNewLine(Rec: Record "Sales Shipment Line"; NewComment: Text[50])
    var
        SalesShipmentLine: Record "Sales Shipment Line";
        NextLineNo: Integer;
        InitialLineNo: Integer;
    begin
        SalesShipmentLine.Reset();
        SalesShipmentLine.SetRange("Document No.", Rec."Document No.");
        SalesShipmentLine.SetFilter("Line No.", '>=%1', Rec."Line No.");
        SalesShipmentLine.FindSet();

        InitialLineNo := Rec."Line No.";

        IF SalesShipmentLine.NEXT() <> 0 THEN
            NextLineNo := SalesShipmentLine."Line No."
        ELSE
            NextLineNo := SalesShipmentLine."Line No." + 10000;

        SalesShipmentLine.INIT();
        SalesShipmentLine."Line No." := (Rec."Line No." + ((NextLineNo - InitialLineNo) DIV 2));
        SalesShipmentLine."Document No." := Rec."Document No.";
        SalesShipmentLine.Type := SalesShipmentLine.Type::" ";
        SalesShipmentLine.Description := NewComment;
        SalesShipmentLine.Insert();
    end;
}

```

<br><br>

Explicando el código, podemos ver que primero filtramos las líneas del albarán a partir de la línea seleccionada. Esto nos permitirá trabajar únicamente con las líneas relevantes, evitando complicaciones innecesarias.

Luego, almacenamos el número de línea actual y el siguiente. De esta manera, podemos calcular el punto medio entre ellos, lo que nos permitirá insertar nuestra nueva línea justo en ese espacio, manteniendo todo en orden y secuencial.

Una vez realizado el cálculo, creamos la nueva línea y la insertamos en el albarán. Es importante destacar que este proceso es similar a cualquier otra inserción en una tabla de base de datos, pero la clave aquí es el cálculo inteligente del número de línea para asegurarnos de que siempre se coloque entre las dos líneas originales.

<br>

En resumen, hemos explorado una funcionalidad invaluable para desarrolladores que desean agregar cualquier cosa entre líneas. Mediante una inteligente solución de programación, aprendimos a dividir líneas y colocar nuevos comentarios en el punto medio entre ellas.

<br><br>

Como siempre, podréis ver el ejemplo entero en el [GitHub](https://github.com/Esanpons/ejemplos-blog/tree/main/AL/DividirLinea)

<br>

¡Y eso es todo! Espero que esta explicación y el código os hayan sido de ayuda. Si tenéis algún comentario o pregunta, no dudéis en hacerla. ¡Hasta la próxima!
