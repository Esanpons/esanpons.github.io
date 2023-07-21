---
title: ""
summary: ""
layout: article
#cSpell:disable
category: ["Business_Central", Reports]
custom_type: Boveda
permalink: /boveda/dataset-de-reports-en-bc
#cSpell:enable
author: Esteve Sanpons
date: 2023-07-21 02:00:00 +0200
LinkedIn: false
---

¡Descubre una utilidad imprescindible para los usuarios que provienen de versiones antiguas de Business Central y que ha sido una gran ausencia durante muchos años!

Estamos hablando del Zoom del Dataset de un informe, una funcionalidad que en versiones anteriores a Business Central se utilizaba para investigar problemas o irregularidades en los informes. Cuando un informe arrojaba resultados extraños o datos incorrectos, los usuarios tenían la opción de ejecutar el informe y visualizar el dataset completo en forma de tabla, lo que permitía identificar rápidamente cualquier inconveniente.

Sin embargo, cuando la transición a Business Central se produjo y las ejecuciones de informes pasaron a ser vía web, esta útil función quedó en el olvido, ya que resultaba complicado implementarla en este nuevo entorno.

¡Pero espera, hay buenas noticias! A partir de la versión 18.3 de Business Central, la funcionalidad del Zoom del Dataset ha vuelto a la vida, aunque con una nueva y refrescante forma de operar. Te invito a explorar la documentación oficial de Microsoft para conocer más detalles [enlace](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/whatsnew/whatsnew-update-18-3)

Antes de sumergirnos en cómo realizar esta acción en Business Central, permitirme explicar brevemente cómo solíamos hacerlo en Navision, para aquellos que no estén familiarizados con el proceso.

En Navision, ejecutábamos un informe, como, por ejemplo, el de la factura de venta, y en la vista previa, nos dirigíamos a la esquina izquierda y hacíamos clic en el botón siguiente:

<input type="checkbox" id="image-checkbox-02" class="image-checkbox">
<label for="image-checkbox-02"  class="image-label">
    <img class="img-container" src="/assets/img/articles/dataset-de-reports-en-bc/imagen02.png">
</label>

<br><br>

Al hacer esto, se abría una ventana emergente con un mensaje informativo:

<input type="checkbox" id="image-checkbox-03" class="image-checkbox">
<label for="image-checkbox-03"  class="image-label">
    <img class="img-container" src="/assets/img/articles/dataset-de-reports-en-bc/imagen03.png">
</label>

<br><br>

Este mensaje nos indicaba que debíamos volver a ejecutar el informe para activar la función de visualización del dataset. Por lo tanto, al ejecutar nuevamente el informe, obtendríamos el siguiente resultado:

<input type="checkbox" id="image-checkbox-04" class="image-checkbox">
<label for="image-checkbox-04"  class="image-label">
    <img class="img-container" src="/assets/img/articles/dataset-de-reports-en-bc/imagen04.png">
</label>

<br><br>

Como puedes observar, ahora se muestran una gran cantidad de datos, que corresponden al dataset enviado al layout del informe. De esta manera, si hubiera algún error en los datos o problemas de posicionamiento, podríamos identificarlos al instante.

Pasemos ahora a las versiones más recientes de Business Central, donde encontramos esta funcionalidad, pero con un enfoque diferente y más práctico para los usuarios.

Antes de ejecutar el informe, simplemente seleccionamos la opción "Imprimir" y, a continuación, hacemos clic en el botón "Enviar a":

<input type="checkbox" id="image-checkbox-05" class="image-checkbox">
<label for="image-checkbox-05"  class="image-label">
    <img class="img-container" src="/assets/img/articles/dataset-de-reports-en-bc/imagen05.png">
</label>

<br><br>

A continuación, se mostrará un listado de opciones, y seleccionamos la opción marcada a continuación:

<input type="checkbox" id="image-checkbox-06" class="image-checkbox">
<label for="image-checkbox-06"  class="image-label">
    <img class="img-container" src="/assets/img/articles/dataset-de-reports-en-bc/imagen06.png">
</label>

<br><br>

Listo! Ahora obtendremos un archivo de Excel con todo el dataset del informe. Esto nos permite visualizar y analizar los datos de una manera más cómoda y versátil.

Gracias a esta mejora en versiones posteriores de Business Central, recuperamos una funcionalidad valiosa que habíamos extrañado desde tiempos de Navision.
