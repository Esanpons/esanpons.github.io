---
layout: post
title: "Transformando la Gestión de Archivos en Business Central: ¡Arrastra y Suelta para la Eficiencia!"
summary: "Descubre cómo revolucionar la administración de archivos en Dynamics 365 Business Central mediante la implementación de una funcionalidad de arrastrar y soltar. En este artículo, exploramos el código y los componentes esenciales que te permitirán simplificar la gestión de documentos adjuntos."
author: "Esteve Sanpons"
date: "2023-11-13 08:00:00 +0200"
#cSpell:disable
category: ["JavaScript", "ControlAddin", "Business_Central"]
thumbnail: /assets/img/posts/arrastrar-ysoltar-documentos-adjuntos/imagen01.png
permalink: /blog/arrastrar-ysoltar-documentos-adjuntos/
custom_type: Blog
#cSpell:enable
---

Hola a todos los entusiastas de Dynamics 365 Business Central!

Hoy nos sumergiremos en un emocionante desarrollo que simplificará drásticamente la gestión de archivos adjuntos en tu Business Central. ¿Alguna vez te has preguntado cómo facilitar este proceso? ¡No busques más! Aquí te traigo una solución ingeniosa que hará que la administración de archivos sea pan comido.

 <br>

¡Así que, acompáñame mientras exploramos cada componente de este código ingenioso! ¡Vamos manos a la obra! 😤

 <br>

## Control Add-in "Attachments Drag And Drop"

Comenzamos con el "Control Add-in Attachments Drag And Drop". Este componente es esencial para habilitar la función de arrastrar y soltar archivos en Business Central. ¿Qué hace exactamente?

-   **Scripts**: El control Add-in hace referencia a dos archivos de scripts cruciales:

    -   **jquery-3.3.1.min.js**: Este archivo facilita la manipulación del DOM (Modelo de Objeto de Documento) y la gestión de eventos en la página.

    -   **scripts.js**: Aquí reside el conjunto de funciones necesarias para habilitar la función de arrastrar y soltar archivos.

-   **StartupScript**: Define un archivo de inicio llamado "startup.js". Este archivo se ejecuta automáticamente cuando el control Add-in se inicializa en una página.

-   **Altura Requerida y Mínima Altura**: Estas propiedades establecen la altura del control Add-in en la página, garantizando que se vea y se sienta como parte integral de la interfaz de usuario.

-   **HorizontalStretch**: Esta propiedad permite que el control se expanda horizontalmente para aprovechar el espacio disponible de manera eficiente.

-   **Eventos**: Aquí se definen dos eventos cruciales: `ControlAddinReady` (cuando el control Add-in está listo) y `OnFileUpload` (cuando se carga un archivo). Estos eventos son esenciales para manejar la interacción con el control en la página de Business Central.

```javascript

controladdin "Attachments Drag And Drop"
{
    Scripts = 'https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js',
        'AttachmentsDragAndDrop\src\controladdin\AttachmentsDragAndDrop\js\scripts.js';
    StartupScript = 'AttachmentsDragAndDrop\src\controladdin\AttachmentsDragAndDrop\js\startup.js';

    RequestedHeight = 1;
    MinimumHeight = 1;
    HorizontalStretch = true;

    event ControlAddinReady();
    event OnFileUpload(FileName: Text; FileAsText: Text; IsLastFile: Boolean)
    procedure InitializeFileDragAndDrop()
}


```

<br><br>

## Archivo "scripts.js"

Este archivo es el corazón de la funcionalidad de arrastrar y soltar archivos en Business Central. Permíteme ofrecerte un resumen más detallado de sus principales funciones:

-   **InitializeFileDragAndDrop()**: Esta función es la encargada de poner en marcha la funcionalidad de arrastrar y soltar archivos en el control Add-in. Detecta el elemento de la página al que se debe aplicar esta funcionalidad y configura eventos para supervisar el proceso de arrastre y suelta de archivos.

-   **PreventDefaults(e)**: Evita los comportamientos predeterminados de los eventos de arrastrar y soltar, lo que nos proporciona un control total sobre la interacción con los archivos que se arrastran y sueltan en el control Add-in de Business Central.

-   **Highlight(e)**: Realza visualmente el área de soltar cuando un elemento se arrastra sobre ella, proporcionando una experiencia intuitiva al usuario.

-   **Unhighlight(e)**: Elimina el realce del área de soltar cuando un elemento se retira o se suelta en ella, manteniendo una experiencia de usuario limpia y fácil de entender.

-   **HandleDrop(e)**: Gestiona el evento de soltar un archivo en el área designada, extrayendo los archivos soltados y llamando a la función correspondiente para manejarlos adecuadamente.

