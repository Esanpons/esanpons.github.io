---
layout: post
title: ""
summary: ""
author: "Esteve Sanpons"
date: "2021-10-22 08:00:00 +0200"
#cSpell:disable
category: ["JavaScript", "ControlAddin", "Business_Central"]
thumbnail: /assets/img/posts/cambiar-alto-y-ancho-elementos-bc/imagen01.png
permalink: /blog/cambiar-alto-y-ancho-elementos-bc/
custom_type: Blog
#cSpell:enable
---

¡Hola a todos y bienvenidos a otra semana emocionante repleta de programación y consejos para dominar el mundo de Business Central!

¿Alguna vez te has encontrado en la encrucijada de diseñar tu página de Business Central y deseado fervientemente que dos secciones vecinas se acoplen perfectamente, ocupando todo el espacio posible? ¡A mí también me ha pasado y estoy aquí para arrojar luz sobre este desafío!

Visualiza esto: tienes una lista de elementos en la parte izquierda de tu página y algunas estadísticas interesantes en la parte derecha. Tu objetivo es que ambos bloques ocupen cada centímetro disponible y se vean impresionantes. Pero, ¡ay caramba!, Business Central parece tener sus propias ideas sobre tamaños y eso puede desbaratar tus planes.

<input type="checkbox" id="image-checkbox-02" class="image-checkbox">
<label for="image-checkbox-02"  class="image-label">
    <img class="img-container" src="/assets/img/posts/cambiar-alto-y-ancho-elementos-bc/imagen02.png">
</label>
<br><br>

Si lo miras bien, notarás ese espacio en blanco desaprovechado y para colmo, los registros se sienten apretujados en la parte izquierda, ¡y ni hablemos de la molesta barra de desplazamiento! Esto significa que hay más registros que podríamos estar visualizando, ¡pero están ahí, ocultos a plena vista!

Nuestra misión aquí es expandir la sección de la lista en la parte izquierda hasta que abarque todo el espacio disponible en la ventana.

<br>

Pero aquí viene lo emocionante, vamos manos a la obra. 😎

<br>

## El Desafío: Diseñando un Layout Personalizado

Para abordar este reto, vamos a sacar a relucir un poco de JavaScript para ajustar los tamaños de las secciones de acuerdo a nuestros deseos.

Comencemos creando objetos para presentar la información en nuestra página. Imagina que te encuentras en una página de clientes y quieres mostrar una lista de esos clientes junto con algunas estadísticas jugosas. Echa un vistazo a este fragmento de código:

```javascript
page 60001 "ChangeSizeSelector"
{
    PageType = List;
    ApplicationArea = All;
    UsageCategory = Lists;
    SourceTable = Customer;

    layout
    {
        area(Content)
        {
            grid(MyGrid)
            {
                GridLayout = Rows;
                ShowCaption = false;
                repeater(Control1)
                {
                    field(No; Rec."No.")
                    {
                        ApplicationArea = All;
                        ToolTip = 'Specifies the value of the field', comment = 'ESP="Especifica el valor del campo"';
                    }
                    field(Name; Rec.Name)
                    {
                        ApplicationArea = All;
                        ToolTip = 'Specifies the value of the field', comment = 'ESP="Especifica el valor del campo"';
                    }
                }

                group(PagesPart)
                {

                    ShowCaption = false;
                    part(CustomerStatisticsFactBox; "Customer Statistics FactBox")
                    {
                        ApplicationArea = All;
                        SubPageLink = "No." = FIELD("No.");
                        UpdatePropagation = Both;
                    }
                }
            }
        }
    }
}
```

<br><br>

## El Poder del ControlAddin y JavaScript

Ahora, para hacer que el ajuste de tamaño sea dinámico, creamos un ControlAddin en JavaScript. Este ControlAddin nos dará el poder de cambiar el tamaño de los elementos a medida.

Si no estás familiarizado con el término "ControlAddin", ¡no te preocupes! Aquí te presento una guía rápida sobre [cómo crear uno básico.](/boveda/ontroladdin-basico)

Echemos un vistazo al código:

```javascript
controladdin "Change Size Selector"
{

    StartupScript =
        'src/controladdin/ChangeSizeSelector/js/Startup.js';

    Scripts = 'src/controladdin/ChangeSizeSelector/js/Scripts.js';

    RequestedHeight = 1;
    RequestedWidth = 1;
    HorizontalShrink = true;
    HorizontalStretch = true;
    event ControlAddInReady();
    procedure Ejecute(Selector: Text; SubtractHeight: Decimal; SubtractWitdh: Decimal; IsAll: Boolean; IsHeight: Boolean; IsWidth: Boolean);
}

```

