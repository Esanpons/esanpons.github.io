---
layout: post
title: "Introducción a los Reports con Word en Business Central"
summary: "Crea informes personalizados en Microsoft Dynamics 365 Business Central usando reports con Word."
author: "Esteve Sanpons"
date: "2023-03-22 08:00:00 +0200"
#cSpell:disable
category: ["Word", "Reports", "Business_Central"]
thumbnail: /assets/img/posts/report-word-en-bc/image-01.png
permalink: /blog/report-word-en-bc/
custom_type: Blog
#cSpell:enable
published: false
---

Buenas a todos una semana mas.

En esta publicación, aprenderemos cómo utilizar los reports con Word en Microsoft Dynamics 365 Business Central. Los reports con Word son una característica poderosa que nos permite generar informes personalizados utilizando plantillas de Word y aprovechar al máximo la flexibilidad de esta conocida herramienta de procesamiento de textos. Utilizaremos un ejemplo sencillo de report para comprender cómo funciona esta funcionalidad.

<br><br>

#### Qué es un report con Word

Un report con Word en Business Central es un informe que se genera utilizando una plantilla de Word como diseño base. Con esta funcionalidad, podemos diseñar informes personalizados, agregar formato avanzado, incluir gráficos, tablas e imágenes, y aprovechar la funcionalidad de Word para crear documentos altamente profesionales. Además, los reports con Word nos permiten fusionar datos desde Business Central directamente en la plantilla de Word, lo que facilita la generación de informes consistentes y actualizados.

Bueno, después de la explicación, vamos manos a la obra :sunglasses:

Vamos a crear un report sencillo de cliente:

```
report 50000 "Report Word"
{
UsageCategory = ReportsAndAnalysis;
ApplicationArea = All;
WordLayout = 'src/layout/ReportWord.docx';
DefaultLayout = Word;
EnableHyperlinks = true;
PreviewMode = PrintLayout;

    dataset
    {
        dataitem(Customer; Customer)
        {
            column(No; Customer."No.")
            {

            }
            column(Name; Customer.Name)
            {

            }
            column(Index; index)
            {

            }

            trigger OnAfterGetRecord()
            begin
                index += 1;
            end;
        }
    }

    var
        index: Integer;

}

```

<br>

En este ejemplo, el report tiene varias propiedades que definen su comportamiento y apariencia.
La propiedad UsageCategory indica que este report pertenece a la categoría de informes y análisis.
La propiedad ApplicationArea indica que este report se aplica a todas las áreas de la aplicación.
La propiedad WordLayout especifica la ubicación de la plantilla de Word que se utilizará como diseño del report. En este caso, la plantilla se encuentra en 'src/layout/ReportWord.docx'.
La propiedad DefaultLayout establece el diseño predeterminado como Word.
La propiedad EnableHyperlinks permite habilitar los hipervínculos en el report.
La propiedad PreviewMode define el modo de vista previa del report, que en este ejemplo se establece como PrintLayout.

<br>

Dentro del bloque dataset, se define el dataset del report y los campos que se incluirán. En este ejemplo, el DataItem "Customer" se utiliza para contener los datos del cliente. Dentro de este DataItem, se definen tres columnas: "No", "Name" e "Index". Estas columnas se corresponden con los campos del cliente que se mostrarán en el informe.

Además, en el disparador OnAfterGetRecord, se implementa una lógica adicional para incrementar el valor de la variable index cada vez que se obtiene un registro del DataItem. Esto puede ser útil si deseas agregar numeración o índices personalizados en tu informe.

<br><br><br>

#### Cómo utilizar el Word con el report en Business Central

Ahora viene la parte del layout, que la haremos con Word.
Lo primero que haremos es compilar el proyecto, la manera mas sencilla es con Ctrl + Shift + b

Esto generara el word vinculado al report. igual que DataItem con cualquier informe.

Ahora abriremos este word y en el encontraremos que podemos añadir las referencias del DataItem en nuestro Word

<img class="img-container"  src="/assets/img/posts/report-word-en-bc/image-02.png">
<br><br><br><br>

Como podemos ver lo que se tiene que hacer es ir a la pestaña de programador y clicar en botón de "Panel de asignación XML" después buscamos nuestro informe en el panel del XML y nos mostrara el DataItem.
ahora solo tenemos que añadirlo allí donde queramos.

Para crear una repetición de lineas lo que se ha de hacer es lo siguiente.

<img class="img-container"  src="/assets/img/posts/report-word-en-bc/image-03.png">
<br><br><br><br>

como podemos ver damos botón derecho encima del DataItem y esto nos desplegar la repetición que l añadiremos a una tabla previamente creada y seleccionamos la linea donde queremos la repetición.

Con esto ya tenemos nuestro layout apunto.

Resumiendo hemos creado un report vinculado con clientes para mostrar todos los clientes en un listado y que este listado fuera hecho des de un Word. Después hemos abierto el Word y hemos añadido una tabla y le hemos puesto la repetición y los campos.

<br>
<br>

Como siempre podréis ver el ejemplo entero en el [Link](https://github.com/Esanpons/report-bc)
