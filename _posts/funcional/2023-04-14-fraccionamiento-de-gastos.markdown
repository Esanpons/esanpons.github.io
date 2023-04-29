---
title: Fraccionamiento de Gastos

# post specific
# if not specified, .name will be used from _data/owner/[language].yml
author: Esteve Sanpons
# multiple category is not supported
category: Funcional
# multiple tag entries are possible
tags: [Manuales, Finanzas]
# disable comments on this page
comments_disable: true

# publish date
date: 2023-04-14 16:00:00 +0200
---

<!-- outline-start -->

###### Fraccionamiento de Gastos

<br>
<!-- outline-end -->

## Introducción

Esta funcionalidad de Navision se utiliza para periodificar o distribuir
un importe en diferentes períodos, independientemente de la fecha de
registro de la factura.

Generalmente, se utiliza en las facturas de compra para distribuir un
gasto en diferentes períodos, pero también se puede indicar en las
ventas y desde el pedido.

## Plantillas de fraccionamiento

Navision las utiliza para definir las diferentes formas de fraccionar un
importe, se desde Departamentos -\ Administración -\ Configuración de
la aplicación -\ Gestión financiera -\ Grupos contables.

![such a lovely place](:fraccionamiento-de-gastos-image1.png)
<br><br><br>

Se muestra una lista con todas las plantillas definidas para poder
modificar, eliminar o generar nuevas.
![such a lovely place](:fraccionamiento-de-gastos-image2.png)
<br><br><br>

Los datos necesarios de las plantillas son:
![such a lovely place](:fraccionamiento-de-gastos-image3.png)
<br><br><br>

- Código de fraccionamiento: se debe asignar un código diferente a
  cada plantilla.

- Descripción: descriptivo del código de fraccionamiento.

- Cuenta de fraccionamiento: cuenta contable donde cargar el importe
  mientras no se distribuye.

- \% de fraccionamiento: se indica el porcentaje del importe que se
  fraccionará en los distintos períodos.

- Método de calc.:

- Lineal: el importe del primer período dependerá de los días entre el
  valor indicado en "Fecha inicial" (Fecha de registro, Principio
  del período, ...) y el final del período contable, el resto de los
  períodos tendrán el mismo importe y el importe "sobrante" lo
  incluirá en un nuevo período (siempre habrá un período más del
  indicado en la plantilla).

Si realizamos un fraccionamiento de una factura registrada el 15/01/20
de 6.000 euros con la plantilla TRIMESTRAL (definida en la imagen
anterior), el resultado será:
![such a lovely place](:fraccionamiento-de-gastos-image4.png)
<br><br><br>

- Igual por período: todos los períodos tendrán el mismo importe.

- Días por período: el importe de cada período dependerá de los días
  reales de cada uno (enero -\ 31 días, febrero -\ 28/29, marzo
  -\ 31, ...).

- Definido por el usuario: en cada factura se indicará manualmente el
  importe de cada período.

- Fecha inicial: será la fecha del primer asiento que realiza para
  cargar el importe fraccionado (traspasar de la cuenta de
  fraccionamiento a la cuenta real). El resto de los traspasos
  siempre son a día 1.

- N.º de períodos: el número de períodos (definidos en "Períodos
  contables") para fraccionar el importe.

- Desc. del período: descriptivo que se incluye en los asientos de
  distribución/fraccionamiento del importe.

## Facturas compra

Aunque la funcionalidad de fraccionamiento se puede utilizar a nivel de
compras y ventas, detallaremos solo la parte de compras porque el
funcionamiento es exactamente igual.

La plantilla de fraccionamiento a utilizar se indica a nivel del detalle
de la factura, en el campo "Código de fraccionamiento" (si el campo no
está visible se tendrá que seleccionar con "Elegir columna"). Así, cada
línea de factura puede tener un fraccionamiento diferente
![such a lovely place](:fraccionamiento-de-gastos-image5.png)
<br><br><br>

Antes de registrar la factura, es útil utilizar la opción "Vista previa
de registro" para comprobar que los movimientos de contabilidad que se
realizarán se corresponden con la correcta distribución del gasto.

Si el "Método de cálculo" del código de fraccionamiento es "Definido por
el usuario", antes de registrar la factura se tiene que indicar la
distribución del importe. Hay que situarse en la línea correspondiente y
en "Líneas" seleccionar "Información adicional" y "Previsión
fraccionamiento".

![such a lovely place](:fraccionamiento-de-gastos-image6.png)
<br><br><br>

En la pantalla que se muestra, se deben indicar manualmente los importes
de cada período hasta completar el importe a fraccionar.

![such a lovely place](:fraccionamiento-de-gastos-image7.png)
<br><br><br>

Esta pantalla también sirve para comprobar, antes de registrar la
factura, el fraccionamiento realizado automáticamente por Navision.
