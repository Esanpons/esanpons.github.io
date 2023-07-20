---
layout: post
title: "Implementando un diálogo de progreso"
summary: "Aprende a implementar un diálogo de progreso. Descubre cómo crear una codeunit, definir variables y desarrollar funciones para abrir, actualizar y cerrar el diálogo. Mejora la experiencia del usuario en tus proyectos y mantén informados a los usuarios sobre el progreso de las tareas."
author: "Esteve Sanpons"
date: "2023-07-23 04:00:00 +0200"
#cSpell:disable
category: ["Navision", "Business_Central", "Utils"]
thumbnail: /assets/img/posts/dialogo-de-progreso/imagen01.png
permalink: /blog/dialogo-de-progreso/
custom_type: Blog
#cSpell:enable
---

¡Hola a todos! Espero que estén teniendo un excelente día.

Hoy quiero compartir con ustedes un concepto básico pero sumamente útil tanto en Navision como en Business Central: el diálogo de progreso.

Un diálogo de progreso es una herramienta comúnmente utilizada cuando necesitamos recorrer un proceso o una tabla mientras mostramos un diálogo al usuario para mantenerlo informado sobre el avance. En este blog, explicaré cómo implementar un código en AL que logra precisamente eso.

<input type="checkbox" id="image-checkbox-02" class="image-checkbox">
<label for="image-checkbox-02"  class="image-label">
    <img class="img-container" src="/assets/img/posts/dialogo-de-progreso/imagen02.png">
</label>

<br><br><br>

Bueno, después de esta breve explicación, ¡pongámonos manos a la obra! 😎

<br>

Comencemos por crear una codeunit y agregar las variables necesarias

```javascript
codeunit 70101 "Mgt. Dialogo De Proceso"
{
    trigger OnRun()
    begin

    end;

    var
        MyDialog: Dialog;
        MyNext: Integer;
        MaxCount: Integer;
}
```

<br><br><br>

La primera función que vamos a desarrollar es la de apertura del diálogo.

En esta función, es importante destacar que debemos pasar el total de líneas a recorrer como parámetro.

```javascript
    procedure ProcessDialogOpen(NewMaxCount: Integer)
    var
        Text001Lbl: Label 'Progress to %1: ', Comment = 'ESP="Progreso hasta %1: "';
        Text002Lbl: Label '#1#####', Locked = true;
        ValueText: Text;
    begin
        MyNext := 0;
        MaxCount := NewMaxCount;
        ValueText := StrSubstNo(Text001Lbl, MaxCount) + Text002Lbl;
        MyDialog.Open(ValueText, MyNext);
    end;
```

<br><br><br>

La función de actualización es bastante sencilla.

Esta función será llamada en cada iteración del bucle.

```javascript
    procedure ProcessDialogUpdate()
    begin
        MyNext := MyNext + 1;
        MyDialog.Update();
    end;
```

<br><br><br>

Por último, tenemos la función para cerrar el diálogo.

```javascript
    procedure ProcessDialogClose()
    begin
        MyDialog.Close();
    end;
```

<br><br><br>

Ahora, para utilizar estas funciones, les mostraré un ejemplo en el que recorremos la tabla de clientes.

```javascript
    trigger OnRun()
    var
        Customer: Record Customer;
    begin
        if Customer.FindSet() then begin
            ProcessDialogOpen(Customer.Count);

            repeat
                ProcessDialogUpdate();

            until Customer.Next() = 0;

            ProcessDialogClose();
        end;
    end;
```

<br><br><br>

Esto nos mostrar el dialogo y lo va actualizando en cada uno de nuestros clientes.

<br>

En este artículo, hemos aprendido a implementar un diálogo de progreso en código AL para Navision y Business Central. Este diálogo es útil para mostrar el avance de un proceso o recorrido de tabla al usuario.

Creamos una codeunit y definimos variables. Luego, desarrollamos funciones para abrir, actualizar y cerrar el diálogo. Utilizamos un ejemplo de recorrido de la tabla de clientes para ilustrar su uso.

Ahora podrás utilizar esta herramienta para mejorar la experiencia del usuario en tus proyectos de Navision o Business Central.

<br>
<br>

Como siempre, podréis ver el ejemplo entero en el [Link](https://github.com/Esanpons/ejemplos-blog/tree/main/AL/DialogoDeProceso)

<br>

¡Y eso es todo! Espero que esta explicación y el código os hayan sido de ayuda. Si tenéis algún comentario o pregunta, no dudéis en hacerla. ¡Hasta la próxima!
