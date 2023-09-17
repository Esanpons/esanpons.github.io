---
layout: post
title: "Maximizando la Inteligencia Artificial en Business Central: ChatGPT y la Interacción con Datos de Clientes"
summary: "Descubre cómo llevar la integración de ChatGPT y Business Central al siguiente nivel, utilizando datos de tu Base de Datos para obtener respuestas más precisas. Aprende a enviar información de clientes y realizar preguntas específicas, como analizar ventas y más."
author: "Esteve Sanpons"
date: "2023-09-09 04:00:00 +0200"
#cSpell:disable
category: ["Business_Central", ChatGPT, HttpClient]
thumbnail: /assets/img/posts/chatgpt-avanzado/imagen01.png
permalink: /blog/chatgpt-avanzado/
custom_type: Blog
#cSpell:enable
---

Hace poco, exploramos la emocionante integración de ChatGPT y Dynamics 365 Business Central. Puedes consultar los detalles en este [enlace](/blog/chatgpt-basic/).

En ese blog, desglosamos el proceso de cómo conectarse a la API de ChatGPT y realizar consultas desde nuestra instancia de Business Central. Sin embargo, hoy estamos dando un paso más allá. En esta ocasión, vamos a sumergirnos en el mundo de los datos al enviar información directamente desde nuestra Base de Datos.

Usaremos esta información para hacer consultas aún más específicas y obtener respuestas contextualizadas de ChatGPT.

Imaginemos un escenario en el que enviemos datos de nuestros clientes a ChatGPT y, a través de esos datos, planteemos preguntas que nos brinden respuestas valiosas. Desde cuantificar la cantidad de clientes en nuestra base hasta identificar las ventas generadas por clientes, e incluso descubrir quién es nuestro cliente más lucrativo.

Es fundamental comprender que cuanto más ricos sean los datos que proporcionamos, más profunda será la comprensión de ChatGPT y, por ende, más precisa y útil será su respuesta.

<br>

Después de la explicación, ¡Vamos manos a la obra! 🤗

<br>

Lo primero en nuestra lista es efectuar una pequeña modificación en la página que habíamos creado para hacer preguntas.

Se trata de una alteración sencilla que consiste en añadir una funcionalidad al prompt, de modo que no se agregue el prompt predeterminado si la variable ya está completada.

El resto del código permanecerá intacto.

A continuación, te presento cómo llevar a cabo este ajuste:

```javascript

    trigger OnOpenPage()
    begin
        if Prompt = '' then
            Prompt := Text001Txt;
    end;


    procedure SetPrompt(NewPrompt: Text)
    begin
        Prompt := NewPrompt;
    end;
```

<br><br>

Ahora, vamos a crear una extensión de página en la lista de clientes.

Añadiremos una nueva acción que nos permitirá acceder a la página de preguntas a ChatGPT. Sin embargo, la diferencia clave aquí es que incluiremos un prompt inicial, el cual generaremos en una función dentro de esta misma extensión de página.

En esta función, recorreremos la tabla de clientes y recopilaremos datos importantes, como el número de cliente, el nombre y las ventas realizadas. Esta información se almacenará en un objeto JSON Array.

Recuerda que cuanto más contextualizada sea la información que proporcionemos, más precisas serán las respuestas de ChatGPT.

El siguiente fragmento de código muestra cómo llevar a cabo este proceso:

```javascript
pageextension 50000 "CustomerList" extends "Customer List"
{
    actions
    {
        addlast(processing)
        {
            action(CustomerQuestions)
            {
                ToolTip = 'Customer Questions', Comment = 'ESP="Preguntas sobre los clientes"';
                Caption = 'Customer Questions', Comment = 'ESP="Preguntas sobre los clientes"';
                ApplicationArea = All;
                Image = Help;

                trigger OnAction()
                var
                    AskChatGPT: Page "Ask ChatGPT";
                begin
                    AskChatGPT.SetPrompt(CreatePromptCustomer());
                    AskChatGPT.RunModal()
                end;
            }
        }

        addlast(Promoted)
        {
            actionref("AskCustomersInfo_Promoted"; CustomerQuestions)
            {
            }
        }
    }

    local procedure CreatePromptCustomer() ReturnValue: Text
    var
        JObject: JsonObject;
        JArray: JsonArray;
        JArrayText: Text;
        PromptTxt: Label 'Please answer the question based on the data set in JsonArray format attached below. For any information requested that is not present in the data set provided, please reply: I could it find an answer. Dataset: %1', Comment = 'ESP="Responda la pregunta según el conjunto de datos en formato JsonArray que adjunto a continuación. Para cualquier información solicitada que no esté presente en el conjunto de datos proporcionado, responda: No puedo encontrar una respuesta. Conjunto de datos: %1"';
    begin
        if Rec.FindSet() then
            repeat
                Rec.CalcFields("Sales (LCY)");

                Clear(JObject);
                JObject.Add(Rec.FieldCaption("No."), Rec."No.");
                JObject.Add(Rec.FieldCaption(Name), Rec.Name);
                JObject.Add(Rec.FieldCaption("Sales (LCY)"), Rec."Sales (LCY)");

                JArray.Add(JObject);

            until Rec.Next() = 0;

        JArray.WriteTo(JArrayText);
        ReturnValue := StrSubstNo(PromptTxt, JArrayText);
    end;
}
```

<br><br><br>

En resumen, hemos llevado la integración entre ChatGPT y Dynamics 365 Business Central a un nivel superior al enriquecer nuestras preguntas con datos de clientes. Este enfoque nos ha permitido obtener respuestas más precisas y contextualizadas. Al proporcionar información detallada y rica en contexto, hemos potenciado la comprensión de ChatGPT y hemos obtenido resultados más útiles. Desde ajustes en la interfaz hasta extensiones para consultas personalizadas, hemos demostrado cómo aprovechar esta sinergia para mejorar la toma de decisiones y la eficiencia operativa.

<br>

Como siempre, podréis ver el ejemplo entero en el [GitHub](https://github.com/Esanpons/chatgpt-in-business-central)

<br>

Si tienes alguna pregunta o inquietud, no dudes en compartirla. ¡Hasta la próxima, y sigamos avanzando e innovando
