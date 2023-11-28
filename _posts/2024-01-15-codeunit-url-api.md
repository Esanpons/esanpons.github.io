---
layout: post
title: "Simplificando la Programación en Business Central: Un Viaje a Través de Funciones Personalizadas para la Gestión de URLs"
summary: "Adéntrate en el universo de funciones personalizadas, desde la creación de la base de la URL hasta la extracción de consultas dinámicas. Este artículo te guiará a través de un viaje emocionante, liberándote de la monotonía y brindándote el control total sobre tus peticiones API."
author: "Esteve Sanpons"
date: "2024-01-14 04:00:00 +0200"
#cSpell:disable
category: ["Business_Central", "API"]
thumbnail: /assets/img/posts/codeunit-url-api/imagen01.png
permalink: /blog/codeunit-url-api/
custom_type: Blog
#cSpell:enable
---

Hola a todos, en esta nueva entrega de la serie de objetos API de Business Central vamos a hablar sobre cómo crear funciones para facilitar la gestión de URLs desde el código.

Hoy veremos como crear en Business Central unas funciones para facilitar el manejo de la URL y sus infinitas opciones des del código.

Con estas funciones podremos evitar tener que escribir a mano las infinitas opciones que ofrecen las URLs, lo que agilizará nuestro trabajo y nos permitirá centrarnos en lo que realmente importa.

<br>

Por fin hoy nos pondremos a desarrollar en Business Central. Así que, ¡Vamos manos a la obra! 😍

<br>

## Base URL

Primero, creamos una nueva Codeunit con una función para inicializar la base de la URL. Esta función guarda en una variable de tipo texto el resultado y la variable que hemos creado se llama "BaseURL". Le pedimos a la función el nombre del servicio del objeto API, que se corresponde con el EntitySetName.

Esto guardara en una variable de tipo texto el resultado. Esta variable en el ejemplo que estamos creado es la variable “BaseURL”

Como vemos le pedimos a la función el EntitySetName que seria el nombre del servicio del objeto tipo API.

```javascript
codeunit 51102 "MyCreateUrl"
{
    procedure InitBaseURLPageAPI(EntitySetName: Text)
    var
        URLBaseLbl: Label 'http://bclast:7048/BC/api/', Locked = true;
        APIPublisherLbl: Label 'mycompany/', Locked = true;
        APIGroupLbl: Label 'sales/', Locked = true;
        VersionLbl: Label 'v2.0/', Locked = true;
        URLCompanieLbl: Label 'companies(33e38fba-8f98-ec11-bb87-000d3a2690e9)/', Locked = true;
    begin
        BaseURL := URLBaseLbl + APIPublisherLbl + APIGroupLbl + VersionLbl + URLCompanieLbl + EntitySetName;
    end;

}
```

<br>

## Llamar a funciones en objetos API

También hemos creado una función para llamar a las funciones externas de los objetos API. Como ya hemos visto, se pueden llamar a las funciones de los objetos API añadiéndolos en la URL. Con esta función solo tendremos que indicarle el nombre de la función y lo añadirá automáticamente a la URL.

```javascript
    procedure SetUrlFuntionPageAPI(NameFunction: Text)
    var
        FunctionLbl: Label '/Microsoft.NAV.', Locked = true;
    begin
        NameFunction := FunctionLbl + NameFunction;
        UrlFuntionPageAPI := NameFunction;
    end;
```

<br>

Tal como vimos anteriormente se puede llamar a las funciones de los objetos de tipo API añadiéndolos en la URL.

Con esta función solo tendremos que notificarle el nombre de la función y lo añadirá a la URL.

<br>

## Filtro de la Key del objeto tipo API

Otra función que hemos creado es para añadir el valor de la clave en la URL de petición. Esto es útil para añadir el dato del "ODataKeyFields" que hemos visto en las URLs.

```javascript
    procedure SetUrlFilterKeyPageAPI(ValueEquals: Variant)
    var
        char39: Char;
        TxtValueEquals: Text;
    begin
        char39 := 39;
        TxtValueEquals := Format(ValueEquals);

        case true of
            ValueEquals.IsText:
                TxtValueEquals := char39 + TxtValueEquals + char39;
            ValueEquals.IsDate:
                TxtValueEquals := Format(ValueEquals, 0, '<Year4>-<Month,2>-<Day,2>');
        end;

        TxtValueEquals := StrSubstNo('(%1)', TxtValueEquals);
        UrlFilterKeyPageAPI := TxtValueEquals;
    end;
```

<br>

Pues en esta función lo que estamos haciendo es, precisamente eso, añadir el valor de la clave en la URL de petición.

<br>

## Mostrar un máximo de registros

Para mostrar un número máximo de registros por cada llamada, hemos creado una función que añade el máximo que le digamos a una variable que después añadiremos a la URL.

```javascript
    procedure SetUrlTop(Top: Integer)
    var
        FilterLbl: Label '$top=', Locked = true;
    begin
        InitConsultationOptionsURL(UrlTop, FilterLbl, MyLogicalOperators::" ");
        UrlTop += Format(Top);
    end;
```