<br><br>

## Personalizando el Tamaño: Detalles en Scripts.js

Aquí es donde entra en juego el archivo "Scripts.js", que contendrá dos componentes de JavaScript: el evento de inicio y la función que manejará los cambios de tamaño de nuestros elementos.

Básicamente, estamos pidiendo que se nos entregue el elemento HTML tal como está. Te mostraré cómo lograr esto en un momento.

Luego, si deseamos agregar un margen desde el borde de la pantalla, tanto en altura como en anchura, usamos las variables de resta correspondientes.

Los tres valores booleanos finales determinarán si se modifican todos los elementos con el mismo nombre o solo el primero. También indicarán si se ajustará la altura, la anchura o ambas.

Vamos a crear el archivo "Scripts.js" y agregar las variables globales y la función junto con sus parámetros de entrada.

```javascript
var selector;
var subtractWidth;
var SubtractHeight;
var isAll;
var IsHeight;
var IsWidth;

//Selector: Text; SubtractHeight: Decimal; SubtractWitdh: Decimal; IsAll: Boolean; IsHeight: Boolean; IsWidth: Boolean
function Ejecute(Newselector, NewSubtractHeight, NewSubtractWitdh, NewIsAll, NewIsHeight, NewIsWidth) {
    console.log("Ejecute:");
    selector = Newselector;
    SubtractHeight = NewSubtractHeight;
    subtractWidth = NewSubtractWitdh;
    isAll = NewIsAll;
    IsHeight = NewIsHeight;
    IsWidth = NewIsWidth;
    ChangeSize();
}
```

<br><br>

Aquí inicializamos las variables globales y luego llamamos a la función de cambio.

La función de cambio es simple: si elegimos ajustar todos los elementos, recorremos el array de elementos y aplicamos el cambio a cada uno. Si no, solo seleccionamos el primero y pasamos ese único elemento a la última función, que se encargará tanto de la altura como de la anchura.

```javascript
function ChangeSize() {
    console.log("isAll: ", isAll);

    if (isAll) {
        var elements = window.parent.document.querySelectorAll(selector);
        for (var i = 0; i < elements.length; i++) {
            OneChangeSize(elements[i]);
        }
    } else {
        var Container = window.parent.document.querySelector(selector);
        OneChangeSize(Container);
    }
}

function OneChangeSize(Container) {
    console.log("Before Container: ", Container);
    if (IsHeight) {
        Container.style.removeProperty("max-height");
        Container.style.removeProperty("height");

        var WidthWindows = window.outerHeight - SubtractHeight;
        Container.style.maxHeight = WidthWindows.toString() + "px";
        Container.style.height = WidthWindows.toString() + "px";
        console.log("height: ", Container.offsetHeight);
    }

    if (IsWidth) {
        Container.style.removeProperty("max-width");
        Container.style.removeProperty("width");

        var WidthWindows = window.outerWidth - subtractWidth;
        Container.style.maxWidth = WidthWindows.toString() + "px";
        Container.style.width = WidthWindows.toString() + "px";
        console.log("width: ", Container.offsetWidth);
    }

    console.log("After Container: ", Container);
}
```

<br><br>

He agregado varios console.log para que puedas rastrear los cambios en la consola del navegador. Eliminamos las propiedades existentes para facilitar la adición posterior si es necesario, lo cual en nuestro caso será necesario.

Finalmente, en este archivo, nos vinculamos al evento de cambio de tamaño de la ventana y llamamos nuevamente a la función de cambio de tamaño para que la adaptación sea fluida.

```javascript
window.addEventListener(
    "resize",
    function (event) {
        ChangeSize();
    },
    true
);
```

<br><br>

## Aplicando el Cambio en Tu Página

Ahora es el momento de incorporar esto en nuestra página.

Sin embargo, primero debemos descubrir qué clase tiene el elemento que queremos modificar. Para ello, recurrimos a las herramientas de desarrollo de tu navegador. Nuestra meta es ubicar la clase que contiene el elemento específico.

