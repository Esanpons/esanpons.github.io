---
title: " Guía Definitiva para Convertir de CAL a AL con la herramienta de txt2al"
summary: "Aprende cómo utilizar la poderosa herramienta txt2al para realizar una migración fluida de tus objetos de CAL a AL en Microsoft Dynamics 365 Business Central. Descubre los pasos detallados, ejemplos prácticos y una versión avanzada para una experiencia aún más sencilla."
layout: article
#cSpell:disable
category: ["Business_Central", Navision]
custom_type: Boveda
permalink: /boveda/txt2al
#cSpell:enable
author: Esteve Sanpons
date: 2023-07-21 02:00:00 +0200
LinkedIn: false
---

Si estás considerando realizar una migración de CAL a AL o, en otras palabras, de Navision a Business Central, estás en el lugar adecuado. Hoy quiero presentarte una herramienta que nos llega del estándar y que facilitará la traspaso de toda la funcionalidad de nuestros objetos de CAL a objetos válidos de AL.

La herramienta en cuestión es el "txt2al", y puedes encontrar la documentación oficial en el siguiente [Link](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/developer/devenv-txt2al-tool)

El txt2al nos permite convertir los objetos que tenemos en formato texto, pasando del código CAL al código AL. Sin embargo, antes de utilizar esta herramienta, es fundamental que podamos descargar los objetos a un archivo de texto, o en caso contrario, solicitarle al partner que nos los proporcione.

<br>

Para ilustrar este proceso, he creado un ejemplo sencillo con una tabla y una página que veremos a continuación:

```javascript
OBJECT Table 50000 Custom Customer Ledger Entry
{
  OBJECT-PROPERTIES
  {
    Date=28/02/23;
    Time=[ 9:41:37];
    Modified=Yes;
    Version List=;
  }
  PROPERTIES
  {
    CaptionML=[ENU=Custom Customer Ledger Entry;
               ESP=Mov de cliente personalizado];
  }
  FIELDS
  {
    { 1   ;   ;Entry No.           ;Integer       ;CaptionML=[ENU=Entry No.;
                                                              ESP=Nº mov.] }
    { 2   ;   ;Customer No.        ;Code20        ;TableRelation=Customer.No.;
                                                   OnValidate=BEGIN
                                                                Customer.GET("Customer No.");
                                                                "Customer Name" := Customer.Name;
                                                              END;

                                                   CaptionML=[ENU=Customer No.;
                                                              ESP=Nº cliente] }
    { 3   ;   ;Customer Name       ;Code20        ;TableRelation=Customer.Name WHERE (No.=FIELD(Customer No.));
                                                   ValidateTableRelation=No;
                                                   CaptionML=[ENU=Customer Name;
                                                              ESP=Nombre cliente] }
    { 4   ;   ;Is blocked          ;Boolean       ;FieldClass=FlowField;
                                                   CalcFormula=Lookup(Customer.Blocked WHERE (No.=FIELD(Customer No.)));
                                                   CaptionML=[ENU=Is blocked;
                                                              ESP=Esta bloqueado] }
    { 5   ;   ;Comment             ;Text250       ;CaptionML=[ENU=Comment;
                                                              ESP=Comentarios] }
  }
  KEYS
  {
    {    ;Entry No.                               ;Clustered=Yes }
  }
  FIELDGROUPS
  {
  }
  CODE
  {
    VAR
      Customer@1000000000 : Record 18;

    BEGIN
    END.
  }
}

```

<br><br>

```javascript
OBJECT Page 50000 Custom Customer Ledger Entry
{
  OBJECT-PROPERTIES
  {
    Date=28/02/23;
    Time=[ 9:42:01];
    Modified=Yes;
    Version List=;
  }
  PROPERTIES
  {
    SourceTable=Table50000;
    PageType=List;

  }
  CONTROLS
  {
    { 1000000000;0;Container;
                ContainerType=ContentArea }

    { 1000000001;1;Group  ;
                Name=Group;
                GroupType=Repeater }

    { 1000000002;2;Field  ;
                SourceExpr="Entry No.";
                Editable=false }

    { 1000000003;2;Field  ;
                SourceExpr="Customer No." }

    { 1000000004;2;Field  ;
                SourceExpr="Customer Name";
                Editable=false }

    { 1000000005;2;Field  ;
                SourceExpr="Is blocked";
                Visible=false }

    { 1000000006;2;Field  ;
                SourceExpr=Comment }

  }
}

```

