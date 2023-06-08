---
layout: post
title: "Implementación de firmas en informes: Un paso hacia la eficiencia"
summary: "Aprende cómo implementar la funcionalidad de firmas en tus informes para mejorar la eficiencia y seguridad de tus documentos. Descubre los pasos necesarios y las ventajas que esta característica ofrece. No dudes en aprovechar esta herramienta para añadir un toque personalizado y profesional a tus informes."
author: "Esteve Sanpons"
date: "2023-06-07 07:00:00 +0200"
#cSpell:disable
category: ["TiposDeCampos", "Reports", "Business_Central"]
thumbnail: /assets/img/posts/mediaset-en-report/imagen01.jpg
permalink: /blog/mediaset-en-report/
custom_type: Blog
#cSpell:enable
---

¡Hola a todos! Una semana más.

Voy a enlazar con los últimos posts de los que hemos estado hablando. Lo que vamos a hacer esta semana es mostrar en un informe la firma que hicimos en el post [Link](/blog/firmar-en-bc/)
y también la inserción de esta firma en un campo MediaSet, que vimos en el post [Link](/blog/agregar-imagen-en-base64-a-campo-mediaset/).

Por lo tanto, si no los habéis leído, os aconsejo que primero los vayáis a revisar.

Bueno, después de la breve explicación, vamos manos a la obra. :sunglasses:

<br>

Lo primero es llamar al ControlAddin de firma y añadir en la página de firma la función de conversión de Base64 a campo MediaSet.

Para todo esto utilizaremos la tabla de productos que ya tiene el campo, tal como vimos en los ejemplos mencionados.

Una vez que tengamos la firma insertada en un producto, toca crear el informe y ver cómo lo añadimos.

<br>

Para este caso, he creado un informe relacionado con la tabla de productos y es muy simple.

```javascript
report 60000 "Print Signature"
{
    DefaultLayout = RDLC;
    ApplicationArea = All;
    Caption = 'Print Signature', Comment = 'ESP="Imprimir firma"';
    UsageCategory = ReportsAndAnalysis;
    RDLCLayout = 'src\report\PrintSign.rdl';

    dataset
    {
        dataitem(Item; Item)
        {
            column(No; "No.")
            {
            }
            column(Picture; Picture)
            {
            }
        }
    }
}

```

<br> <br>

Como podemos ver, lo que estamos haciendo es simple. Recorremos la tabla de productos y añadiremos el número de producto y el campo "Picture" tal cual, sin más código que esto.

<br>

Ahora compilamos para que se cree el layout y lo abrimos con la aplicación de informes para poder modificarlo.

Creamos una tabla y añadimos el código.

<input type="checkbox" id="image-checkbox-02" class="image-checkbox">
<label for="image-checkbox-02"  class="image-label">
    <img class="img-container" src="/assets/img/posts/mediaset-en-report/imagen02.png">
</label>

<br><br><br>

Ahora, donde debe ir la imagen, añadimos una imagen y le ponemos las propiedades que requerimos.

<input type="checkbox" id="image-checkbox-03" class="image-checkbox">
<label for="image-checkbox-03"  class="image-label">
    <img class="img-container" src="/assets/img/posts/mediaset-en-report/imagen03.png">
</label>

<br><br><br>

Por último, guardamos todo, compilamos y ejecutamos, y veremos que nos muestra un listado de todos los productos con su firma. En nuestro caso, solo lo hemos añadido en el primero.

<br><br>

En resumen, en este post hemos abordado la tarea de mostrar una firma en un informe utilizando un campo MediaSet y la conversión de imágenes en formato Base64. Hemos repasado los posts anteriores donde discutimos cómo firmar en BC y agregar imágenes en formato Base64 a campos MediaSet.

<br><br>

Como siempre, podréis ver el ejemplo entero en el [Link](https://github.com/Esanpons/ControlAddIn-Basico-BC/tree/main/src/report)

<br>

Espero que este post haya sido útil y os ayude a implementar la funcionalidad de firma en vuestros informes. Si tenéis alguna pregunta o comentario, no dudéis en compartirlo. ¡Hasta la próxima semana!
