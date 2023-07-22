---
layout: post
title: "Optimizando la Exportación e Importación de Archivos en Business Central"
summary: "Descubre cómo mejorar tus procesos de exportación e importación de archivos en Business Central. Aprende a realizar estas operaciones sin depender de la codeunit 419 y optimiza tus flujos de trabajo en entornos OnPrem y SAS."
author: "Esteve Sanpons"
date: "2023-08-27 04:00:00 +0200"
#cSpell:disable
category: ["Business_Central"]
thumbnail: /assets/img/posts/descargar-y-cargar-bc/imagen01.png
permalink: /blog/descargar-y-cargar-bc/
custom_type: Blog
#cSpell:enable
---

¡Bienvenidos una vez más!

Hoy, como siempre, os traigo nuevas y útiles herramientas para trabajar con Business Central y programar de manera más eficiente.

¿Alguna vez os ha pasado que estáis trabajando en Business Central y necesitáis guardar o subir archivos? En las instalaciones locales (OnPrem), esto se puede realizar fácilmente utilizando la codeunit 419, diseñada para la gestión de ficheros.

Sin embargo, cuando trabajamos en el entorno SaaS (Software as a Service), la situación cambia y puede volverse un poco más complicada.

¡Pero no os preocupéis! Hoy os presentaré una solución para realizar estas operaciones sin la famosa codeunit 419.

<br>

¡Vamos manos a la obra! 💪

<br>

## Exportar archivos con la función 'descargar'

Comenzaremos con la función de exportar archivos. Lo primero que haremos es definir las variables necesarias para el proceso.

A continuación, asignaremos un nombre a la variable que contendrá el nombre del archivo que vamos a exportar.

Iniciaremos la variable 'TempBlob' con 'OutStream'.

En este punto, podemos agregar el contenido del archivo, ya sea un archivo de tipo JSON u otro tipo de archivo que deseemos exportar.

Finalmente, para completar esta función, inicializaremos el 'InStream' y llamaremos a la función estándar 'DownloadFromStream'.

La función 'DownloadFromStream' descargará el contenido almacenado en el 'InStream' y lo guardará en la carpeta configurada en el navegador.

```javascript
    procedure Download()
    var
        TempBlob: Codeunit "Temp Blob";
        DocOutStream: OutStream;
        DocInStream: InStream;
        FileName: Text;
    begin
        FileName := 'Ejemplo.Txt';

        TempBlob.CreateOutStream(DocOutStream);
        DocOutStream.WriteText('Hola Mundo!!!');

        TempBlob.CreateInStream(DocInStream);
        DownloadFromStream(DocInStream, '', '', '', FileName);
    end;
```

<br><br>

## Importar archivos con la función 'cargar'

Ahora, centrémonos en la función para importar archivos.

Al igual que antes, comenzamos definiendo las variables necesarias.

Primero, agregamos la llamada a la función 'UploadIntoStream' para cargar el archivo.

Podemos decidir qué hacer en caso de que haya algún error durante la carga del archivo o si el usuario decide no cargar nada. En este ejemplo, hemos optado por salir de la función en caso de que la carga no se realice correctamente o el usuario no seleccione ningún archivo.

Por último, para completar esta función, leemos el contenido del 'InStream' y lo mostramos en un mensaje.

```javascript
    procedure Upload()
    var
        DocInStream: InStream;
        FileName: Text;
        ValueText: Text;
    begin
        if not UploadIntoStream('', '', '', FileName, DocInStream) then
            exit;

        DocInStream.ReadText(ValueText);
        Message(ValueText);
    end;
```

<br><br>

Agregamos estas dos funciones a una codeunit para que puedan ser ejecutadas.

<br>

## Probando las funciones en una página

Podemos poner a prueba todo esto agregando unos botones en la lista de clientes de la siguiente manera:

Al ejecutar el botón de exportar, se creará un archivo de texto con el contenido "Hola Mundo!!".

Cuando carguemos ese archivo, veremos que el contenido se mostrará en un mensaje.

```javascript
pageextension 50100 "Extension Customer" extends "Customer List"
{
    actions
    {
        addfirst(processing)
        {
            action("Import")
            {
                ApplicationArea = Basic, Suite;
                Caption = 'Import', comment = 'ESP="Import"';
                Image = Import;
                Promoted = true;
                PromotedCategory = Category4;
                PromotedIsBig = true;
                ToolTip = 'Import', comment = 'ESP="Import"';

                trigger OnAction()
                begin
                    MgtDownloadUpload.Upload();
                end;
            }
            action("Export")
            {
                ApplicationArea = Basic, Suite;
                Caption = 'Export', comment = 'ESP="Export"';
                Image = Export;
                Promoted = true;
                PromotedCategory = Category4;
                PromotedIsBig = true;
                ToolTip = 'Export', comment = 'ESP="Export"';

                trigger OnAction()
                begin
                    MgtDownloadUpload.Download();
                end;
            }
        }
    }

    var
        MgtDownloadUpload: Codeunit "Mgt. Download/Upload";

}

```

<br><br>

En este blog, hemos aprendido a optimizar la exportación e importación de archivos en Microsoft Business Central sin utilizar la codeunit 419. Desarrollamos funciones personalizadas para descargar y cargar archivos, mejorando la eficiencia en el manejo de flujos de datos.

<br><br>

Como siempre, podréis ver el ejemplo entero en el [GitHub](https://github.com/Esanpons/ejemplos-blog/tree/main/AL/Download_And_Upload)

<br>

¡Espero que esta solución sea de gran utilidad para mejorar vuestra experiencia! Si tenéis algún comentario o pregunta, no dudéis en hacerla. ¡Hasta la próxima!