-   **UploadFile(file, i)**: Esta función se encarga de cargar un archivo en el sistema, convirtiéndolo en formato base64 y enviándolo a Business Central. Un paso clave en la administración de archivos.

```javascript
// Esta función se encarga de inicializar la funcionalidad de arrastrar y soltar archivos en el ControlAddin de Business Central.
function InitializeFileDragAndDrop() {
    // Busca el controlAddIn en el documento padre que tengan el atributo "controlname" igual a "AttachmentsDragAndDropFactbox". Es importante que tenga el mismo nombre que la page donde tendremos nuestro ControlAddIn.
    nodes = window.parent.document.querySelectorAll('div[controlname="AttachmentsDragAndDropFactbox"]');

    // Selecciona el último elemento encontrado, que será el control en el que queremos habilitar la funcionalidad.
    // Aquí es importante encontrar todos los nodos HTML posibles de esta página mediante querySelectorAll. Porque en Business Central puedes abrir varias páginas sin cerrar las anteriores.
    FactBox = nodes[nodes.length - 1];

    // Evita los comportamientos por defecto de arrastrar y soltar para los eventos especificados estándar.
    // Esto se hace para tener un control total sobre cómo se interactúa con los archivos arrastrados y soltados en el ControlAddin de Business Central.
    ["dragenter", "dragover", "dragleave", "drop"].forEach((eventName) => {
        FactBox.addEventListener(eventName, preventDefaults, false);
        // También se previenen los comportamientos por defecto a nivel de documento.
        document.body.addEventListener(eventName, preventDefaults, false);
    });

    // Cuando un elemento se arrastra sobre el área de soltar, resalta visualmente el área.
    ["dragenter", "dragover"].forEach((eventName) => {
        FactBox.addEventListener(eventName, highlight, false);
    });

    // Cuando el elemento arrastrado sale del área de soltar o se suelta en ella, se quita el resaltado.
    ["dragleave", "drop"].forEach((eventName) => {
        FactBox.addEventListener(eventName, unhighlight, false);
    });

    // Maneja el evento de soltar un archivo en el área de soltar.
    FactBox.addEventListener("drop", handleDrop, false);
}

// Esta función previene los comportamientos por defecto de los eventos de arrastrar y soltar.
function preventDefaults(e) {
    e.preventDefault(); // Previene la acción por defecto asociada al evento.
    e.stopPropagation(); // Detiene la propagación del evento en la jerarquía del DOM.
}

// Esta función se encarga de resaltar el área de soltar cuando un elemento se arrastra sobre ella.
function highlight(e) {
    FactBox.style.border = "thick dotted blue"; // Cambia el estilo de borde para indicar visualmente el área de soltar.
}

// Esta función se encarga de quitar el resaltado del área de soltar cuando un elemento sale o se suelta en ella.
function unhighlight(e) {
    FactBox.style.border = ""; // Restaura el estilo de borde a su valor por defecto.
}

// Esta función maneja el evento de soltar un archivo en el área de soltar.
function handleDrop(e) {
    var dt = e.dataTransfer; // Obtiene el objeto de transferencia de datos del evento.
    var files = dt.files; // Obtiene la lista de archivos que se soltaron en el área.

    // Llama a la función para manejar los archivos soltados.
    handleFiles(files);
}

// Variable para llevar el conteo de archivos soltados.
var filesCount = 0;

// Esta función maneja una lista de archivos.
function handleFiles(files) {
    // Convierte la lista de archivos en un arreglo para facilitar su manejo.
    files = [...files];
    // Actualiza el conteo de archivos con la cantidad de archivos soltados.
    filesCount = files.length;
    // Llama a la función de subida de archivo para cada archivo en la lista.
    files.forEach(uploadFile);
}

// Esta función se encarga de cargar un archivo al sistema.
function uploadFile(file, i) {
    var reader = new FileReader(); // Crea un nuevo lector de archivos.
    reader.addEventListener("loadend", function () {
        // Cuando se completa la lectura del archivo...
        // El resultado del lector contiene el contenido del archivo en formato base64.
        var base64data = reader.result;

        // Invoca un método de extensibilidad de Business Central para manejar la subida del archivo y enviar la información a la page de Business Central.
        Microsoft.Dynamics.NAV.InvokeExtensibilityMethod("OnFileUpload", [file.name, base64data, filesCount == i + 1]);
    });

    // Lee el contenido del archivo como una URL base64.
    reader.readAsDataURL(file);
}
```

<br><br>

## Codeunit "Variables SingleInstance"

