---
title: Usuarios, perfiles y permisos
summary: "Una breve explicación de como funciona los usuarios, roles y perfiles"
layout: article
author: Esteve Sanpons
category: [Funcional, Manuales, Usuarios, Perfil, Roles]
date: 2022-08-22 09:02:00 +0200
---

###### Usuarios, perfiles y permisos

<br>

## Creación de usuarios

Vamos a usuarios, clicamos Nuevo

<img class="img-container"  src="/assets/img/articles/usuarios-perfiles-y-permisos/image1.png">
<br><br><br>

En la ficha de usuario agregaremos los datos necesarios para su
creación.

<img class="img-container"  src="/assets/img/articles/usuarios-perfiles-y-permisos/image2.png">
<br><br><br>

Si creamos un usuario de dominio le daremos a los tres puntos de nombre
de usuario de Windows y después buscaremos en el dominio el usuario en
cuestión. En esta opción no tenemos que poner contraseña porque ya viene
dada des de Windows.

Si solo creamos el usuario, le pondremos el nombre y la contraseña en
los campos arriba marcados.

Si dejamos el check marcado obligaremos al usuario a cambiar la
contraseña la próxima vez que inicie sesión, para que así ponga la
contraseña que él quiera.

<img class="img-container"  src="/assets/img/articles/usuarios-perfiles-y-permisos/image3.png">
<br><br><br>

En las dos ultimas pestañas le asignaremos los grupos o roles que sean
necesarios para dicho usuario.

<img class="img-container"  src="/assets/img/articles/usuarios-perfiles-y-permisos/image4.png">
<br><br><br>

## Crear o modificar los perfiles

Ahora iremos a la lista de perfiles para administrar los perfiles que
tenemos o crear de nuevos.

<img class="img-container"  src="/assets/img/articles/usuarios-perfiles-y-permisos/image5.png">
<br><br><br>

Tanto si editamos uno existente como si creamos uno nuevo nos aparecerá
el siguiente formulario.

<img class="img-container"  src="/assets/img/articles/usuarios-perfiles-y-permisos/image6.png">
<br><br><br>

- Id. Perfil: es el id para identificarlo

- Descripción: este campo lo utilizaremos para una breve descripción
  del perfil

- Id. Área de trabajo: aquí escogeremos el número del área de trabajo
  que queramos para este perfil

- Area trabajo predet: si tenemos esta casilla marcada es para que el
  perfil en cuestión sea el predeterminado.

- Deshabilitar personalización: si tenemos esto habilitado, el usuario
  no podrá personalizar nada en su perfil.

Las demás opciones no las utilizaremos.

## Asignar perfil a un usuario

Nos vamos a la ventana de lista personalización usuario, aquí podremos
asignar varios valores, entre ellos el perfil.

<img class="img-container"  src="/assets/img/articles/usuarios-perfiles-y-permisos/image7.png">
<br><br><br>

Aquí podemos editar las personalizaciones que tengan los usuarios o si
el usuario no está crear una nueva.

Al entrar en el formulario podremos modificar o añadir las siguientes
opciones.

<img class="img-container"  src="/assets/img/articles/usuarios-perfiles-y-permisos/image8.png">
<br><br><br>

- Id de usuario: aquí asignamos el usuario

- Id. Perfil: ponemos el perfil para el usuario

- Id. Idioma y Id configuración: en estos campos mejor poner 1034 en
  lugar de 3082.

- Zona horaria: tendremos que poner, Romance Standard Time

- Empresa: en este campo lo podemos poner vacío para que tenga la
  personalización para todas las empresas.

## Crear roles

Vamos a Conjunto de permisos, en esta ventana es donde administraremos
cada uno de los roles que tienen nuestros usuarios.

<img class="img-container"  src="/assets/img/articles/usuarios-perfiles-y-permisos/image9.png">
<br><br><br>

Haremos un nuevo rol, ponemos el id y la descripción.

<img class="img-container"  src="/assets/img/articles/usuarios-perfiles-y-permisos/image10.png">
<br><br><br>

Después ciclamos en el botón Permisos. Aquí es donde podremos insertar
los permisos a este rol.

<img class="img-container"  src="/assets/img/articles/usuarios-perfiles-y-permisos/image11.png">
<br><br><br>

Podemos añadir el permiso si sabemos el número.

En esta pantalla tenemos los campos Tipo objeto que aquí es donde
especificaremos que tipo es, si es page o tableData o report....

Las 4 columnas de lectura, inserción, modificacion y eliminación, son
los permisos que tendrá para ese objeto. Poner siempre los permisos
necesarios.

<img class="img-container"  src="/assets/img/articles/usuarios-perfiles-y-permisos/image11.png">
<br><br><br>

Si no sabemos el número del objeto podemos acceder a la lista
desplegable para buscar en avanzado por nombre del objeto. Cuando lo
tengamos podemos hacer dos cosas o apuntar el numero en la ventana
anterior o aceptar y se añadirá a la ventana anterior.

<img class="img-container"  src="/assets/img/articles/usuarios-perfiles-y-permisos/image12.png">
<br><br><br>

## Crear grupos de usuarios y agregar roles

Nos vamos a la ventana de grupos de usuarios y clicamos nuevo para
añadir un nuevo grupo.

Añadimos el código, el nombre y aquí podemos añadir el perfil
predeterminado que queramos para el grupo.

<img class="img-container"  src="/assets/img/articles/usuarios-perfiles-y-permisos/image13.png">
<br><br><br>

Ahora añadiremos los roles al grupo

<img class="img-container"  src="/assets/img/articles/usuarios-perfiles-y-permisos/image14.png">
<br><br><br>

Ahora añadimos todos los roles que utilizara este grupo de usuarios

<img class="img-container"  src="/assets/img/articles/usuarios-perfiles-y-permisos/image15.png">
<br><br><br>

## Agregar grupo o rol al usuario

Vamos a la ventana de usuarios, aquí buscamos el usuario que le vamos a
poner el grupo o rol y clicamos en editar.

<img class="img-container"  src="/assets/img/articles/usuarios-perfiles-y-permisos/image16.png">
<br><br><br>

En la edición del usuario las pestañas que nos interesa son Grupo de
usuarios o Conjunto de permisos de usuarios.

Dependiendo de si queremos añadir un grupo o solo un rol, lo pondremos
en una sección o en otra.

<img class="img-container"  src="/assets/img/articles/usuarios-perfiles-y-permisos/image17.png">
<br><br><br>
