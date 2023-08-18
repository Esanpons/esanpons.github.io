---
title: Cierre de contabilidad
summary: "tutorial para hacer un cierre de contabilidad"
layout: article
author: Esteve Sanpons
#cSpell:disable
category: [Funcional, Manuales, Finanzas]
custom_type: Boveda
permalink: /boveda/2
#cSpell:enable
date: 2022-08-13 13:01:00 +0200
---

###### Cierre de contabilidad

<br>

## Introducción

A continuación, se detallan los pasos a seguir para realizar el cierre contable de la empresa. Las opciones están detalladas por orden de ejecución porque existen ciertas opciones que necesitan que se hayan ejecutado otras, por ejemplo: no se puede imprimir el "Libro diario oficial" si no no se han renumerado los asientos.

## Cierre contable

Antes de realizar el cierre de una empresa hay que:

-   Registradas todas las facturas del ejercicio que queremos cerrar.

-   Ejecutar los procesos de valoración de stock y registro de variación de existencias.

    -   Este paso se debe realizar aún y teniendo configurada el registro automático de variación de existencias.

    -   El registro de variación de existencias considera un número importante de variables que se deben tener en cuenta. Si no se ha ejecutado antes, consultar con su consultor asignado en Netzer.

Posteriormente, al finalizar todos los pasos detallados a continuación, se pueden hacer más asientos contables dentro del ejercicio cerrado.
Después, se tendrán que repetir los pasos a partir del "Asiento de regularización".

### Cierre del ejercicio

A esta opción se accede desde: Gestión financiera -\ Actividades periódicas -\ Ejercicio -\ Periodos contables.

Marcamos el periodo de final de ejercicio haciendo clic en el botón "Fijar cierre".

<img class="img-container"  src="/assets/img/articles/2/image1.png">
<br><br><br>

### Asiento de regularización

El asiento de regularización traspasa los saldos de las cuentas comerciales (grupos 6 y 7) a la cuenta que tengamos configurada como "Cuenta regularización".

La "Cuenta de regularización" se detalla en las fichas de cada una de las cuentas definidas en el "Plan de cuentas" (Gestión financiera -\Contabilidad).

El acceso al "Asiento de regularización" está en: Gestión financiera -\Contabilidad -\ Actividades periódicas -\ Ejercicio.

<img class="img-container"  src="/assets/img/articles/2/image2.png">
<br><br><br>

Si no trabajamos con divisa adicional, no rellenamos el campo Cta. Ajtes. Red. Div. Adic.

Los campos de "Cerrado por" se utilizan para que el sistema realice los asientos de regularización teniendo en cuenta las combinaciones de dimensiones existentes en las cuentas comerciales.

El proceso genera un asiento contable automáticamente con la fecha indicada en pantalla añadiendo delante el carácter "U" para que siempre sean estos asientos los últimos de todo el ejercicio.

### Numeración de asientos contables

Como en los libros oficiales hay que detallar los asientos numerados en orden de fecha y Navision permite hacer los asientos en el orden que se desee, hay que renumerarlos antes de generar los libros.

Esta opción está en: Gestión financiera -\ Contabilidad -\ Historial -\ Registro movs. contabilidad -\ pestaña Acciones -\ botón "Asignar número periodo".

<img class="img-container"  src="/assets/img/articles/2/image3.png">
<br><br><br>

### Impresión de libros.

-   "Libro diario oficial" ó "Libro diario oficial resumido".

Según indicaciones de la asesoría fiscal, se presenta uno u otro documento. Se accede desde Gestión financiera -\ Contabilidad -\ Informes.

La fecha final del periodo debe incluir el asiento de regularización, con los cual tendrá el carácter "U" delante de la fecha (ejemplo: U3112xx)

<img class="img-container"  src="/assets/img/articles/2/image4.png">
<br><br><br>

-   Balance de Sumas y Saldos, separado por trimestres. (Gestión financiera -\ Contabilidad -\ Informes)

-   Estados de PyG. (Gestión financiera -\ Contabilidad -\ Esquemas de cuentas)

-   Balance de Situación. (Gestión financiera -\ Contabilidad -\ Esquemas de cuentas)

### Filtros para el balance de sumas y saldos:

<img class="img-container"  src="/assets/img/articles/2/image5.png">
<br><br><br>
