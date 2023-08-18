---
title: "Códigos de compra estándar"
summary: "Esta funcionalidad de Navision se utiliza en los pedidos y facturas de compras para incluir automáticamente líneas de detalle que se repiten con cierta frecuencia en las compras al proveedor"
layout: article
author: Esteve Sanpons
#cSpell:disable
category: [Funcional, Manuales, Compras]
custom_type: Boveda
permalink: /boveda/3
#cSpell:enable
date: 2022-08-13 14:00:00 +0200
---

###### Códigos de compra estándar

<br>

## Introducción

Esta funcionalidad de Navision se utiliza en los pedidos y facturas de
compras para incluir automáticamente líneas de detalle que se repiten
con cierta frecuencia en las compras al proveedor.
<br><br><br>

## Códigos de compra estándar

Lo primero que hay que hacer es definir los códigos de compra. A esta
opción se accede desde Departamentos -\> Compra -\> Procesamiento de
pedidos -\> Configuración.

<img class="img-container"  src="/assets/img/articles/3/image1.png">
<br><br><br>

Los productos, cuentas, cargo producto o activos se definen agrupados en
diferentes códigos de compra, cada uno de ellos será como una plantilla
diferente. Son definiciones genéricas, sin tener en cuenta el proveedor.

Con el botón "Editar" de la cinta de opciones, se visualiza el
contenido.

<img class="img-container"  src="/assets/img/articles/3/image2.png">
<br><br><br>

Son campos obligatorios el "Tipo" y "Nº", pero también puede indicarse
la cantidad y variar la descripción que recupera el sistema por defecto.

## Pedidos y facturas de compras

La recuperación de los códigos de compra se realiza desde los pedido y
facturas de compra, con la opción "Obtener líneas de compra recurrentes"
(pestaña "Acciones" de las cinta de opciones).

<img class="img-container"  src="/assets/img/articles/3/image3.png">
<br><br><br>

La primera vez que se accede, hay que seleccionar los "Códigos de compra
estándar" asociados al proveedor indicado en el documento. Después ya
aparecen los códigos y simplemente se selecciona el que corresponda.

Automáticamente se incorporan en el pedido, recuperando el resto de
datos (precios, dtos. ...) de las fichas. Una vez introducida la
cantidad, siempre que no se haya indicado en la definición de los
códigos de compra, ya se puede registrar el documento.

<img class="img-container"  src="/assets/img/articles/3/image4.png">
<br><br><br>
