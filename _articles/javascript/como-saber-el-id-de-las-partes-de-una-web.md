---
title: "Utilizar las herramientas de desarrollador para encontrar el ID o la clase de un elemento en una página web"
summary: 'Cómo utilizar las herramientas de desarrollador para encontrar el ID o la clase de un elemento en una página web, proporcionando pasos detallados y destacando la funcionalidad "Select an element in the page to inspect"'
layout: article
author: Esteve Sanpons
#cSpell:disable
category: [JavaScript]
custom_type: Boveda
permalink: /boveda/como-saber-el-id-de-las-partes-de-una-web
#cSpell:enable
date: 2023-06-07 21:00:00 +0200
---

## Introducción:

En el mundo del desarrollo web, es común encontrarse con la necesidad de identificar elementos específicos en una página para realizar modificaciones o interactuar con ellos a través de JavaScript. Afortunadamente, los exploradores modernos ofrecen herramientas de desarrollador que hacen que esta tarea sea más fácil. En este artículo, te guiaré paso a paso a través del proceso de cómo encontrar el ID o la clase de un elemento utilizando estas herramientas.

<br><br>

## ¿Qué son las herramientas de desarrollador?

Antes de sumergirnos en los pasos, es importante comprender qué son las herramientas de desarrollador. Estas son conjuntos de utilidades integradas en los exploradores web modernos que permiten a los desarrolladores examinar, depurar y modificar el código de una página web en tiempo real. Con estas herramientas, puedes inspeccionar y manipular diferentes aspectos de una página, incluidos los elementos y sus atributos.

<br><br>

#### Paso 1: Abrir las herramientas de desarrollador

El primer paso es abrir las herramientas de desarrollador en tu explorador web. La forma de hacerlo puede variar ligeramente según el explorador que utilices. A continuación, te mostraré cómo abrir las herramientas de desarrollador en Google Chrome, uno de los exploradores más populares:

Haz clic derecho en cualquier parte de la página que estás visualizando.
En el menú desplegable, selecciona la opción "Inspeccionar" o "Inspeccionar elemento".
También puedes utilizar el atajo de teclado Ctrl+Shift+I (Windows/Linux) o Cmd+Option+I (Mac) para abrir las herramientas de desarrollador.

<input type="checkbox" id="image-checkbox-01" class="image-checkbox">
<label for="image-checkbox-01"  class="image-label">
    <img class="img-container" src="/assets/img/articles/como-saber-el-id-de-las-partes-de-una-web/imagen01.png">
</label>

<br><br>

#### Paso 2: Explorar la estructura de la página

Una vez que hayas abierto las herramientas de desarrollador, se mostrará una ventana o panel en tu explorador que contiene varias pestañas y herramientas relacionadas. Esta ventana suele estar dividida en dos partes: una muestra el código fuente de la página y la otra muestra la vista en vivo de la página.

<br><br>

#### Paso 3: Localizar el elemento deseado

Ahora que tienes las herramientas de desarrollador abiertas, es hora de localizar el elemento de la página web del que deseas obtener el ID o la clase. Puedes utilizar el cursor o la función de selección de elementos en las herramientas de desarrollador para resaltar el elemento en la vista en vivo. A medida que te muevas sobre los elementos en la página, verás cómo se resalta el código correspondiente en la ventana de herramientas de desarrollador.

<br><br>

#### Paso 4: La funcionalidad "Select an element in the page to inspect"

Dentro de las herramientas de desarrollador de los exploradores modernos, existe una funcionalidad especialmente útil llamada "Select an element in the page to inspect". Esta función permite seleccionar directamente un elemento en la página web y mostrar su código correspondiente en las herramientas de desarrollador. Veamos cómo utilizar esta función:

Con las herramientas de desarrollador abiertas, busca el botón o icono que representa la función "Select an element in the page to inspect". En Google Chrome, por ejemplo, este botón suele tener el ícono de un cursor junto a un cuadro de selección.

Haz clic en el botón "Select an element in the page to inspect". Una vez activado, el cursor del ratón cambiará de forma, indicando que estás en modo de selección.

<input type="checkbox" id="image-checkbox-02" class="image-checkbox">
<label for="image-checkbox-02"  class="image-label">
    <img class="img-container" src="/assets/img/articles/como-saber-el-id-de-las-partes-de-una-web/imagen02.png">
</label>

<br>

Lleva el cursor sobre el elemento que deseas inspeccionar. A medida que te muevas sobre los elementos de la página, verás cómo se resaltan a medida que pasas por encima de ellos. Esto te brinda una vista previa visual de los elementos a medida que los seleccionas.

Haz clic en el elemento que deseas inspeccionar. Al hacerlo, las herramientas de desarrollador mostrarán automáticamente el código correspondiente a ese elemento. Podrás ver su estructura, atributos y otros detalles en la ventana de herramientas de desarrollador.

<input type="checkbox" id="image-checkbox-03" class="image-checkbox">
<label for="image-checkbox-03"  class="image-label">
    <img class="img-container" src="/assets/img/articles/como-saber-el-id-de-las-partes-de-una-web/imagen03.png">
</label>

<br>

La función "Select an element in the page to inspect" es extremadamente útil cuando deseas identificar rápidamente un elemento específico en una página y examinar su código sin tener que navegar manualmente por la estructura de la página. Esta función te ahorra tiempo y te permite obtener una visión más detallada del elemento seleccionado.

Una vez que hayas utilizado esta funcionalidad para seleccionar un elemento, podrás continuar con los pasos previos mencionados para encontrar su ID o clase en el código y utilizar esa información según tus necesidades de desarrollo.

Recuerda que la función "Select an element in the page to inspect" puede variar en su ubicación o diseño dependiendo del explorador que estés utilizando, pero su funcionalidad es similar en todos los casos. Esta herramienta te brinda una manera rápida y eficiente de seleccionar elementos y obtener información detallada sobre ellos.

<br><br>

#### Paso 5: Encontrar el ID o la clase del elemento

Una vez que hayas localizado el elemento deseado, es momento de encontrar su ID o clase en el código. En la ventana de herramientas de desarrollador, busca el código resaltado que representa el elemento seleccionado. Puedes buscar el atributo "id" o "class" para identificar el ID o la clase del elemento, respectivamente. Por ejemplo, puedes encontrar algo como id="mi-elemento" o class="mi-clase" en el código resaltado.

<br><br>

#### Paso 6: Utilizar el ID o la clase obtenidos

Una vez que hayas encontrado el ID o la clase del elemento, puedes utilizar esta información para diversas finalidades. Por ejemplo, puedes utilizar el ID o la clase para modificar el estilo del elemento mediante CSS. También puedes acceder al elemento a través de JavaScript utilizando el ID o la clase para realizar manipulaciones adicionales, como agregar o eliminar contenido.

<br><br>

## Conclusiones

Las herramientas de desarrollador son una herramienta valiosa para los desarrolladores web, ya que facilitan la tarea de encontrar el ID o la clase de un elemento en una página web. Siguiendo los pasos descritos en este artículo, puedes inspeccionar y obtener los atributos necesarios para interactuar con elementos específicos de una manera más eficiente.
