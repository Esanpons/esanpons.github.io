---
layout: post
title: "Introducción a los Reports con Word en Business Central"
summary: "Crea informes personalizados en Microsoft Dynamics 365 Business Central usando reports con Word."
author: "Esteve Sanpons"
date: "2023-03-22 08:00:00 +0200"
#cSpell:disable
category: ["Word", "Reports", "Business_Central"]
thumbnail: /assets/img/posts/agregar-geolocalizacion-en-bc.png
permalink: /blog/report-word-en-bc/
custom_type: Blog
#cSpell:enable
published: false
---

Buenas a todos una semana mas.

En esta publicación, aprenderemos cómo utilizar los reports con Word en Microsoft Dynamics 365 Business Central. Los reports con Word son una característica poderosa que nos permite generar informes personalizados utilizando plantillas de Word y aprovechar al máximo la flexibilidad de esta conocida herramienta de procesamiento de textos. Utilizaremos un ejemplo sencillo de report para comprender cómo funciona esta funcionalidad.

###### Qué es un report con Word

Un report con Word en Business Central es un informe que se genera utilizando una plantilla de Word como diseño base. Con esta funcionalidad, podemos diseñar informes personalizados, agregar formato avanzado, incluir gráficos, tablas e imágenes, y aprovechar la funcionalidad de Word para crear documentos altamente profesionales. Además, los reports con Word nos permiten fusionar datos desde Business Central directamente en la plantilla de Word, lo que facilita la generación de informes consistentes y actualizados.

Bueno, después de la explicación, vamos manos a la obra :sunglasses:

Vamos a crear un report sencillo de cliente:

´´´

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

´´´
</br>

En este ejemplo, el report tiene varias propiedades que definen su comportamiento y apariencia.
La propiedad UsageCategory indica que este report pertenece a la categoría de informes y análisis.
La propiedad ApplicationArea indica que este report se aplica a todas las áreas de la aplicación.
La propiedad WordLayout especifica la ubicación de la plantilla de Word que se utilizará como diseño del report. En este caso, la plantilla se encuentra en 'src/layout/ReportWord.docx'.
La propiedad DefaultLayout establece el diseño predeterminado como Word.
La propiedad EnableHyperlinks permite habilitar los hipervínculos en el report.
La propiedad PreviewMode define el modo de vista previa del report, que en este ejemplo se establece como PrintLayout.

</br>

Dentro del bloque dataset, se define el dataset del report y los campos que se incluirán. En este ejemplo, el dataitem "Customer" se utiliza para contener los datos del cliente. Dentro de este dataitem, se definen tres columnas: "No", "Name" e "Index". Estas columnas se corresponden con los campos del cliente que se mostrarán en el informe.

Además, en el disparador OnAfterGetRecord, se implementa una lógica adicional para incrementar el valor de la variable index cada vez que se obtiene un registro del dataitem. Esto puede ser útil si deseas agregar numeración o índices personalizados en tu informe.

</br>

###### Cómo utilizar el Word con el report en Business Central

Ahora viene la parte del layout, que la haremos con Word.
