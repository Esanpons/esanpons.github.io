---
layout: post
title: "Cómo registrar una aplicación y otorgar permisos en Azure"
summary: "En este blog, aprenderemos cómo registrar una aplicación en Azure Active Directory y otorgarle los permisos necesarios. Seguiremos los pasos para crear una aplicación registrada, obtener los ID necesarios, generar un secreto y configurar los permisos en Azure. ¡Descubre cómo hacerlo paso a paso!"
author: "Esteve Sanpons"
date: "2023-07-01 04:00:00 +0200"
#cSpell:disable
category: ["API_Graph", "Azure"]
thumbnail: /assets/img/posts/registrar-app-y-dar-permisos-en-azure/imagen01.png
permalink: /blog/registrar-app-y-dar-permisos-en-azure/
custom_type: Blog
#cSpell:enable
---

¡Buenas una semana más!

Esta semana continuaremos con el tema de la API Graph. Hoy veremos cómo crear una aplicación registrada y otorgarle los permisos necesarios para ejecutar Postman y realizar pruebas.

En la semana anterior, vimos cómo obtener las funciones necesarias para la API Graph en nuestro Postman. Puedes encontrarlo en el [Link](/blog/api-graph-en-postman/)

<br>

Sin más dilación, vamos al grano. 😄

<br>

Accedemos al [Link](https://portal.azure.com/#home) Si se nos solicita iniciar sesión, utilizamos nuestras credenciales de Microsoft y buscamos "Azure Active Directory".

<input type="checkbox" id="image-checkbox-02" class="image-checkbox">
<label for="image-checkbox-02"  class="image-label">
    <img class="img-container" src="/assets/img/posts/registrar-app-y-dar-permisos-en-azure/imagen02.png">
</label>

<br><br>

Una vez aquí, buscamos el registro de aplicaciones y hacemos clic en "Nuevo registro" en un botón ubicado arriba.

<input type="checkbox" id="image-checkbox-03" class="image-checkbox">
<label for="image-checkbox-03"  class="image-label">
    <img class="img-container" src="/assets/img/posts/registrar-app-y-dar-permisos-en-azure/imagen03.png">
</label>

<br><br>

En la ventana de creación, ingresamos el nombre y añadimos la URL de redirección.

> https://oauth.pstmn.io/v1/browser-callback

<input type="checkbox" id="image-checkbox-04" class="image-checkbox">
<label for="image-checkbox-04"  class="image-label">
    <img class="img-container" src="/assets/img/posts/registrar-app-y-dar-permisos-en-azure/imagen04.png">
</label>

<br><br>

Una vez hecho esto, tendremos nuestra aplicación creada.

Aquí encontraremos una gran cantidad de información que necesitaremos.

Las dos primeras son el Tenant y el Client ID.

<input type="checkbox" id="image-checkbox-05" class="image-checkbox">
<label for="image-checkbox-05"  class="image-label">
    <img class="img-container" src="/assets/img/posts/registrar-app-y-dar-permisos-en-azure/imagen05.png">
</label>

<br><br>

Estas dos cadenas de números y letras debemos guardarlas. Si tienes la interfaz en español, pueden aparecer como:

-   Id. de aplicación (cliente) = Client ID
-   Id. de directorio (inquilino) = Tenant

Ahora debemos crear un secreto para la aplicación. Para ello, seguimos los pasos del siguiente menú.

<input type="checkbox" id="image-checkbox-06" class="image-checkbox">
<label for="image-checkbox-06"  class="image-label">
    <img class="img-container" src="/assets/img/posts/registrar-app-y-dar-permisos-en-azure/imagen06.png">
</label>

<br><br>

Añadimos una descripción y establecemos la fecha de vencimiento.

<input type="checkbox" id="image-checkbox-07" class="image-checkbox">
<label for="image-checkbox-07"  class="image-label">
    <img class="img-container" src="/assets/img/posts/registrar-app-y-dar-permisos-en-azure/imagen07.png">
</label>

<br><br>

Al agregarlo, se generará un secreto que debemos copiar.

<input type="checkbox" id="image-checkbox-08" class="image-checkbox">
<label for="image-checkbox-08"  class="image-label">
    <img class="img-container" src="/assets/img/posts/registrar-app-y-dar-permisos-en-azure/imagen08.png">
</label>

<br><br>

Es importante guardar estos datos, ya que los utilizaremos más adelante.

Ahora es el momento de otorgar permisos. Accedemos al menú lateral y seleccionamos "Agregar un permiso".

<input type="checkbox" id="image-checkbox-09" class="image-checkbox">
<label for="image-checkbox-09"  class="image-label">
    <img class="img-container" src="/assets/img/posts/registrar-app-y-dar-permisos-en-azure/imagen09.png">
</label>

<br><br>

Hacemos clic en "Microsoft Graph".

<input type="checkbox" id="image-checkbox-10" class="image-checkbox">
<label for="image-checkbox-10"  class="image-label">
    <img class="img-container" src="/assets/img/posts/registrar-app-y-dar-permisos-en-azure/imagen10.png">
</label>

<br><br>

Luego seleccionamos "Permisos de la aplicación".

<input type="checkbox" id="image-checkbox-11" class="image-checkbox">
<label for="image-checkbox-11"  class="image-label">
    <img class="img-container" src="/assets/img/posts/registrar-app-y-dar-permisos-en-azure/imagen11.png">
</label>

<br><br>

Buscamos los siguientes permisos:

<input type="checkbox" id="image-checkbox-12" class="image-checkbox">
<label for="image-checkbox-12"  class="image-label">
    <img class="img-container" src="/assets/img/posts/registrar-app-y-dar-permisos-en-azure/imagen12.png">
</label>

<br>

<input type="checkbox" id="image-checkbox-13" class="image-checkbox">
<label for="image-checkbox-13"  class="image-label">
    <img class="img-container" src="/assets/img/posts/registrar-app-y-dar-permisos-en-azure/imagen13.png">
</label>

<br><br>

Después de agregar los permisos, pulsamos el botón "Agregar permisos".

<input type="checkbox" id="image-checkbox-14" class="image-checkbox">
<label for="image-checkbox-14"  class="image-label">
    <img class="img-container" src="/assets/img/posts/registrar-app-y-dar-permisos-en-azure/imagen14.png">
</label>

<br><br>

Veremos que los permisos aún no están completamente autorizados, por lo que debemos garantizar estos permisos.

Esto lo hacemos seleccionando la opción marcada arriba.

<input type="checkbox" id="image-checkbox-15" class="image-checkbox">
<label for="image-checkbox-15"  class="image-label">
    <img class="img-container" src="/assets/img/posts/registrar-app-y-dar-permisos-en-azure/imagen15.png">
</label>

<br><br>

Ahora ya tenemos todo configurado en Azure para que funcione todo.

<br>

Hemos aprendido cómo registrar una aplicación en Azure Active Directory y otorgarle los permisos necesarios para ejecutar pruebas con Postman. Hemos seguido los pasos para crear una aplicación registrada, obtener los ID necesarios, generar un secreto y configurar los permisos en Azure. Ahora, estamos listos para utilizar la API Graph con nuestra aplicación registrada

<br>

¡Espero que os resulte útil y emocionante! No dudéis en enviar vuestros comentarios y preguntas. ¡Hasta la próxima!
