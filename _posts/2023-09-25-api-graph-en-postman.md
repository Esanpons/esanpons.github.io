---
layout: post
title: "Descubre la potencia de la API Graph de Microsoft"
summary: "Descubre cómo configurar Postman con la potente API Graph de Microsoft. Simplifica tu proceso de desarrollo y aprovecha al máximo los recursos y datos disponibles en la API Graph de Microsoft con la ayuda de Postman."
author: "Esteve Sanpons"
date: "2023-09-25 04:00:00 +0200"
#cSpell:disable
category: ["API_Graph", "Postman"]
thumbnail: /assets/img/posts/api-graph-en-postman/imagen01.png
permalink: /blog/api-graph-en-postman/
custom_type: Blog
#cSpell:enable
---

¡Hola! ¡Aquí estoy una semana más para traeros un montón de descubrimientos nuevos!

En las próximas semanas vamos a adentrarnos en el maravilloso mundo de la API Graph.

¿Y qué es eso de la API Graph, os preguntaréis?

La API Graph de Microsoft es una potente interfaz que os permite acceder y gestionar los recursos de los diferentes servicios y productos del ecosistema de Microsoft, como Office 365, Azure, SharePoint, OneDrive, Outlook y muchos más. Es una herramienta versátil y completa que os brinda la posibilidad de interactuar con datos y funcionalidades de diversos servicios en un solo lugar.

A grandes rasgos y para no complicar mucho el asunto, la API Graph es la interfaz unificada de programación de aplicaciones (API) de Microsoft que os permite acceder a los recursos y datos de los servicios y productos mencionados anteriormente. Con la API Graph, podemos realizar consultas, obtener información, crear, actualizar y eliminar recursos, y mucho más, todo ello de forma integrada y coherente.

Si queréis obtener más información sobre la API Graph y explorar todas las posibilidades que ofrece, podéis consultar la documentación oficial en el siguiente [Link](https://learn.microsoft.com/es-es/graph/use-the-api)

<br><br>

Tenemos 5 pasos que debemos seguir y os los iré explicando en los próximos días.

1. Copiar Microsoft Graph en Postman.
2. Crear una aplicación registrada en Azure y otorgarle permisos.
3. Configurar y ejecutar en Postman una función de recepción de correo.
4. Solicitar el token a la API Graph utilizando OAuth 2.0 desde Business Central.
5. Realizar la misma solicitud de recepción de correo, pero esta vez en Business Central.
6. Como veis, hay mucha información, pero os aseguro que es muy útil.

<br>

El objetivo final es para poder leer los correos electrónicos que estén en la bandeja de entrada y que no estén leídos en nuestro Business Central.

Tenéis que tener en cuenta que hay un sinfín de ecosistemas en la API Graph de Microsoft, por lo que esto no tiene límite.

<br>

Dicho todo esto vamos manos a la obra. 😆

<br>

Lo primero que tenemos que hacer es ir al [Link](https://aka.ms/graphpostmanwkspc)

<br>

Tenéis que tener una cuenta creada en Postman para que todo esto vaya bien y también tenéis que descargar su aplicación e instalarla.

El enlace os llevará al Workspaces de Microsoft, donde tenéis unas funciones ya preparadas para que podáis hacer pruebas.

Ahora tenéis que ir y crear un fork.

<input type="checkbox" id="image-checkbox-02" class="image-checkbox">
<label for="image-checkbox-02"  class="image-label">
    <img class="img-container" src="/assets/img/posts/api-graph-en-postman/imagen02.png">
</label>

<br><br>

Esto es tan sencillo como darle a los 3 puntitos y presionar “create a fork”.

Os aparecerá una ventana que os pedirá que le añadáis un nombre al fork y también en qué workspace lo queréis colocar.

<input type="checkbox" id="image-checkbox-03" class="image-checkbox">
<label for="image-checkbox-03"  class="image-label">
    <img class="img-container" src="/assets/img/posts/api-graph-en-postman/imagen03.png">
</label>

<br><br>

Para finalizarlo todo, le dais al botón "Fork Collection" para crear esa copia duplicada en vuestro Workspaces.

Una vez hecho esto, vais a Postman, que ya tenéis instalado.

Ahora, si vais a vuestros workspaces, veréis que se ha creado una nueva colección llamada Microsoft Graph.

<input type="checkbox" id="image-checkbox-04" class="image-checkbox">
<label for="image-checkbox-04"  class="image-label">
    <img class="img-container" src="/assets/img/posts/api-graph-en-postman/imagen04.png">
</label>

<br><br>

Ahora si vamos a intentar que nos de el token

Esto esta por ejemplo en:

<input type="checkbox" id="image-checkbox-05" class="image-checkbox">
<label for="image-checkbox-05"  class="image-label">
    <img class="img-container" src="/assets/img/posts/api-graph-en-postman/imagen05.png">
</label>

<br><br>

Al darle al botón nos dará error de autentificación ya que no lo tenemos configurado aún.

<input type="checkbox" id="image-checkbox-06" class="image-checkbox">
<label for="image-checkbox-06"  class="image-label">
    <img class="img-container" src="/assets/img/posts/api-graph-en-postman/imagen06.png">
</label>

<br><br>

Esto es debido a que no tenemos los valores configurados aun.

<input type="checkbox" id="image-checkbox-07" class="image-checkbox">
<label for="image-checkbox-07"  class="image-label">
    <img class="img-container" src="/assets/img/posts/api-graph-en-postman/imagen07.png">
</label>

<br><br>

Bueno hoy hemos visto cómo obtener las funciones directamente del workspaces de Microsoft, así no las tenemos que crear nosotros mismos y con ello nos facilita las pruebas que haremos a partir de hoy.

<br>

¡Espero que os resulte útil y emocionante! No dudéis en enviar vuestros comentarios y preguntas. ¡Hasta la próxima!