Ahora, en la página, buscamos y seleccionamos el elemento que nos interesa. Esto es tan sencillo como seguir los pasos que se detallan en este [enlace](/boveda/como-saber-el-id-de-las-partes-de-una-web)

Supongamos que has encontrado la lista de clientes que deseas ajustar. El siguiente paso es identificar el contenedor correcto.

<input type="checkbox" id="image-checkbox-03" class="image-checkbox">
<label for="image-checkbox-03"  class="image-label">
    <img class="img-container" src="/assets/img/posts/cambiar-alto-y-ancho-elementos-bc/imagen03.png">
</label>
<br><br>

Solo deberías ver la lista de clientes en este caso.

Al activar las herramientas de desarrollo, te mostrará una representación del elemento, tal como Business Central lo presenta.

Tu tarea ahora es expandir los niveles hasta llegar al contenedor con la propiedad "max-height" definida. En mi caso, se ve así:

<input type="checkbox" id="image-checkbox-04" class="image-checkbox">
<label for="image-checkbox-04"  class="image-label">
    <img class="img-container" src="/assets/img/posts/cambiar-alto-y-ancho-elementos-bc/imagen04.png">
</label>
<br><br>

Quizás haya que navegar por dos niveles para llegar hasta allí, pero aún tendrás seleccionada toda el área de la lista.

Lo que necesitamos en este caso es el nombre de la clase, que está claramente marcado. Este dato es el que necesitaremos para continuar.

Ahora nos dirigimos a la página y añadimos nuestro ControlAddin de la siguiente manera:

```javascript
//contenedor de la izquierda
usercontrol(ChangeHeightSelector01; "Change Size Selector")
{
    ApplicationArea = All;
    trigger ControlAddInReady()
    var
        SelectoLbl: Label 'div[class="ms-nav-grid-horizontal-container"]', Locked = true;
    begin
        CurrPage.ChangeHeightSelector01.Ejecute(SelectoLbl, 450, 0, false, true, false);
    end;
}

```

<br><br>

Observa cómo hemos añadido la clase al selector y, además, hemos establecido la altura para que sea igual a la altura de la ventana menos 450 píxeles. Esto asegura que no esté pegado al límite de la ventana. El número "450" lo puedes personalizar a tu gusto. También hemos indicado que solo estamos ajustando la altura.

Ahora, si subimos nuestra aplicación y volvemos a ejecutar la ventana, veremos que nuestra lista de clientes se ha expandido para ocupar todo el ancho disponible.

<input type="checkbox" id="image-checkbox-05" class="image-checkbox">
<label for="image-checkbox-05"  class="image-label">
    <img class="img-container" src="/assets/img/posts/cambiar-alto-y-ancho-elementos-bc/imagen05.png">
</label>
<br><br>

Podemos agregar tantos controles como necesitemos. Por ejemplo, he creado un segundo para que la línea divisoria entre los dos elementos llegue hasta el final.

```javascript
//contenedor de la derecha
usercontrol(ChangeHeightSelector02; "Change Size Selector")
{
    ApplicationArea = All;
    trigger ControlAddInReady()
    var
        SelectoLbl: Label 'div[class="ms-nav-layout-lastcolumn"]', Locked = true;
    begin
        CurrPage.ChangeHeightSelector02.Ejecute(SelectoLbl, 450, 0, false, true, false);
    end;
}
```

<br><br>

Y el resultado final sería el siguiente:

<input type="checkbox" id="image-checkbox-06" class="image-checkbox">
<label for="image-checkbox-06"  class="image-label">
    <img class="img-container" src="/assets/img/posts/cambiar-alto-y-ancho-elementos-bc/imagen06.png">
</label>
<br><br>

¡Y ahí lo tienes! Hemos transformado un problema en una solución efectiva utilizando ControlAddins y JavaScript. Ahora puedes maximizar el espacio en tus páginas de Business Central. Recuerda, este es solo un ejemplo y puedes adaptarlo y ampliarlo según tus propias necesidades.

<br>

Como siempre, puedes encontrar el ejemplo completo en [GitHub](https://github.com/Esanpons/ControlAddIns-Business-Central)

<br>

Recuerda que esta es solo una muestra y que puedes personalizarla y expandirla para que se adapte a tus necesidades específicas. ¡Mucha suerte con tus proyectos y explora nuevas oportunidades! ¡No dudes en compartir tus comentarios y preguntas! ¡Espero que esta guía te haya resultado útil y emocionante. ¡Hasta la próxima!
