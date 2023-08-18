---
title: Diseños personalizados y Word Layouts
summary: "Cambio de diseño de los layouts en Word"
layout: article
author: Esteve Sanpons
#cSpell:disable
category: [Funcional, Manuales, Reports, "Informacion_Otros"]
custom_type: Boveda
permalink: /boveda/5
#cSpell:enable
date: 2022-08-13 15:30:00 +0200
---

###### Diseños personalizados y Word Layouts

<br>

Es innegable que los avances en los sistemas de gestión de la
información al alcance de las empresas minimizan las necesidades de
disponer de informes impresos que realicen las tareas de procesar la
información y mostrarla de forma estructurada.

Pero lo es también que la mayor parte de las empresas requiere disponer,
ya sea por razones de organización interna, características de los
destinatarios de la información o de las propias entidades procesadas
(difícil es no requerir el formato de documentos, como, por ejemplo, las
facturas de venta) de **impresos originados desde su ERP**.

Para facilitar cubrir estas necesidades, las empresas disponen
de **Microsoft Dynamics NAV**. De hecho, desde su versión 2015, las
compañías tienen a su alcance una nueva funcionalidad, complementaria al
propio sistema de desarrollo de informes impresos de la aplicación (que
utilizan *Visual Studio* o *Report Builder* como sistemas de diseño)

Son los **Diseños personalizados**, que facilitan enormemente el diseño
y uso de plantillas de impresos mediante Word (Office 2013 o superior)

Este doble sistema de desarrollo de formatos de los impresos permite
compaginar informes de mayor complejidad, realizados mediante las
propias herramientas de desarrollo de la aplicación, con otros diseñados
con plantillas de Word y que no requieren experiencia en el uso de
herramientas Visual Studio o Report Builder para el diseño del impreso.

## Utilización de Word Layouts

Determinar que un impreso utilice su versión Word es muy simple.
Únicamente necesitaremos seguir estos pasos:

-   El primer paso es acceder a la **Ventana Selección de diseño de
    informes** donde podemos buscar el impreso a modificar.

<img class="img-container"  src="/assets/img/articles/5/image1.jpg">
<br><br><br>

-   Para el impreso deseado optaremos por la opción de **_Diseño
    seleccionado_** Word (integrado). Esta opción determina que el
    impreso se ejecutará utilizando su plantilla de Word.

<img class="img-container"  src="/assets/img/articles/5/image2.jpg">
<br><br><br>

Dado que no todos los impresos disponen por defecto de una versión de
layout con Word (podéis revisar, como toma de contacto inicial, los
impresos *1304 Ventas -- Oferta* o *1305 Venta -- Confirmación* como
ejemplo de impresos que inicialmente sí disponen de esta opción) el
siguiente paso será **generar nuestros propios formatos nuevos**.

## Diseño de nuevos formatos

Vamos a mostrar la creación de uno nuevo diseño de plantilla Word para
uno de nuestros informes de Microsoft Dynamics NAV. Por ejemplo, vamos a
crear un albarán de compra personalizado.

-   El primer paso sería **crear**, en Word, **la plantilla**.
    Empezaremos por un diseño muy simple. La ventaja que ofrece Word
    es su facilidad en cuanto a la maquetación y sencillez de uso.
    Ésta será la plantilla que importaremos a Microsoft Dynamics NAV
    para asociarla con nuestro informe.

<img class="img-container"  src="/assets/img/articles/5/image3.jpg">
<br><br><br>

-   En Microsoft Dynamics NAV utilizaremos de nuevo las opciones de
    la **Ventana Selección de diseño de informes** para buscar nuestro
    informe.

<img class="img-container"  src="/assets/img/articles/5/image4.jpg">
<br><br><br>

-   Una vez encontrado el informe que deseamos adaptar seleccionamos la
    opción \*de **Diseños personalizados\*** y creamos uno En este nuevo
    diseño personalizado indicamos que queremos que su *Tipo* sea Word.

<img class="img-container"  src="/assets/img/articles/5/image5.jpg">
<br><br><br>

También cabe destacar que se puede establecer la empresa para en la que
queremos utilizar nuestro diseño personalizado. Así podemos realizar un
diseño distinto para cada una de nuestras empresas.

-   El siguiente paso sería importar nuestra plantilla de Word para
    asociarla al nuevo diseño. Utilizaremos para ello la opción
    de **_Importar Diseño_**

<img class="img-container"  src="/assets/img/articles/5/image6.jpg">
<br><br><br>

-   Una vez hecho esto se nos abrirá el Word con nuestra plantilla.
    Tenemos que mostrar la pestaña de desarrollador, **Panel de
    Asignación XML** para poder terminar nuestro trabajo.

<img class="img-container"  src="/assets/img/articles/5/image7.jpg">
<br><br><br>

Como observamos, tenemos a nuestro alcance los datos incluidos en la
estructura de tablas y campos del informe. Esto incluye imágenes,
estructuras de tipo cabecera y líneas, etc. Podemos colocarlas en el
lugar que deseemos.

Una vez terminado el diseño ya podremos ejecutar nuestro informe para
ver como ha quedado.

Una opción cómoda para la edición de informes preexistente consiste
utilizar la opción que disponemos, al imprimirlos, de guardarlos en
formato Word y utilizar ese archivo como plantilla. Solo tendremos que
ajustar el formato y cambiar los datos por las variables del panel de
asignación de XML.

<br><br><br>

[Link Original](https://blog.aitana.es/2017/01/25/diseños-personalizados-word-layouts-dynamics-nav/#respond)
