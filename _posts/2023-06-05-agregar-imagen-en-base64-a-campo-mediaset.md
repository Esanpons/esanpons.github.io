---
layout: post
title: "Añadiendo imágenes a campos MediaSet en Business Central: Convertir y enriquecer tu contenido"
summary: "Descubre cómo convertir imágenes en formato Base64 y agregarlas a campos MediaSet en Business Centra. Aprende a mejorar tus proyectos al incorporar de manera eficiente imágenes a tus registros y enriquecer tu contenido visual"
author: "Esteve Sanpons"
date: "2023-06-05 07:01:00 +0200"
#cSpell:disable
category: ["Base64", "TiposDeCampos", "Business_Central"]
thumbnail: /assets/img/posts/agregar-imagen-en-base64-a-campo-mediaset/imagen01.jpg
permalink: /blog/agregar-imagen-en-base64-a-campo-mediaset/
custom_type: Blog
#cSpell:enable
---

¡Hola a todos! Espero que hayáis empezado bien la semana.

<br>

Hoy quiero compartir con vosotros un proceso sencillo pero muy útil: cómo convertir una imagen en Base64 y luego añadirla a un campo de tipo MediaSet.

Si queréis obtener más información sobre este campo, podéis consultar la documentación oficial en este [enlace](https://learn.microsoft.com/es-es/dynamics-nav/mediaset-data-type).

<br>

Bueno, después de esta breve explicación, ¡pongámonos manos a la obra! 😆

<br>

Para empezar, vamos a crear una codeunit con dos funciones.

La primera función se encargará de subir la imagen y convertirla en Base64.

El código para ello sería el siguiente:

```javascript

    procedure Upload()
    var
        Base64Convert: Codeunit "Base64 Convert";
        DocInStream: InStream;
        FileName: Text;
        ValueText: Text;
    begin
        if not UploadIntoStream('', '', '', FileName, DocInStream) then
            exit;

        Clear(Base64Convert);
        ValueText := Base64Convert.ToBase64(DocInStream);
        Base64ToPicture(ValueText);
    end;

```

<br> <br>

En este código, hemos utilizado la codeunit "Base64 Convert" para convertir lo que hemos subido con la función "UploadIntoStream".

Después, hemos utilizado un "InStream" para transformarlo en Base64 y finalmente llamamos a la función "Base64ToPicture".

<br> <br>

Ahora, pasemos a la función "Base64ToPicture". En esta función, vamos a convertir el texto en formato Base64 y lo añadiremos a un campo de tipo MediaSet, en este caso el campo "Picture" de la tabla "Item".

Aquí está el código correspondiente:

```javascript

    procedure Base64ToPicture(Base64Text: Text)
    var
        Item: Record Item;
        Base64Convert: Codeunit "Base64 Convert";
        TempBlob: Codeunit "Temp Blob";
        DocOutStream: OutStream;
        DocInStream: InStream;
    begin
        Clear(TempBlob);
        Clear(Base64Convert);
        Clear(DocOutStream);
        Clear(DocInStream);

        TempBlob.CreateOutStream(DocOutStream);
        Base64Convert.FromBase64(Base64Text, DocOutStream);
        TempBlob.CreateInStream(DocInStream);

        Item.FindFirst();
        Clear(Item.Picture);
        Item.Picture.ImportStream(DocInStream, 'sign', 'image/jpeg');
        Item.Modify();
    end;

```

<br> <br>

En este código, observamos que convertimos el texto de formato Base64 a un "InStream" pasando por un "OutStream".

Es importante mencionar que en la función anterior teníamos directamente el "InStream", pero de esta forma también os muestro cómo convertir de un "OutStream" a un "InStream".

<br>

Una vez que tenemos el "InStream" preparado, inicializamos la variable "Item" y luego importamos el "InStream" dentro del campo "Picture" de la tabla "Item".

Es crucial especificar el tipo de imagen, en este caso "image/jpeg", para que pueda ser correctamente leída posteriormente en la página de productos.

<br> <br>

En resumen, hemos explorado cómo convertir una imagen en Base64 y agregarla a un campo de tipo MediaSet en Business Central. A través del código proporcionado y los pasos explicados, has aprendido a realizar esta tarea de manera efectiva. Ahora podrás aprovechar esta funcionalidad y mejorar tus proyectos al manipular imágenes de manera más eficiente. ¡No dudes en aplicar estos conocimientos y seguir explorando las posibilidades de Business Central!

<br>
<br>

Como siempre, podréis ver el ejemplo entero en el [Link](https://github.com/Esanpons/ejemplos-blog/tree/main/AL/Base64ToMediset)

<br>

¡Y eso es todo! Espero que esta explicación y el código os hayan sido de ayuda. Si tenéis algún comentario o pregunta, no dudéis en hacerla. ¡Hasta la próxima!
