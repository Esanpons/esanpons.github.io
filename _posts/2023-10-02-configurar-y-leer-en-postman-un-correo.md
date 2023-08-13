---
layout: post
title: "Configurar y leer correos en Postman usando el API Graph"
summary: " En este artículo aprenderás cómo configurar y leer correos utilizando el API Graph desde Postman. Te guiaré paso a paso para que puedas conectar tu aplicación y obtener acceso a los mensajes de correo de un usuario en particular. Además, te mostraré cómo filtrar los correos no leídos en el buzón de entrada."
author: "Esteve Sanpons"
date: "2023-10-01 04:00:00 +0200"
#cSpell:disable
category: ["API_Graph", "Postman"]
thumbnail: /assets/img/posts/configurar-y-leer-en-postman-un-correo/imagen01.png
permalink: /blog/configurar-y-leer-en-postman-un-correo/
custom_type: Blog
#cSpell:enable
---

¡Buenas una semana más! Hoy continuaremos explorando el API Graph.

<br>

En las últimas semanas, hemos hablado sobre cómo importar las funciones necesarias del API Graph a Postman en nuestro artículo anterior [Link](/blog/api-graph-en-postman/) y también sobre cómo registrar una aplicación y otorgar permisos en Azure en el artículo [Link](/blog/registrar-app-y-dar-permisos-en-azure/).

<br>

Esta semana nos centraremos en la configuración y lectura de correos desde Postman.

<br>

Sin mas dilación vamos manos a la obra. 😜

<br>

En el artículo anterior, dejamos configurados y copiados los datos necesarios para establecer la conexión.

Una vez que tenemos todos los datos, los añadiremos a Postman. Abriremos Postman y editaremos la solicitud, insertando los tres datos que creamos y copiamos anteriormente.

<input type="checkbox" id="image-checkbox-02" class="image-checkbox">
<label for="image-checkbox-02"  class="image-label">
    <img class="img-container" src="/assets/img/posts/configurar-y-leer-en-postman-un-correo/imagen02.png">
</label>

<br><br>

Ahora guardamos la carpeta "Application"

<input type="checkbox" id="image-checkbox-03" class="image-checkbox">
<label for="image-checkbox-03"  class="image-label">
    <img class="img-container" src="/assets/img/posts/configurar-y-leer-en-postman-un-correo/imagen03.png">
</label>

<br><br>

Podemos ver cómo los datos se reflejan en las diferentes variables de Postman.

<input type="checkbox" id="image-checkbox-04" class="image-checkbox">
<label for="image-checkbox-04"  class="image-label">
    <img class="img-container" src="/assets/img/posts/configurar-y-leer-en-postman-un-correo/imagen04.png">
</label>

<br><br>

Al hacer clic en el botón al final para crear el token, observamos que ahora estamos autorizados.

<input type="checkbox" id="image-checkbox-05" class="image-checkbox">
<label for="image-checkbox-05"  class="image-label">
    <img class="img-container" src="/assets/img/posts/configurar-y-leer-en-postman-un-correo/imagen05.png">
</label>

<br><br>

Vemos que ahora si que nos dice que estamos autorizados

<input type="checkbox" id="image-checkbox-06" class="image-checkbox">
<label for="image-checkbox-06"  class="image-label">
    <img class="img-container" src="/assets/img/posts/configurar-y-leer-en-postman-un-correo/imagen06.png">
</label>

<br><br>

YAl hacer clic en "Proceed", se abrirá una nueva ventana donde podremos utilizar el token que se nos ha devuelto.

<input type="checkbox" id="image-checkbox-07" class="image-checkbox">
<label for="image-checkbox-07"  class="image-label">
    <img class="img-container" src="/assets/img/posts/configurar-y-leer-en-postman-un-correo/imagen07.png">
</label>

<br><br>

Seleccionaremos el botón amarillo de sincronización y, luego, el otro botón para sincronizar el token.

<input type="checkbox" id="image-checkbox-08" class="image-checkbox">
<label for="image-checkbox-08"  class="image-label">
    <img class="img-container" src="/assets/img/posts/configurar-y-leer-en-postman-un-correo/imagen08.png">
</label>

<br><br>

Ahora, guardaremos de nuevo y ya tendremos el token listo para ser utilizado. Este token tiene una duración de 3600 segundos.

<input type="checkbox" id="image-checkbox-09" class="image-checkbox">
<label for="image-checkbox-09"  class="image-label">
    <img class="img-container" src="/assets/img/posts/configurar-y-leer-en-postman-un-correo/imagen09.png">
</label>

<br><br>

Realizaremos el mismo proceso para la carpeta "Delegated". Esto es necesario para configurar nuestro usuario y realizar pruebas con nuestra cuenta de Microsoft.

<input type="checkbox" id="image-checkbox-10" class="image-checkbox">
<label for="image-checkbox-10"  class="image-label">
    <img class="img-container" src="/assets/img/posts/configurar-y-leer-en-postman-un-correo/imagen10.png">
</label>

<br><br>

Se abrirá la pantalla de inicio de sesión de Microsoft, donde ingresaremos nuestro usuario y contraseña, y luego seguiremos el mismo proceso anterior con el token.

Ahora, vamos a buscar nuestro UserId. Esto lo haremos con la función "Get my profile".

<input type="checkbox" id="image-checkbox-11" class="image-checkbox">
<label for="image-checkbox-11"  class="image-label">
    <img class="img-container" src="/assets/img/posts/configurar-y-leer-en-postman-un-correo/imagen11.png">
</label>

<br><br>

Luego, iremos a las variables que habíamos editado anteriormente.

<input type="checkbox" id="image-checkbox-12" class="image-checkbox">
<label for="image-checkbox-12"  class="image-label">
    <img class="img-container" src="/assets/img/posts/configurar-y-leer-en-postman-un-correo/imagen12.png">
</label>

<br><br>

Y añadimos la variable del UserId

<input type="checkbox" id="image-checkbox-13" class="image-checkbox">
<label for="image-checkbox-13"  class="image-label">
    <img class="img-container" src="/assets/img/posts/configurar-y-leer-en-postman-un-correo/imagen13.png">
</label>

<br><br>

A continuación, utilizaremos la función "Get a user's message" para ver los mensajes disponibles para el usuario.

<input type="checkbox" id="image-checkbox-14" class="image-checkbox">
<label for="image-checkbox-14"  class="image-label">
    <img class="img-container" src="/assets/img/posts/configurar-y-leer-en-postman-un-correo/imagen14.png">
</label>

<br><br>

Como se puede observar, la variable UserId se encuentra completada con los datos que creamos anteriormente.

Al ejecutar la solicitud, veremos el resultado en formato JSON con los correos electrónicos.

Por último, quiero mostrarte cómo configurar la visualización de los mensajes en la bandeja de entrada y cómo filtrar solo los mensajes no leídos.

<input type="checkbox" id="image-checkbox-15" class="image-checkbox">
<label for="image-checkbox-15"  class="image-label">
    <img class="img-container" src="/assets/img/posts/configurar-y-leer-en-postman-un-correo/imagen15.png">
</label>

<br><br>

En resumen, hemos aprendido a cómo configurar y leer correos desde Postman utilizando el API Graph. Comenzamos por importar las funciones necesarias, configurar la conexión con los datos correspondientes y obtener un token de autorización. Luego, exploramos cómo buscar el UserId y utilizarlo para acceder a los mensajes del usuario. Además, vimos cómo filtrar los mensajes no leídos en la bandeja de entrada.

<br>

¡Espero que os resulte útil y emocionante! No dudéis en enviar vuestros comentarios y preguntas. ¡Hasta la próxima!