Este codeunit desempeña un papel crucial para gestionar las referencias de registros de manera eficiente. ¿Por qué es tan importante? Bueno, necesitamos mantener el RecordRef disponible en todo momento, y los eventos estándar a día de hoy aún no nos permiten recogerlo. Pero tranquilo, ¡lo solucionaremos pronto! Aquí tienes las tres secciones principales:

-   **ClearRecordRefe()**: Esta función se encarga de borrar la referencia de registro, asegurando que todo esté limpio y listo para nuevas operaciones.

-   **SetRecordRefe(l_RecordRefe: RecordRef)**: Aquí establecemos la referencia de registro, lo que nos permite acceder a ella cuando sea necesario.

-   **GetRecordRefe()**: Por último, esta función nos permite recuperar la referencia de registro cuando la necesitemos.

Esta funcionalidad es esencial para garantizar que los registros se manejen adecuadamente durante todo el proceso de arrastrar y soltar. Utilizamos la propiedad SingleInstance para almacenar los datos para toda la instancia, lo que hace que todo funcione como un reloj suizo.

```javascript
codeunit 60001 "Variables SingleInstance"
{
    SingleInstance = true;

    #region CELAR
    procedure ClearRecordRefe()
    begin
        Clear(RecordRefe);
    end;
    #endregion

    #region SET
    procedure SetRecordRefe(l_RecordRefe: RecordRef)
    begin
        RecordRefe := l_RecordRefe;
    end;
    #endregion

    #region GET
    procedure GetRecordRefe(): RecordRef
    begin
        exit(RecordRefe);
    end;
    #endregion
    var
        RecordRefe: RecordRef;
}

```

<br><br>

## Codeunit "Events"

Este codeunit se utiliza para manejar eventos relacionados con la inicialización de campos en el registro "Document Attachment". En particular, nos centramos en el evento `OnAfterInitFieldsFromRecRef` para configurar la referencia de registro en la codeunit "Variables SingleInstance".

```javascript
codeunit 60000 "Events"
{
    #region EVENTOS
    [EventSubscriber(ObjectType::Table, Database::"Document Attachment", OnAfterInitFieldsFromRecRef, '', true, true)]
    local procedure T1173_OnAfterInitFieldsFromRecRef(var DocumentAttachment: Record "Document Attachment"; var RecRef: RecordRef)
    var
        VariablesSingleInstance: Codeunit "Variables SingleInstance";
    begin
        VariablesSingleInstance.SetRecordRefe(RecRef);
    end;
    #endregion
}
```

<br><br>

## Pagina "Attachments Drag And Drop Factbox"

La estrella de la extensión de página 81700 es el "Attachments Drag And Drop Factbox". Su función principal es integrar esta función directamente en la lista de clientes sin necesidad de modificar la página base "Customer List". Esto es genial porque te brinda acceso rápido y sencillo a documentos relacionados con clientes específicos.

**Propiedades Generales**:

-   **Caption**: Aquí definimos el título de la extensión, que en este caso es "Customer List".
-   **PageType**: La página base que estamos extendiendo es "Customer List", una lista de clientes en Business Central.

**Diseño y Funcionalidad**:
La sección de diseño de la extensión determina cómo se agregan elementos visuales y funcionales a la página base. En particular, se centra en la sección "AddFirst Factbox".

**AddFirst Factbox**:
Esta sección dicta cómo se agregan los Factbox a la página de lista de clientes.

-   **Part DragAndDropFactbox**: Aquí especificamos el componente que se añade a la página, el "Attachments Drag And Drop Factbox".
-   **ApplicationArea**: Define en qué partes de la aplicación estará disponible este Factbox, configurado como "All" para estar disponible en toda la aplicación Business Central.
-   **SubPageLink**: Permite vincular el Factbox a registros específicos en la página base "Customer List" mediante "Table ID" y "No." para garantizar que esté relacionado con clientes individuales.

**Función del Factbox**:

El "Attachments Drag And Drop Factbox" es la joya de la corona que se añade a la lista de clientes. Este Factbox permite arrastrar y soltar archivos directamente en la página de clientes, lo que simplifica enormemente la gestión de archivos adjuntos en el contexto de un cliente específico. En última instancia, esto mejora la accesibilidad y la usabilidad en la gestión de documentos.