<br>

Para esto hemos creado esta función que añadirá el máximo que le digamos a una variable que después añadiremos a la URL.

<br>

## Inicio Operadores de la URL

A partir de este momento necesitamos una función para determinar si es el primer parámetro de o se han añadido otros. Para ello hemos creado una función que añadiremos antes de cualquier llamada de las siguientes funciones. Esta función añade el operador lógico correspondiente en la URL según corresponda.

```javascript
    local procedure InitConsultationOptionsURL(var TextFilterUrl: Text; FilterTxt: Text; MyEnumUrlFilter: Enum MyLogicalOperators)
    var
        Pos: Integer;
        AndLbl: Label ' and ', Locked = true;
        OrLbl: Label ' or ', Locked = true;
        "?Lbl": Label '?', Locked = true;
        "&Lbl": Label '&', Locked = true;
        PosicionInicialLbl: Label '?$', Locked = true;
    begin
        RepeatControl(TextFilterUrl, FilterTxt, MyEnumUrlFilter);

        if IsTheQuestionMarkThere then
            TextFilterUrl += "&Lbl"
        else begin
            TextFilterUrl += "?Lbl";
            IsTheQuestionMarkThere := true;
        end;

        Pos := StrPos(TextFilterUrl, FilterTxt);
        if Pos = 0 then
            TextFilterUrl += FilterTxt
        else begin
            TextFilterUrl := PadStr(TextFilterUrl, StrLen(TextFilterUrl) - 1);
            TextFilterUrl += format(MyEnumUrlFilter);
        end;
    end;
```

<br>

Como vemos lo primero que pondrá será o el “?” o el “&” si es el primero operador pondrá el “?” y si no pondrá el “&”.

Después verificara si ese operador ya esta en la URL si estuviera se puede configurar para que añada un “and” o un “or” para ese operador.

<br>

## Operador Lógico

Además, hemos creado un enum para determinar si los operadores dentro del mismo tipo serán con "And" o con "or".

```javascript
enum 51100 "MyLogicalOperators"
{
    Extensible = true;

    value(1; " ") { }
    value(2; " and ") { }
    value(3; " or ") { }
}
```

<br>

## Seleccionar campos en concreto

Otra función que hemos creado es para seleccionar campos específicos dentro de una petición de WebService. Esto nos permite ver solo ciertos campos de los que nos ofrecen

```javascript
    procedure SetUrlSelectFields(NameFields: Text)
    var
        FilterLbl: Label '$select=', Locked = true;
    begin
        RepeatControl(UrlSelectFields, FilterLbl, MyLogicalOperators::" ");

        InitConsultationOptionsURL(UrlSelectFields, FilterLbl, MyLogicalOperators::" ");
        UrlSelectFields += NameFields;
    end;
```

<br>

Esta función añade a la URL los campos que se quieran mostrar.

<br>

## Mostrar subpágina

También hemos creado una función para mostrar subpáginas. En algunos casos, como en los pedidos, los registros tienen una cabecera y una lista de líneas, por lo que las líneas están dentro de la página de cabecera. El resultado lo añadimos a una variable global para ser insertada en la URL al final.

```javascript
    procedure SetUrlSubPage(NameSubPage: Text)
    var
        FilterLbl: Label '$expand=', Locked = true;
    begin
        RepeatControl(UrlSubPage, FilterLbl, MyLogicalOperators::" ");

        InitConsultationOptionsURL(UrlSubPage, FilterLbl, MyLogicalOperators::" ");
        UrlSubPage += NameSubPage;
    end;
```

<br>

## Filtros en subpágina

En las subpáginas también se pueden hacer filtros. En esta función, hemos creado una codeunit como variable externa para que solo nos devuelva los filtros para poder ser añadidos a la URL final.

```javascript
    procedure SetUrlSubPage_Filter(NameSubPage: Text; TagField: Text; ValueEquals: Variant; MyLogicalOperators: Enum MyLogicalOperators; MyUrlFilter: Enum MyUrlFilter)
    var
        TxtFilter: Text;
        MyCreateUrl: Codeunit MyCreateUrl;
        WhichLbl: Label '?', Locked = true;
        WhereLbl: Label '=', Locked = true;
    begin
        Clear(MyCreateUrl);
        MyCreateUrl.SetUrlFilter(TagField, ValueEquals, MyLogicalOperators, MyURLFilter);
        UrlSubPage_Filter += MyCreateUrl.GetURL();
        UrlSubPage_Filter := DelChr(UrlSubPage_Filter, WhereLbl, WhichLbl);
    end;
```

<br>

En esta función los filtros hemos llamado a esta codeunit como una variable externa para que así solo nos devuelva los filtros para poder ser añadidos a la URL final.

<br>

## Filtrar valores en campos

Se ha creado una función para filtrar valores en campos concretos. Lo primero que hacemos es formatear el tipo de valor ya que para muchos endpoint debe tener una formato en concreto y para los objetos de tipo API los formatos tienen que ser específicos. Después inicializamos el filtro y añadimos el valor a la variable.