<br><br>

Una vez descargados los objetos y guardados en archivos de texto, procederemos a preparar el terreno para la conversión. Para ello, crearemos dos carpetas, una llamada "CAL" y otra llamada "AL", ambas ubicadas en la raíz de la unidad C.

Luego, copiaremos los archivos descargados en la carpeta "CAL", donde guardaremos temporalmente nuestros objetos.

A continuación, localizaremos la herramienta "txt2al", la cual suele encontrarse en la carpeta de instalación de Navision. La ruta de acceso puede variar según la versión, pero para nuestro ejemplo, estará en la ruta especificada.

<input type="checkbox" id="image-checkbox-02" class="image-checkbox">
<label for="image-checkbox-02"  class="image-label">
    <img class="img-container" src="/assets/img/articles/txt2al/imagen02.png">
</label>

<br><br>

Con todo listo, abriremos una terminal como administrador y nos situaremos en la carpeta mencionada anteriormente.

<input type="checkbox" id="image-checkbox-03" class="image-checkbox">
<label for="image-checkbox-03"  class="image-label">
    <img class="img-container" src="/assets/img/articles/txt2al/imagen03.png">
</label>

<br><br>

A continuación, ejecutaremos el siguiente comando para llevar a cabo la conversión de los objetos de CAL a AL:

<input type="checkbox" id="image-checkbox-04" class="image-checkbox">
<label for="image-checkbox-04"  class="image-label">
    <img class="img-container" src="/assets/img/articles/txt2al/imagen04.png">
</label>

<br><br>

El proceso de conversión comenzará, y al finalizar, se mostrarán los objetos convertidos junto con los posibles errores que hayan ocurrido.

¡Felicidades! Ahora, si exploramos la carpeta "AL", encontraremos los objetos ya convertidos y listos para su uso.

Es importante tener en cuenta que, en ocasiones, puede ser necesario realizar algunos ajustes después de la conversión para evitar errores. Para minimizar este tipo de problemas, es recomendable ejecutar todo este proceso desde la última versión de BC14 que tenía CAL y, por ende, la herramienta txt2al.

Si deseas probar este proceso de forma práctica, he preparado algunos ejemplos en mi [GitHub](https://github.com/Esanpons/ejemplos-blog/tree/main/Varios/txt2al/Basico) para que puedas descargarlos y ejecutarlos.

<br><br>

# Modo avanzado

Para aquellos que deseen una alternativa más sencilla y rápida, he creado una versión avanzada de la herramienta. Consiste en una carpeta con las DLL necesarias y un ejecutable para una experiencia aún más fluida.

Para comenzar, descarga la carpeta "Avanzado" desde mi [GitHub](https://github.com/Esanpons/ejemplos-blog/tree/main/Varios/txt2al/Avanzado) y descomprímela. A continuación, copia el contenido en la unidad "C" o descárgala directamente en dicha unidad. Es importante que las tres carpetas estén en la raíz para que funcione correctamente.

Una vez descargados los objetos que deseas convertir, cópialos en la carpeta "CAL", tal como hicimos en el proceso anterior.

Luego, entra en la carpeta "EjecuteTxt2AL" y ejecuta el archivo "\_process.bat" como administrador.

Al finalizar, podrás dirigirte a la carpeta "AL" y encontrar los archivos convertidos listos para ser utilizados.

¡Y eso es todo! Ahora puedes convertir tus objetos de CAL a AL de manera más eficiente.

Espero que esta herramienta te resulte útil en tus futuros proyectos de migración y que te ahorre tiempo y esfuerzo.