```javascript
page 60015 "AttachmentsDragAndDropFactbox"
{
    Caption = 'Attachments Drag And Drop', Comment = 'ESP="Documentos Adjuntos Drag And Drop"';
    PageType = ListPart;
    UsageCategory = None;
    SourceTable = "Document Attachment";
    InsertAllowed = false;
    ModifyAllowed = false;
    RefreshOnActivate = true;

    layout
    {
        area(content)
        {
            usercontrol(AttachmentsDragAndDrop; "Attachments Drag And Drop")
            {
                ApplicationArea = All;

                trigger ControlAddinReady()
                begin
                    CurrPage.AttachmentsDragAndDrop.InitializeFileDragAndDrop();
                end;

                trigger OnFileUpload(FileName: Text; FileAsText: Text; IsLastFile: Boolean)
                var
                    Base64Convert: Codeunit "Base64 Convert";
                    TempBlob: Codeunit "Temp Blob";
                    RecordRefe: RecordRef;
                    FileInStream: InStream;
                    FileOutStream: OutStream;
                begin
                    TempBlob.CreateOutStream(FileOutStream, TextEncoding::UTF8);
                    Base64Convert.FromBase64(FileAsText.Substring(FileAsText.IndexOf(',') + 1), FileOutStream);
                    TempBlob.CreateInStream(FileInStream, TextEncoding::UTF8);

                    RecordRefe := VariablesSingleInstance.GetRecordRefe();

                    Rec.ID := 0;
                    Rec.SaveAttachmentFromStream(FileInStream, RecordRefe, FileName);

                    if IsLastFile then
                        CurrPage.Update(false);
                end;
            }
            repeater(Control1)
            {
                ShowCaption = false;
                field("File Name"; Rec."File Name")
                {
                    ApplicationArea = All;
                    ToolTip = 'Specifies the value of the File Name field.';
                }
                field("File Extension"; Rec."File Extension")
                {
                    ApplicationArea = All;
                    ToolTip = 'Specifies the value of the File Extensions field.';
                }
            }
        }
    }
    actions
    {
        area(Processing)
        {
            action(DownloadFile)
            {
                ApplicationArea = All;
                Caption = 'Download File';
                ToolTip = 'Download File';
                Image = Download;
                Enabled = Rec."File Name" <> '';
                trigger OnAction()
                begin
                    ExportFile(true);
                end;
            }
        }
    }

    trigger OnOpenPage()
    begin
        VariablesSingleInstance.ClearRecordRefe();
    end;

    trigger OnQueryClosePage(CloseAction: Action): Boolean
    begin
        VariablesSingleInstance.ClearRecordRefe();
    end;

    procedure SetParams(RecVariant: Variant)
    var
        RecordRefe: RecordRef;
    begin
        VariablesSingleInstance.ClearRecordRefe();

        CLEAR(RecordRefe);
        RecordRefe.GETTABLE(RecVariant);
        Rec.InitFieldsFromRecRef(RecordRefe);
    end;

    procedure ExportFile(ShowFileDialog: Boolean): Text
    var
        TempBlob: Codeunit "Temp Blob";
        FileManagement: Codeunit "File Management";
        OutStream: OutStream;
        FullFileName: Text;
    begin
        if Rec.ID = 0 then
            exit;

        if not Rec."Document Reference ID".HasValue then
            exit;

        FullFileName := Rec."File Name" + '.' + Rec."File Extension";
        TempBlob.CreateOutStream(OutStream);
        Rec."Document Reference ID".ExportStream(OutStream);
        exit(FileManagement.BLOBExport(TempBlob, FullFileName, ShowFileDialog));
    end;

    var
        VariablesSingleInstance: Codeunit "Variables SingleInstance";
}

```

<br><br>

## Page Extension 81700 "Customer List" Extends "Customer List"

La misión principal de este objeto es integrar el "Attachments Drag And Drop Factbox" directamente en la lista de clientes. Esto hace que la gestión de archivos adjuntos en el contexto de clientes individuales sea más fácil que nunca.

```javascript
pageextension 81700 "Customer List" extends "Customer List"
{
    layout
    {
        addfirst(factboxes)
        {
            part(DragAndDropFactbox; "AttachmentsDragAndDropFactbox")
            {
                ApplicationArea = All;
                SubPageLink = "Table ID" = const(Database::Customer), "No." = field("No.");
            }
        }
    }
}


```

<br><br>

En resumen, en este emocionante desarrollo para Dynamics 365 Business Central, hemos creado una funcionalidad de arrastrar y soltar archivos para simplificar la gestión de documentos adjuntos. Esto se logra a través de un Control Add-in llamado "Attachments Drag And Drop," que permite a los usuarios arrastrar archivos directamente en la aplicación.

<br>

Como siempre, puedes encontrar el ejemplo completo en [GitHub](https://github.com/Esanpons/ControlAddIns-Business-Central)

<br>

¡Preparado para simplificar la gestión de archivos en Business Central? ¡Es momento de empezar a arrastrar y soltar para una administración más eficiente! ¡No esperes más, simplifica tu flujo de trabajo ahora mismo!
