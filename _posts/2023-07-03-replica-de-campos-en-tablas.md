---
layout: post
title: "Evita errores comunes: Replicación de campos en tablas"
summary: 'Descubre cómo evitar el error "Los campos siguientes deben ser del mismo tipo..." al registrar pedidos, facturas, devoluciones....  
Aprende la importancia de replicar correctamente los campos en las tablas. Obtén consejos prácticos y soluciones para mantener la coherencia de los campos y evitar dolores de cabeza en tu sistema.'
author: "Esteve Sanpons"
date: "2023-07-03 04:00:00 +0200"
#cSpell:disable
category: ["Navision", "Business_Central"]
thumbnail: /assets/img/posts/replica-de-campos-en-tablas/imagen01.png
permalink: /blog/replica-de-campos-en-tablas/
custom_type: Blog
#cSpell:enable
---

¡Hola a todos!

En esta ocasión, quiero abordar un error común que seguramente muchos de vosotros habéis experimentado y que puede resultar muy frustrante.

¿Alguna vez os ha sucedido que al intentar registrar un pedido, una factura o una devolución, os aparece el siguiente error:
"Los campos siguientes deben ser del mismo tipo..."?

<input type="checkbox" id="image-checkbox-02" class="image-checkbox">
<label for="image-checkbox-02"  class="image-label">
    <img class="img-container" src="/assets/img/posts/replica-de-campos-en-tablas/imagen02.png">
</label>

<br><br>

Este es un error bastante habitual y suele ocurrir debido a la falta de coordinación al crear campos en las tablas relacionadas.

Es sumamente importante asegurarse de que los campos estén replicados correctamente entre las tablas.

<br>

Sin más preámbulos, ¡vamos manos a la obra! :sunglasses:

<br>

El error que se genera con mayor frecuencia, es debido a que no se crean los campos correctamente entre las tablas vinculadas. El ejemplo que vamos a seguir seria con las tablas de ventas.

<input type="checkbox" id="image-checkbox-03" class="image-checkbox">
<label for="image-checkbox-03"  class="image-label">
    <img class="img-container" src="/assets/img/posts/replica-de-campos-en-tablas/imagen03.png">
</label>

<br><br>

Como podemos observar en las tres tablas, el mismo número de campo tiene un tipo diferente.

Esto representa un problema, y aunque podríamos pensar que cada tabla es independiente y que no es necesario replicar los campos, nos hemos encontrado con casos que tienen decenas de campos personalizados.

Mantener el control de eso en cada una de las tablas sin replicar los campos resulta ser toda una odisea.

<br><br>

Este error suele ocurrir en las codeunits de registro al utilizar el comando TRANSFERFIELDS.

Un ejemplo de ello lo encontramos en la codeunit 80, que se encarga del registro de documentos de venta.

<input type="checkbox" id="image-checkbox-04" class="image-checkbox">
<label for="image-checkbox-04"  class="image-label">
    <img class="img-container" src="/assets/img/posts/replica-de-campos-en-tablas/imagen04.png">
</label>

<br><br>

Como podemos ver, lo que se está haciendo es transferir los datos de la tabla 36, correspondiente a la cabecera de ventas, a la tabla 110, que representa la cabecera de albaranes de ventas.

Debido a que los campos no son iguales, tal como hemos mostrado anteriormente, se produce el tan esperado error.

Si ya se tenéis este error, solo se puede hacer una cosa, y es crear un nuevo campo en alguna de las tablas afectadas.

Normalmente, esto se hace en aquella que está generando el desequilibrio.

Luego, se debe desarrollar una solución para transferir los datos al nuevo campo, eliminar el campo que está causando problemas y replicarlo en las demás tablas.

Siguiendo el ejemplo anterior, el resultado sería el siguiente:

<input type="checkbox" id="image-checkbox-05" class="image-checkbox">
<label for="image-checkbox-05"  class="image-label">
    <img class="img-container" src="/assets/img/posts/replica-de-campos-en-tablas/imagen05.png">
</label>

<br><br>

En algunas tablas, hemos deshabilitado el campo, ya que no es necesario en ellas. Esto es lo que se debería hacer al replicar un campo: habilitarlo solo cuando sea necesario para la tabla correspondiente.

<br><br><br><br>

## Replica de campos

Ahora, seguramente os preguntareis cuáles son las tablas en las que se deben replicar estos campos para evitar que el error vuelva a ocurrir. Aquí os dejo la lista de las que he encontrado hasta ahora:

<br>

#### Compras y ventas

-   Cabeceras de ventas: <specialDiv style="color: #6BB47F">36, 110, 112, 114, 5107, 6660.</specialDiv>
-   Líneas de ventas: <specialDiv style="color: #FF4C4C">37, 111, 113, 115, 5108, 6661.</specialDiv>
-   Cabeceras de compras: <specialDiv style="color: #4C63D6">38, 120, 122, 124, 5109, 6650.</specialDiv>
-   Líneas de compras: <specialDiv style="color: #BFC681">39, 121, 123, 125, 5110, 6651.</specialDiv>

<br>

#### Clientes/proveedores/bancos/contactos

-   Tablas relacionadas: <specialDiv style="color: #980EA9">18, 23, 270, 5050.</specialDiv>

<br>

#### Cartera

-   Cabeceras de cartera: <specialDiv style="color: #02FFF7">295, 297.</specialDiv>
-   Líneas de cartera: <specialDiv style="color: #2EFF00">296, 298.</specialDiv>

<br>

#### Servicios

-   Cabeceras de servicios: <specialDiv style="color: #8E0965">5900, 5990, 5992, 5994.</specialDiv>
-   Líneas de servicios: <specialDiv style="color: #8E6809">5901, 5989, 5993, 5995, 5902, 5991.</specialDiv>
-   Cabeceras de contrato: <specialDiv style="color: #098E27">5965, 5970.</specialDiv>
-   Líneas de contrato: <specialDiv style="color: #8E0909">5964, 5971.</specialDiv>

<br>

#### Almacén

-   Cabeceras de picking: <specialDiv style="color: #E4FF00">5766, 5772</specialDiv>
-   Líneas de picking: <specialDiv style="color: #FF00F0">5767, 5773</specialDiv>

<br><br>

Se recomienda en Business Central para una mayor facilidad de replica el poner en las tableextension un identificativo de la replica, así se pueden buscar con mayor facilidad, en todas las tablas relacionadas y con un simple copiar y pegar estarán todas las tablas replicadas.

```javascript
tableextension 60000 "Sales Header" extends "Sales Header"
{
    //REPLICA CAMPOS (36, 110, 112, 114, 5107, 6660)
    fields
    {
    }
}

```

<br><br><br><br>

¡Y eso es todo! Espero que esta explicación os hayan sido de ayuda. Si tenéis algún comentario o pregunta, no dudéis en hacerla. ¡Hasta la próxima semana!
