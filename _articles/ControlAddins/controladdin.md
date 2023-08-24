---
title: "Dominando los ControlAddIns: Potencia tus desarrollos en Business Central"
summary: "Explora el emocionante mundo de los ControlAddIns en Business Central y descubre cómo elevar tus aplicaciones con JavaScript, CSS y HTML."
layout: article
#cSpell:disable
category: [JavaScript, "ControlAddin", "Business_Central"]
custom_type: Boveda
permalink: /boveda/controladdin
#cSpell:enable
author: Esteve Sanpons
date: 2023-05-01 02:00:00 +0200
---

¡Saludos a todos los apasionados del desarrollo y la programación!

Si eres un entusiasta de Business Central, estás a punto de descubrir un mundo de posibilidades emocionantes. En este artículo, nos sumergiremos en la fascinante creación de "ControlAddIns" y cómo pueden convertir tus desarrollos en auténticas maravillas visuales y funcionales.

<br><br>

# ControlAddIns: Abriendo Nuevas Fronteras en Business Central

Antes de adentrarnos en los detalles, es crucial entender qué es exactamente un "ControlAddIn" en el contexto de Microsoft Business Central. Un ControlAddIn es una herramienta poderosa que te permite integrar componentes web en tus aplicaciones Business Central.

¿Qué significa eso? Puedes combinar la potencia de JavaScript, CSS y HTML para construir experiencias personalizadas y enriquecedoras directamente dentro de tu entorno de desarrollo.

Esta integración es como un lienzo en blanco que te permite fusionar lo mejor del desarrollo web con la funcionalidad empresarial de Business Central. Imagina la capacidad de crear interfaces de usuario modernas y dinámicas, incorporar visualizaciones de datos interactivas, todo desde una única plataforma.

Si deseas explorar más sobre ControlAddIns y cómo revolucionan el desarrollo en Business Central, te invito a visitar [la documentación oficial de Microsoft](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/developer/devenv-control-addin-object) para obtener una visión completa de sus posibilidades y beneficios.

<br><br>

# ¡Hola, Mundo!: Creando tu Primer ControlAddIn

Ahora que entendemos la potencia de los ControlAddIns, ¡vamos a crear uno desde cero! Para este ejemplo, vamos a sumergirnos en algo tan fundamental como el icónico "¡Hola, Mundo!".

```javascript
controladdin "HelloWorldAddIn"
{
    RequestedHeight = 300;
    MinimumHeight = 300;
    MaximumHeight = 300;
    RequestedWidth = 700;
    MinimumWidth = 700;
    MaximumWidth = 700;
    VerticalStretch = true;
    VerticalShrink = true;
    HorizontalStretch = true;
    HorizontalShrink = true;
    StartupScript = 'src\controladdin\HelloWorldAddIn\js\InitScript.js';
    StyleSheets = 'src\controladdin\HelloWorldAddIn\css\StyleSheets.css';
    Scripts = 'src\controladdin\HelloWorldAddIn\js\Scripts.js';

    event CallBack();
    procedure HelloWorld(text: Text);
}
```

<br><br>

Un vistazo rápido al código y verás cómo configuramos la apariencia y el comportamiento de nuestro ControlAddIn. Las propiedades como RequestedHeight y RequestedWidth dan forma a su tamaño, mientras que StartupScript y StyleSheets indican los archivos de inicio y estilos. Además, eventos como CallBack() nos permiten saber cuando se inicia el ControlAddin.

Pero espera, hay algo nuevo aquí... ¿Sabías que incluso puedes crear controles personalizados y conectarlos con las funcionalidades de JavaScript? Esta es una forma poderosa de ampliar la interacción con los usuarios.

Demos vida a nuestros scripts. El script de inicio se ejecutará al cargar el control:

```javascript
Microsoft.Dynamics.NAV.InvokeExtensibilityMethod("CallBack", "");
```

<br><br>

Y el mensaje "¡Hola, Mundo!" ahora se transforma en una función interactiva:

```javascript
function HelloWorld(text) {
    alert("Hello world en " + text);
}
```

<br><br>

Con todo esto dicho, ahora solo nos falta llevar todo esto a una página para ver cómo se ejecuta nuestro mensaje.

En el diseño, añadiremos un "usercontrol" que enlace con nuestro ControlAddin.

Vamos a llamar al evento "CallBack" y, aquí, invocaremos la función "HelloWorld" que creamos en JavaScript.

Para rematar, como prueba de que incluso desde un botón podemos acceder al código de nuestro ControlAddin, crearemos un botón y le asociaremos la función

