---
title: "Funciones String"
summary: "Resumen de las funciones String de Navision y Business Central"
layout: article
author: Esteve Sanpons
#cSpell:disable
category: [Navision, Business_Central]
custom_type: Boveda
permalink: /boveda/funciones-string
#cSpell:enable
date: 2023-06-01 21:00:00 +0200
LinkedIn: false
---

Los ejemplos los vamos a hacer en Business Central, pero sirven igual para las versiones anteriores.

Así que voy a explicar con algunos ejemplos cada una de las funciones.

<input type="checkbox" id="image-checkbox-01" class="image-checkbox">
<label for="image-checkbox-01"  class="image-label">
    <img class="img-container" src="/assets/img/articles/funciones-string/Imagen01.png">
</label>
<br><br><br><br>

## STRSUBSTNO

Reemplaza los campos% 1,% 2,% 3 ... y/o # 1, # 2, # 3 ... de una cadena con los valores que proporciones como parámetros opcionales.
Substituye los %1 por la variable que pongas.

Documentación oficial de Microsoft en [Link](https://docs.microsoft.com/en-us/dynamics-nav/strsubstno-function--code--text-)

#### Ejemplo

Las tres variables son claras, Customer es para buscar los datos, MyTxt para almacenar el resultado y por último MyLbl es para el texto a formatear.

<input type="checkbox" id="image-checkbox-02" class="image-checkbox">
<label for="image-checkbox-02"  class="image-label">
    <img class="img-container" src="/assets/img/articles/funciones-string/Imagen02.png">
</label>
<br><br>

Lo que estamos haciendo aquí es substituir el “%1” por el numero de cliente y el “%2” por el nombre del cliente

El resultado sería:

> El número de cliente 10000 tiene el nombre GDE Distribución S.A.

<br><br><br><br>

## STRPOS

Busca la primera aparición de la subcadena dentro de una cadena.

Documentación oficial de Microsoft en [Link](https://docs.microsoft.com/en-us/dynamics-nav/strpos-function--code--text-)

#### Ejemplo

Aquí al igual que arriba creamos la variable de cliente dos Labels, el primero para buscar la palabra y el segundo para mostrar el mensaje, por último, una variable para que nos indique en que posición esta.

<input type="checkbox" id="image-checkbox-03" class="image-checkbox">
<label for="image-checkbox-03"  class="image-label">
    <img class="img-container" src="/assets/img/articles/funciones-string/Imagen03.png">
</label>
<br><br>

En la función lo que tenemos que pasarla primero es el texto donde estará lo que queremos buscar, y en el segundo parámetro el texto a buscar.

Notar que en este ejemplo a la hora de sacarlo por el mensaje no hemos hecho el StrSubstNo por que el MESSAGE lo lleva incorporado.

El resultado sería:

> El texto "S.A." empieza en la posición 18, del nombre de cliente GDE Distribución S.A.

<br><br><br><br>

## STRLEN

Obtiene la longitud de una cadena que define.

Documentación oficial de Microsoft en [Link](https://docs.microsoft.com/en-us/dynamics-nav/strlen-function--code--text-)

#### Ejemplo

Creamos la variable de ejemplo de cliente, el Integer para obtener la longitud del texto y por ultimo el texto que mostraremos en pantalla

<input type="checkbox" id="image-checkbox-04" class="image-checkbox">
<label for="image-checkbox-04"  class="image-label">
    <img class="img-container" src="/assets/img/articles/funciones-string/Imagen04.png">
</label>
<br><br>

Como vemos este ejemplo lo que nos devolverá es el total de caracteres en un texto en concreto, o lo que es lo mismo la longitud del texto.

El resultado sería:

> El texto "GDE Distribución S.A.", tiene 21 caracteres.

<br><br><br><br>

## INCSTR

Aumenta un número positivo o disminuye un número negativo dentro de una cadena en uno (1).

Documentación oficial de Microsoft en [Link](https://docs.microsoft.com/en-us/dynamics-nav/incstr-function--code--text-)

#### Ejemplo

Las variables son un texto para recibir el resultado y dos labels uno donde estará el texto a incrementar y el otro para mostrar el resultado por mensaje.

<input type="checkbox" id="image-checkbox-05" class="image-checkbox">
<label for="image-checkbox-05"  class="image-label">
    <img class="img-container" src="/assets/img/articles/funciones-string/Imagen05.png">
</label>
<br><br>

Lo que estamos haciendo aquí es substituir el “%1” por el numero de cliente y el “%2” por el nombre del cliente

El resultado sería:

> El número de cliente 10000 tiene el nombre GDE Distribución S.A.

<br><br><br><br>

## COPYSTR

Copia una subcadena de cualquier longitud desde una posición específica en una cadena (texto o código) a una nueva cadena.
Extrae parte del texto de la posición hasta el final.

Documentación oficial de Microsoft en [Link](https://docs.microsoft.com/en-us/dynamics-nav/copystr-function--code--text-)

#### Ejemplo

Volvemos a crear la variable de cliente, el texto de destino y el label final para mostrar por mensaje.

<input type="checkbox" id="image-checkbox-06" class="image-checkbox">
<label for="image-checkbox-06"  class="image-label">
    <img class="img-container" src="/assets/img/articles/funciones-string/Imagen06.png">
</label>
<br><br>

Como podemos ver lo que hemos hecho es descomponer el nombre del cliente sacando una parte del texto.

El resultado sería:

> El texto inicial era: "GDE Distribución S.A." y el nuevo testo es: "Distribución"

<br><br><br><br>

## MAXSTRLEN

Obtiene la longitud máxima definida de una variable de cadena.

Documentación oficial de Microsoft en [Link](https://docs.microsoft.com/en-us/dynamics-nav/maxstrlen-function--code--text-)

#### Ejemplo

Primero creamos la variable de clientes para tener el texto a comprobar, después la variable de Integer para que nos de el resultado y por ultimo la variable que mostraremos en el mensaje.

<input type="checkbox" id="image-checkbox-07" class="image-checkbox">
<label for="image-checkbox-07"  class="image-label">
    <img class="img-container" src="/assets/img/articles/funciones-string/Imagen07.png">
</label>
<br><br>

Como podemos ver la función solo le tenemos que pasar el texto que queremos analizar y ver su máximo contenido.

Hemos ido a la tabla a buscar como esta creado este campo para que podáis ver que el resultado que nos esta dando e el mismo que esta parametrizado en el campo de la tabla.

<input type="checkbox" id="image-checkbox-08" class="image-checkbox">
<label for="image-checkbox-08"  class="image-label">
    <img class="img-container" src="/assets/img/articles/funciones-string/Imagen08.png">
</label>
<br><br>

El resultado sería:

> El texto inicial era: "GDE Distribución S.A." tiene 100 de capacidad maxima.

<br><br><br><br>

## PADSTR

Cambia la longitud de una cadena a una longitud que usted defina.
También rellenar el resto del texto.

Documentación oficial de Microsoft en [Link](https://docs.microsoft.com/en-us/dynamics-nav/padstr-function--code--text-)

#### Ejemplo

En este ejemplo crearemos la variable de clientes, dos texto para recoger los resultados y tres variables de label para mostrar el resultado.

<input type="checkbox" id="image-checkbox-09" class="image-checkbox">
<label for="image-checkbox-09"  class="image-label">
    <img class="img-container" src="/assets/img/articles/funciones-string/Imagen09.png">
</label>
<br><br>

En este ejemplo añadimos una cosa que hasta ahora no lo habíamos mostrado y es que podemos hacer un salto de línea añadiendo al final del texto el símbolo “\”
Por lo que las tres variables de tipo Label las unimos en el mismo mensaje, pero se mostraran en líneas diferentes.

Vamos a explicar un poco los dos resultados.
El primero lo que hace es algo similar al CopyStr pero el parámetro de inicio es siempre 1. Por lo que acortara el texto hasta lo que le marquemos.

El segundo ejemplo es para que le digamos que quiere rellenar el resto del texto con el texto que hayamos puesto en el parámetro el final en este caso el número del cliente tiene 5 caracteres y añadimos 10 mas como “0”.

El resultado sería:

> El texto original es: "10000".
> <br>
> El resultado del primer ejemplo es: "10".
> <br>
> El resultado del segundo ejemplo es:"100000000000000".

<br><br><br><br>

## DELCHR

Elimina uno o más caracteres de una cadena.
Le dices un carácter y te borra de un texto ese carácter.

Documentación oficial de Microsoft en [Link](https://docs.microsoft.com/en-us/dynamics-nav/delchr-function--code--text-)

#### Ejemplo

Creamos la variable de clientes. Después el texto donde recibiremos el resultado del formateo. Lo siguiente son las labels de parámetros y por ultimo los labels de mensaje.

<input type="checkbox" id="image-checkbox-10" class="image-checkbox">
<label for="image-checkbox-10"  class="image-label">
    <img class="img-container" src="/assets/img/articles/funciones-string/Imagen10.png">
</label>
<br><br>

Los labels de parámetros son lo más complicado de entender de esta función, sobre todo el label “WhereLbl”. Este label hará que la función haga una cosa u otra dependiendo de lo que le añadamos.
Los tipos pueden ser:

-   “=” todos los textos iguales se eliminan que el parámetro indicado.
-   “<” se eliminan los textos que empiecen por el parámetro indicado.
-   “>” se eliminan los texto que finalicen por el parámetro indicado.
-   Si esta vacío devolverá el teto tal cual esta.

El segundo parámetro es para indicarle que queremos eliminar.
En este ejemplo lo que hemos querido eliminar es la letra “D”.

El resultado sería:

> El texto original es: "GDE Distribución S.A."
> <br>
> El texto nuevo es: "DE Distribución S.A."

<br><br><br><br>

## STRCHECKSUM

Calcula una suma de comprobación para una cadena que contiene un número.

Documentación oficial de Microsoft en [Link](https://docs.microsoft.com/en-us/dynamics-nav/strchecksum-function--code--text-)

#### Ejemplo

Las variables en este caso son, dos Integers uno para recibir el resultado, dos labels para los números a calcular y los dos labels del mensaje.

<input type="checkbox" id="image-checkbox-11" class="image-checkbox">
<label for="image-checkbox-11"  class="image-label">
    <img class="img-container" src="/assets/img/articles/funciones-string/Imagen11.png">
</label>
<br><br>

Para que lo entendamos un poco mejor lo que está haciendo la fórmula es esta:
(7 - (4x1 + 3x2 + 7x3 + 8x4) MOD 7) MOD 7=0

El resultado sería:

> El número original es: 4378
> <br>
> El resultado de la verificación de suma es: 0

<br><br><br><br>

## CONVERTSTR

Convierte algunos caracteres en una cadena.
Reemplazar caracteres en un texto.

Documentación oficial de Microsoft en [Link](https://docs.microsoft.com/en-us/dynamics-nav/convertstr-function--code--text-)

#### Ejemplo

Creamos la variable de clientes, 2 labels donde indicamos lo que queremos cambiar y como y los labels que mostraremos con el mensaje.

<input type="checkbox" id="image-checkbox-12" class="image-checkbox">
<label for="image-checkbox-12"  class="image-label">
    <img class="img-container" src="/assets/img/articles/funciones-string/Imagen12.png">
</label>
<br><br>

Este proceso lo que hace es substituir que este en el “My1Lbl” por lo que este en el “My2Lbl” y devuelve el nuevo texto.

El resultado sería:

> El texto original es: "GDE Distribución S.A."
> <br>
> El nuevo texto es: "gde Distribución S.A."

<br><br><br><br>

## LOWERCASE

Convierte todas las letras de una cadena a minúsculas.

Documentación oficial de Microsoft en [Link](https://docs.microsoft.com/en-us/dynamics-nav/lowercase-function--code--text-)

#### Ejemplo

Las variables son cliente, la variable de texto y los labels del mensaje.

<input type="checkbox" id="image-checkbox-13" class="image-checkbox">
<label for="image-checkbox-13"  class="image-label">
    <img class="img-container" src="/assets/img/articles/funciones-string/Imagen13.png">
</label>
<br><br>

Como podemos ver en el código esto es simple transforma todo el texto en minúsculas.

El resultado sería:

> El texto original es: "GDE Distribución S.A."
> <br>
> El nuevo texto es: "gde distribución s.a."

<br><br><br><br>

## UPPERCASE

Convierte todas las letras de una cadena a mayúsculas.

Documentación oficial de Microsoft en [Link](https://docs.microsoft.com/en-us/dynamics-nav/uppercase-function--code--text-)

#### Ejemplo

Las variables son cliente, la variable de texto y los labels del mensaje.

<input type="checkbox" id="image-checkbox-14" class="image-checkbox">
<label for="image-checkbox-14"  class="image-label">
    <img class="img-container" src="/assets/img/articles/funciones-string/Imagen14.png">
</label>
<br><br>

Como podemos ver en el código esto es simple transforma todo el texto en mayúsculas.

El resultado sería:

> El texto original es: "GDE Distribución S.A."
> <br>
> El nuevo texto es: "GDE DISTRIBUCIÓN S.A."

<br><br><br><br>

## SELECTSTR

Recupera una subcadena de una cadena separada por comas.
Seleccionar un trozo del texto que este entre comas.

Documentación oficial de Microsoft en [Link](https://docs.microsoft.com/en-us/dynamics-nav/selectstr-function--code--text-)

#### Ejemplo

Las variables son el texto que recibirá el resultado, el label donde tenemos el texto original y por ultimo los labels del mensaje.

<input type="checkbox" id="image-checkbox-15" class="image-checkbox">
<label for="image-checkbox-15"  class="image-label">
    <img class="img-container" src="/assets/img/articles/funciones-string/Imagen15.png">
</label>
<br><br>

Esta función es como si transformara el texto que hay entre cada coma como si fuera un array, por lo que si le decimos el 2 lo que devolverá es la palabra que hay después de la primera coma.

El resultado sería:

> El texto original es: "Esteve Sanpons Carballares"
> <br>
> El nuevo texto es: "Sanpons"

<br><br><br><br>

## DELSTR

Elimina una subcadena dentro de una cadena (texto o código).
Borrar un cacho de cadena con posición inicial y final.

Documentación oficial de Microsoft en [Link](https://docs.microsoft.com/en-us/dynamics-nav/delstr-function--code--text-)

#### Ejemplo

Primero creamos la variable de clientes, una de texto para recibir el resultado y por ultimo los labels del mensaje.

<input type="checkbox" id="image-checkbox-16" class="image-checkbox">
<label for="image-checkbox-16"  class="image-label">
    <img class="img-container" src="/assets/img/articles/funciones-string/Imagen16.png">
</label>
<br><br>

Como podemos ver lo que hace esta función es eliminar des de una posición hasta tantos caracteres como le decimos.
En este caso lo que queremos es quitar la parte inicial del texto.

El resultado sería:

> El texto original es: "GDE Distribución S.A."
> <br>
> El nuevo texto es: "Distribución S.A."

<br><br><br><br>

## INSSTR

Inserta una subcadena en una cadena.
Añade texto en mitad de un texto.

Documentación oficial de Microsoft en [Link](https://docs.microsoft.com/en-us/dynamics-nav/insstr-function--code--text-)

#### Ejemplo

Primero creamos la variable de clientes, una de texto para recibir el resultado y por ultimo los labels del mensaje.

<input type="checkbox" id="image-checkbox-17" class="image-checkbox">
<label for="image-checkbox-17"  class="image-label">
    <img class="img-container" src="/assets/img/articles/funciones-string/Imagen17.png">
</label>
<br><br>

Esta función es muy curiosa lo que hace es insertar un texto en mitad de otro en el lugar que tú le indiques.

El resultado sería:

> El texto original es: "GDE Distribución S.A."
> <br>
> El nuevo texto es: "GDE Distribuciones S.A."

<br><br><br><br>

GitHub de los ejemplos en el [Link](https://github.com/Esanpons/ejemplos-blog/tree/main/AL/FunctionsStrings)