```javascript
    procedure SetUrlFilter(TagField: Text; ValueEquals: Variant; MyLogicalOperators: Enum MyLogicalOperators; MyUrlFilter: Enum MyUrlFilter)
    var
        FilterLbl: Label '$filter=', Locked = true;
        ParenthesisLbl: Label '(%1,%2)', Locked = true;
        Pos: Integer;
        char39: Char;
        TxtValueEquals: Text;
    begin
        char39 := 39;
        TxtValueEquals := Format(ValueEquals);

        case true of
            ValueEquals.IsText:
                TxtValueEquals := char39 + TxtValueEquals + char39;
            ValueEquals.IsDate:
                TxtValueEquals := Format(ValueEquals, 0, '<Year4>-<Month,2>-<Day,2>');
        end;

        InitConsultationOptionsURL(UrlFilter, FilterLbl, MyLogicalOperators);
        case MyUrlFilter of
            MyUrlFilter::contains, MyUrlFilter::startswith, MyUrlFilter::endswith:
                UrlFilter += Format(MyUrlFilter) + StrSubstNo(ParenthesisLbl, TagField, TxtValueEquals);
            else
                UrlFilter += TagField + Format(MyUrlFilter) + TxtValueEquals;
        end;

    end;
```

<br>

Lo primero que hacemos es formatear el tipo de valor ya que para muchos endpoint debe tener una formato en concreto y para los objetos de tipo API los formatos tienes que ser específicos.

Después inicializamos el filtro y por último añadimos el valor a la variable.

Como esta función se puede utilizar para muchos tipos de filtros hemos creado un enum para determinar la gran mayoría de ellos.

```javascript
enum 51101 "MyUrlFilter"
{
    value(1; " ") { }
    //igual que
    value(2; " eq ") { }
    //menor que
    value(3; " lt ") { }
    //mayor que
    value(4; " gt ") { }
    //no es igual que
    value(5; " ne ") { }
    //inferior a o igual que
    value(6; " le ") { }
    //superior a o igual que
    value(7; " ge ") { }
    //Contiene el valor que
    value(8; "contains") { }
    //Empieza con el valor que
    value(9; "startswith") { }
    //Finaliza por el valor que
    value(10; "endswith") { }
}
```

<br>

Lo único que tiene el enum es el tipo de filtro con los espacios para que así podamos ponerlo directamente en la URL

<br>

## Extraer URL

Por último, creamos la función para extraer la composición de la URL que hemos ido creando.

```javascript
   procedure GetURL(): Text
    var
        Pos: Integer;
        TxtLastUrl: Text;
        PosicionInicialLbl: Label '?$', Locked = true;
        SubPageFilterLbl: Label '(%1)', Locked = true;
    begin
        if UrlSubPage_Filter <> '' then begin
            UrlSubPage += StrSubstNo(SubPageFilterLbl, UrlSubPage_Filter);
        end;

        case true of
            StrPos(UrlSubPage, PosicionInicialLbl) <> 0:
                TxtLastUrl := UrlSubPage + UrlTop + UrlSelectFields + UrlFilter;
            StrPos(UrlTop, PosicionInicialLbl) <> 0:
                TxtLastUrl := UrlTop + UrlSubPage + UrlSelectFields + UrlFilter;
            StrPos(UrlSelectFields, PosicionInicialLbl) <> 0:
                TxtLastUrl := UrlSelectFields + UrlSubPage + UrlTop + UrlFilter;
            StrPos(UrlFilter, PosicionInicialLbl) <> 0:
                TxtLastUrl := UrlFilter + UrlSubPage + UrlTop + UrlSelectFields;
        end;

        exit(BaseURL + UrlFilterKeyPageAPI + UrlFuntionPageAPI + TxtLastUrl);
    end;
```

<br>

Como debe tener un orden en concreto lo primero que buscamos es saber cuál será el que tenga el “?” y ese lo ponemos primero en los operadores del final de la URL.

Después a la hora de extraer esa URL la extraemos en el orden que mejor no ira, en este caso primero la Base después la calve del registro, después si hay funciones del objeto API y por ultimo los operadores que hemos añadido.

<br>

## Conclusión

En el emocionante viaje de programar en Business Central, cada línea de código cuenta. Estas funciones personalizadas no solo simplifican tus tareas diarias, sino que también liberan tu creatividad para enfocarte en lo que realmente importa: construir soluciones increíbles.

<br>

Como de costumbre, el ejemplo completo está disponible en [GitHub](https://github.com/Esanpons/Connection-From-BC).

<br>

## Más Allá del Código

Ahora, dejemos por un momento las líneas de código y exploremos las posibilidades. ¿Sabías que estas funciones no solo ahorran tiempo, sino que también te permiten construir consultas más dinámicas y poderosas?

Imagina poder personalizar fácilmente las URL para adaptarse a diferentes escenarios. Desde la selección de campos específicos hasta la aplicación de filtros personalizados, estas funciones te ofrecen un control sin igual sobre tus peticiones API.