```javascript
page 60061 "TestPageAddIn"
{
    Caption = 'Test Page AddIn';

    PageType = List;
    Editable = false;
    UsageCategory = Lists;
    ApplicationArea = All;
    layout
    {
        area(Content)
        {
            usercontrol(HelloWorldAddIn; HelloWorldAddIn)
            {
                ApplicationArea = All;

                trigger Callback()
                begin
                    CurrPage.HelloWorldAddIn.HelloWorld('El Inicio');
                end;
            }
        }
    }

    actions
    {
        area(Creation)
        {
            action(LlamarFuncion)
            {
                ApplicationArea = All;
                Image = "1099Form";
                ToolTip = ' ';

                trigger OnAction();
                begin
                    CurrPage.HelloWorldAddIn.HelloWorld('Action');
                end;
            }
        }
    }
}
```

<br><br>

Ya estamos. Compilamos todo y damos rienda suelta a la ejecución. Si mostramos la página que hemos configurado, veremos nuestro primer mensaje. Y si lo lanzamos desde el botón, ¡aparecerá el segundo mensaje!

<br><br>

# De "¡Hola, Mundo!" a "¡Hola, Universo!"

Ahora que dominamos los conceptos básicos, es hora de elevarlo a un nivel superior. Vamos a añadir más detalles y características a nuestro ejemplo.

Comencemos por personalizar el archivo “InitScript.js”, incorporamos la función "init()":

```javascript
init();
Microsoft.Dynamics.NAV.InvokeExtensibilityMethod("CallBack", "");
```

<br><br>

Y en “Scripts.js”, creamos "init()", que agrega elementos HTML.

En esta sección, examinamos de cerca un fragmento de código que desempeña un papel clave en la creación de elementos visuales en Business Central. Explicamos cómo este código interactúa con elementos HTML y cómo se utiliza para personalizar la apariencia de tu aplicación. Aquí está el desglose del código:

-   Se establece una conexión con un elemento del ControlAddin utilizando su atributo id.
-   Se utiliza la propiedad innerHTML para modificar el contenido HTML del elemento seleccionado.
-   Se añaden elementos <div> y <span> para crear una estructura visual. Los elementos anidados permiten crear contenedores y segmentos de texto.
-   Se aplican clases CSS, como "contenedor", "orange" y "ref", para dar estilo a los elementos y controlar su apariencia.
-   Se proporciona una descripción detallada de cada paso del código y su función.

```javascript
function init() {
    var div = document.getElementById("controlAddIn");

    div.innerHTML += '<div class="contenedor">' + '    <div id="IDcontenedor"></div>' + "</div>";

    div.innerHTML +=
        '<span class="orange">' + "Esta es una línea, y a continuación insertamos un salto de línea.<br/>" + "</span>";

    div.innerHTML += '<span class="ref">Un texto donde necesito alguna cosa</span>';
}
```

<br><br>

El ultimo archivo que tocaremos hoy es el de los estilos

```css
.contenedor {
    float: left;
    width: 40px;
    height: 40px;
    background-color: blue;
}

.ref {
    font-weight: negrita;
    color: navy;
    content: "Reference: ";
}
.orange {
    width: 33.333%;
    vertical-align: middle;
    text-align: center;
    color: rgb(234, 109, 0);
}
```

<br><br>

En este archivo añadiremos las configuraciones y los estilos de cada uno de los controles que antes hemos creado.

Como podéis ver se llaman igual que los que hemos añadido en el archivo “Scripts.js” dentro del código de HTML.

El resultado es digno de ver.

Una caja azul con dos textos uno naranja y otro azul.

<br><br>

# Explorando Más Allá: Tu Trayecto de Desarrollo Continúa

Si has llegado hasta aquí, ¡felicidades! Has recorrido un camino fascinante en el desarrollo en Business Central. Pero recuerda, esto es solo el comienzo.

-   ¿Sabías que puedes incrustar gráficos interactivos utilizando bibliotecas como D3.js?
-   ¿Qué tal integrar APIs externas para enriquecer tus aplicaciones con datos en tiempo real?
-   ¿Has explorado las posibilidades de diseño responsivo para adaptar tus creaciones a diferentes dispositivos?

La programación en Business Central es un universo de oportunidades. ¡Aprovecha al máximo cada herramienta y técnica que encuentres!

<br>

Y si deseas profundizar aún más, te invito a explorar el ejemplo completo en mi [GitHub](https://github.com/Esanpons/ControlAddIns-Business-Central). Allí encontrarás más ejemplos, trucos y descubrimientos que te inspirarán en tu viaje.
