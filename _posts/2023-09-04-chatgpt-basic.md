---
layout: post
title: "Integración Innovadora de Microsoft Business Central y ChatGPT"
summary: "Descubran cómo fusionar la inteligencia de Microsoft Business Central con el poder del lenguaje natural en ChatGPT. Aprendan a configurar la interacción, desde la creación de claves API hasta la creación de páginas de preguntas y respuestas."
author: "Esteve Sanpons"
date: "2023-09-01 04:00:00 +0200"
#cSpell:disable
category: ["Business_Central", ChatGPT, HttpClient]
thumbnail: /assets/img/posts/chatgpt-basic/imagen01.png
permalink: /blog/chatgpt-basic/
custom_type: Blog
#cSpell:enable
---

¡Saludos a todos en esta emocionante nueva entrega! Hoy os traigo una propuesta apasionante que no querréis perderos. 👋🚀

En esta ocasión, exploraremos cómo vincular de manera inteligente y efectiva Microsoft Business Central con el popular asistente de lenguaje, ChatGPT.

<br>

# Conectando Business Central con ChatGPT

El potencial de unir estas dos poderosas herramientas es enormemente prometedor. A través de la API proporcionada por OpenAI, disponible en este [enlace](https://platform.openai.com/docs/api-reference), podréis recibir respuestas directas desde ChatGPT dentro de Business Central.

<br>

Así que, ¡Vamos manos a la obra! 💥👊

<br>

## Creación de una Clave de API

Para comenzar, necesitaremos una cuenta registrada en OpenAI. Nos dirijamos al [enlace](https://platform.openai.com/account/api-keys) y seleccionamos "Create New Secret Key".

Aquí podremos asignar un nombre a la clave y guardar la Secret Key resultante en un lugar seguro, ya que no podremos acceder a ella posteriormente.

<br><br>

## Integración en Business Central

Vamos a desglosar paso a paso cómo integrar ChatGPT en su Business Central.

<br>

#### Estructura de la Tabla

La tabla de ejemplo consta de 4 campos esenciales:

-   API Key: Almacena la clave de API necesaria para autenticarse y conectar con la API de ChatGPT. Esto garantiza el acceso autorizado a la API.
-   Temperature: Configura la temperatura en la generación de respuestas del modelo de lenguaje. La temperatura controla la variabilidad en las respuestas generadas.
-   Max Token: Controla la longitud máxima de los tokens generados por el modelo. Los tokens son unidades de texto utilizadas por el modelo para procesar y generar respuestas.

<br>

La estructura de la tabla se detalla en el siguiente fragmento de código:

```javascript
table 50000 "ChatGPT Setup"
{
    DataClassification = CustomerContent;

    fields
    {
        field(1; "Primary Key"; Code[10])
        {
            DataClassification = CustomerContent;
            Caption = 'Primary Key';

        }
        field(10; "API Key"; Text[100])
        {
            DataClassification = CustomerContent;
            Caption = 'API Key';
        }
        field(20; "Temperature"; Decimal)
        {
            DataClassification = CustomerContent;
            Caption = 'Temperature';
            DecimalPlaces = 1;
            MinValue = 0;
            MaxValue = 1;
        }

        field(30; "Max Token"; Integer)
        {
            DataClassification = CustomerContent;
            Caption = 'Max Token';
        }
    }

    keys
    {
        key(Key1; "Primary Key")
        {
            Clustered = true;
        }
    }
}
```

<br><br>

#### Página de Configuración

La página de configuración corresponde estrechamente a la tabla. Asegúrense de crear registros en esta página para evitar problemas al editar campos más adelante.

<br>

La estructura de la página se detalla en el siguiente fragmento de código:

```javascript
page 50000 "ChatGPT Setup"
{
    ApplicationArea = All;
    UsageCategory = Administration;
    Caption = 'ChatGPT Setup';
    PageType = Card;
    SourceTable = "ChatGPT Setup";
    InsertAllowed = false;
    DeleteAllowed = false;

    layout
    {
        area(content)
        {
            group(General)
            {
                field("API Key"; Rec."API Key")
                {
                    ToolTip = 'Specifies the value of the field', comment = 'ESP="Especifica el valor del campo"';
                    ApplicationArea = All;
                    ExtendedDatatype = Masked;
                }
                field("Default Max Token"; Rec."Max Token")
                {
                    ToolTip = 'Specifies the value of the field', comment = 'ESP="Especifica el valor del campo"';
                    ApplicationArea = All;
                }
                field("Default Temperature"; Rec."Temperature")
                {
                    ToolTip = 'Specifies the value of the field', comment = 'ESP="Especifica el valor del campo"';
                    ApplicationArea = All;
                }
            }
        }
    }

    trigger OnOpenPage()
    begin
        Rec.Reset();
        if not Rec.Get() then begin
            Rec.Init();
            Rec.Insert(true);
        end;
    end;
}

```

<br><br>

#### Página de Preguntas y Respuestas

En esta sección, exploraremos cómo configurar la página que permite a los usuarios realizar preguntas y recibir respuestas a través de ChatGPT. Esta página es el punto de encuentro entre la capacidad de procesamiento de lenguaje natural y las necesidades prácticas de su empresa.

<br>

###### Grupo de Prompt

Este grupo se encarga de configurar el prompt, que es un mensaje inicial que proporciona contexto al modelo de ChatGPT. El campo "Prompt" se puede utilizar para indicar al modelo sobre el tema o el contexto en el que se realiza la pregunta.

<br>

###### Grupo de Pregunta

Aquí es donde los usuarios pueden ingresar sus preguntas. El campo "Request" captura la consulta realizada por el usuario, que será enviada a ChatGPT para obtener una respuesta.

<br>

###### Grupo de Respuesta

Una vez que la solicitud se envía y se procesa, la respuesta generada por ChatGPT se mostrará en el campo "Response". Esto permite a los usuarios ver rápidamente la información proporcionada por el modelo.

<br>

La estructura de la página se detalla en el siguiente fragmento de código:

```javascript
page 50001 "Ask ChatGPT"
{
    Caption = 'Ask ChatGPT', Comment = 'ESP="Pregunta a ChatGPT"';
    ApplicationArea = All;
    PageType = Card;
    UsageCategory = Tasks;
    InsertAllowed = false;
    DeleteAllowed = false;

    layout
    {
        area(content)
        {
            group(PromptGroup)
            {
                ShowCaption = true;
                Caption = 'Prompt', Comment = 'ESP="Prompt"';
                field(Prompt; Prompt)
                {
                    ShowCaption = false;
                    ApplicationArea = All;
                    MultiLine = true;
                }
            }
            group(RequestGroup)
            {
                ShowCaption = true;
                Caption = 'Request', Comment = 'ESP="Pregunta"';
                field(Request; Request)
                {
                    ShowCaption = false;
                    ApplicationArea = All;
                    MultiLine = true;
                }
            }
            group(ResponseGroup)
            {
                ShowCaption = true;
                Caption = 'Response', Comment = 'ESP="Respuesta"';
                field(Response; Response)
                {
                    ShowCaption = false;
                    ApplicationArea = All;
                    MultiLine = true;
                }
            }
        }
    }

    actions
    {
        area(Processing)
        {
            action(Send)
            {
                ToolTip = 'Send request', comment = 'ESP="Enviar petición"';
                Caption = 'Send', Comment = 'ESP="Enviar"';
                Image = SendTo;
                Promoted = true;
                PromotedIsBig = true;
                PromotedCategory = Process;
                PromotedOnly = true;

                trigger OnAction()
                var
                    MgtChatGPT: Codeunit "Mgt. ChatGPT";
                begin
                    Clear(MgtChatGPT);
                    MgtChatGPT.SetParams(Prompt, Request);
                    Response := MgtChatGPT.GetResponse();
                end;
            }
        }
    }

    trigger OnOpenPage()
    begin
        Prompt := Text001Txt;
    end;

    var
        Request: Text;
        Prompt: Text;
        Response: Text;
        Text001Txt: Label 'You are an assistant for users using Dynamics 365 Business Central. Answer the questions asked by the user in a simple and precise way.', Comment = 'ESP="Eres un asistente para los usuarios que usan Dynamics 365 Business Central. Responder a las preguntas realizadas por el usuario de forma sencilla y precisa."';
}

```

<br><br>

#### Codeunit de Conexión

Ahora llegamos al componente crucial: la codeunit que realiza la conexión y facilita la comunicación con ChatGPT.

<br>

###### Funciones SetParams y GetResponse

Comencemos con estas dos funciones que permiten llamar a la funcionalidad desde otros lugares.

La primera agrega los parámetros de prompt y pregunta, mientras que la segunda maneja toda la funcionalidad, incluyendo la creación de cuerpos, cabeceras, el envío de la solicitud y su respuesta.

<br>

###### Función SetHeaders

Esta función se encarga de establecer las cabeceras necesarias para realizar la llamada. Incluye la cabecera de autorización y la codificación del contenido.

<br>

###### Función SetBody

Aquí creamos el cuerpo de la solicitud en formato JSON, siguiendo la estructura requerida por la API de ChatGPT.

El mensaje enviado contiene un JsonArray con el prompt y la pregunta del usuario. Este se realiza en la función "GetMessages"

<br>

###### Función Post

Realiza la solicitud POST a la API de ChatGPT, incluyendo las cabeceras y el cuerpo previamente creados. Maneja los posibles errores y controla las respuestas.

<br>

###### Función ReadResponse

Finalmente, esta función decodifica la respuesta de ChatGPT para obtener el texto de interés y lo presenta en el campo correspondiente de la página.

<br>

La estructura de la codeunit se detalla en el siguiente fragmento de código:

```javascript
codeunit 50000 "Mgt. ChatGPT"
{
    procedure SetParams(NewPrompt: Text; NewRequest: Text)
    begin
        Prompt := NewPrompt;
        Request := NewRequest;
    end;

    procedure GetResponse() ReturnValue: text;
    var
        ResponseText: Text;
        HttpRequestMessage: HttpRequestMessage;
        HttpContent: HttpContent;
    begin
        ChatGPTSetup.Get();

        SetBody(HttpContent);
        SetHeaders(HttpContent, HttpRequestMessage);
        Post(HttpContent, HttpRequestMessage, ResponseText);
        ReturnValue := ReadResponse(ResponseText);
    end;

    local procedure SetHeaders(var HttpContent: HttpContent; var HttpRequestMessage: HttpRequestMessage)
    var
        Headers: HttpHeaders;
        AuthorizationValue: Text;
    begin
        HttpContent.GetHeaders(Headers);
        Headers.Clear();
        Headers.Add('Content-Type', 'application/json');
        HttpRequestMessage.GetHeaders(Headers);
        AuthorizationValue := 'Bearer ' + ChatGPTSetup."API Key";
        Headers.Add('Authorization', AuthorizationValue);
    end;

    local procedure SetBody(var HttpContent: HttpContent)
    var
        bodyJson: JsonObject;
        JsonData: Text;
    begin
        bodyJson.Add('messages', GetMessages());
        bodyJson.Add('model', ModelTxt);
        bodyJson.Add('max_tokens', ChatGPTSetup."Max Token");
        bodyJson.Add('temperature', ChatGPTSetup."Temperature");
        bodyJson.WriteTo(JsonData);
        HttpContent.WriteFrom(JsonData);
    end;

    local procedure GetMessages() Jarray: JsonArray
    var
        JObject: JsonObject;
    begin
        Clear(Jarray);
        if Prompt <> '' then begin
            Clear(JObject);
            JObject.Add('role', 'system');
            JObject.Add('content', Prompt);
            Jarray.Add(JObject);
        end;

        if Request <> '' then begin
            Clear(JObject);
            JObject.Add('role', 'user');
            JObject.Add('content', Request);
            Jarray.Add(JObject);
        end;
    end;

    local procedure Post(var HttpContent: HttpContent; var HttpRequestMessage: HttpRequestMessage; var ResponseText: Text)
    var
        HttpClient: HttpClient;
        ErrorTextLbl: Label 'Something Wrong. Please retry.', Comment = 'ESP="Ocurre algo. Por favor, intenta de nuevo."';
        ErrorResponseTextLbl: Label 'Something Wrong. Error:\ %1', Comment = 'ESP="Ocurre algo. Error:\ %1"';
        HttpResponseMessage: HttpResponseMessage;
    begin
        HttpRequestMessage.Content := HttpContent;
        HttpRequestMessage.SetRequestUri(URLTxt);
        HttpRequestMessage.Method := 'POST';

        if not HttpClient.Send(HttpRequestMessage, HttpResponseMessage) then
            Error(ErrorTextLbl);

        HttpResponseMessage.Content.ReadAs(ResponseText);
        if not HttpResponseMessage.IsSuccessStatusCode then
            Error(ErrorResponseTextLbl, ResponseText);

    end;

    local procedure ReadResponse(Response: Text) ReturnValue: Text
    var
        JsonObjResponse: JsonObject;
        JsonTokResponse: JsonToken;
        JsonTokChoices: JsonToken;
        JsonObjChoices: JsonObject;
        JsonObjMessage: JsonObject;
        JsonTokMessage: JsonToken;
        JsonTokTextValue: JsonToken;
        JsonArr: JsonArray;
    begin
        ReturnValue := '';
        JsonObjResponse.ReadFrom(Response);
        JsonObjResponse.Get('choices', JsonTokResponse);
        JsonArr := JsonTokResponse.AsArray();
        JsonArr.Get(0, JsonTokChoices);
        JsonObjChoices := JsonTokChoices.AsObject();
        JsonObjChoices.Get('message', JsonTokMessage);
        JsonObjMessage := JsonTokMessage.AsObject();
        JsonObjMessage.Get('content', JsonTokTextValue);
        ReturnValue := JsonTokTextValue.AsValue().AsText();
    end;

    var
        ChatGPTSetup: Record "ChatGPT Setup";
        Prompt: Text;
        Request: Text;
        URLTxt: Label 'https://api.openai.com/v1/chat/completions', Locked = true;
        ModelTxt: Label 'gpt-3.5-turbo', Locked = true;

}
```

<br><br><br>

Con esta integración entre Microsoft Business Central y ChatGPT, estaréis listos para aprovechar la potencia del lenguaje natural en las operaciones diarias. Desde consultas simples hasta interacciones más complejas, esta colaboración puede abrir un mundo de posibilidades.

<br>

Como siempre, podréis ver el ejemplo entero en el [GitHub](https://github.com/Esanpons/chatgpt-in-business-central)

<br>

Si tenéis alguna duda o inquietud, no dudéis en compartirlo. ¡Hasta la próxima y seguimos innovando! 👋🌟
