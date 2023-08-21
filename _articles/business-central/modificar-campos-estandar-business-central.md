---
title: "Modificar campos estándar en Business Central"
summary: "Modificar campos estándar en Business Central"
layout: article
author: Esteve Sanpons
#cSpell:disable
category: [Business_Central]
custom_type: Boveda
permalink: /boveda/modificar-campos-estandar-business-central
#cSpell:enable
date: 2023-06-05 21:00:00 +0200
LinkedIn: false
posible_blog: true
---

Os pongo en situación, como podéis ver en la tabla de cabecera de venta el campo de numero de cliente tiene el caption estándar.

<input type="checkbox" id="image-checkbox-01" class="image-checkbox">
<label for="image-checkbox-01"  class="image-label">
    <img class="img-container" src="/assets/img/articles/modificar-campos-estandar-business-central/Imagen01.png">
</label>
<br>

También podemos ver que tiene una relación con la tabla cliente.

Sabemos que estos datos no se pueden cambiar ya que el estándar, el Core de Business Central no se puede tocar.
Pero estos chicos de Microsoft han ideado una manera de que podamos toquetear ni que sea un poquito el Core para poder hacer nuestras modificaciones un poco mas personalizadas.

<br><br>

Lo que haremos es cambiar la tabla relacionada del campo numero de cliente por la tabla de proveedores.
Primero de todo creamos una tableextension de la tabla de cabecera de ventas.
Añadimos “modify” y ponemos el campo que queremos modificar.

```javascript

tableextension 50100 "MGSMyExtension" extends "Sales Header"
{
    fields
    {
        modify("Sell-to Customer No.")
        {
            Caption = 'Sell-to Vendor No.', Comment = 'ESP="Nº proveedor"';
        }

        modify("Sell-to Customer Name")
        {
            Caption = 'Sell-to Vendor Name', Comment = 'ESP="Nombre proveedor"';
        }
    }
}

```

<br><br>

Dentro nos dará unas opciones y entre ellas esta el cambiar el caption, que en nuestro caso pondremos el numero y el nombre del proveedor.

Ahora hacemos una pageextension de la ficha de pedidos.
Aquí hacemos lo mismo que antes, pero esta vez creamos el trigger del OnLookup.

```javascript

pageextension 50100 "MGSMyExtension" extends "Sales Order"
{
    layout
    {
        modify("Sell-to Customer No.")
        {
            Caption = 'Sell-to Vendor No.', Comment = 'ESP="Nº proveedor"';
            trigger OnLookup(var Text: Text): Boolean
            var
                Vendor: Record Vendor;
                VendorList: Page "Vendor List";
            begin
                clear(VendorList);
                if VendorList.RunModal() = ACTION::OK then begin
                    VendorList.GetRecord(Vendor);
                    rec."Sell-to Customer No." := Vendor."No.";
                end;
            end;
        }
    }
}

```

<br><br>

Este trigger anula el TableRelation y le podemos poner lo que nosotros necesitemos que en este caso es la tabla de proveedores.

Este código lo que hará es abrir la page de proveedores y al escoger un proveedor lo que haremos es recoger la opción y ponerla en el campo de número de cliente.

Este código lo podríamos poner en la tabla, pero para este ejemplo he decidido ponerlo en la página.

Ahora después de compilar y generar el fichero de traducciones y subir la extensión, veremos que los cambios ya están activos en los campos.

Por ejemplo, en la página de tipo lista de los pedidos no hay captions como en la de ficha por eso automáticamente sin hacer nada nos sale los cambios hechos en la tabla.

<input type="checkbox" id="image-checkbox-02" class="image-checkbox">
<label for="image-checkbox-02"  class="image-label">
    <img class="img-container" src="/assets/img/articles/modificar-campos-estandar-business-central/Imagen02.png">
</label>
<br><br>

Y si abrimos una ficha y seleccionamos en los tres puntitos.
Nos abrirá la página de proveedores.

<input type="checkbox" id="image-checkbox-03" class="image-checkbox">
<label for="image-checkbox-03"  class="image-label">
    <img class="img-container" src="/assets/img/articles/modificar-campos-estandar-business-central/Imagen03.png">
</label>
<br><br>

GitHub de los ejemplos en el [Link](https://github.com/Esanpons/ejemplos-blog/tree/main/AL/ModifyFieldProperties)
