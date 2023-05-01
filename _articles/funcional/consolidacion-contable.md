---
title: Consolidación Contable
summary: "Navision nos permite consolidar los movimientos de contabilidad"
layout: article
author: Esteve Sanpons
category: [Funcional, Manuales, Finanzas]
custom_type: Boveda
date: 2022-08-13 15:00:00 +0200
---

###### Consolidación Contable

<br>

## Introducción

Dynamics NAV nos permite consolidar los movimientos de contabilidad de
dos o más empresas independientes (subsidiarias) en una sola empresa
consolidada. Cada empresa individual implicada en una consolidación se
denomina empresa. La empresa combinada se denomina empresa consolidada,
que suele ser una empresa configurada exclusivamente con esta finalidad
y en la que no se realizan transacciones empresariales.

Navision permite consolidar:

- Empresas con varios planes de cuentas.

- Empresas con ejercicios distintos.

- Empresas con divisas distintas.

- El importe total o un porcentaje específico de la información
  financiera de una empresa determinada.

- Utilizando métodos distintos de conversión de cuentas individuales.

Por otro lado, se pueden consolidar empresas pertenecientes a:

- La misma base de datos que la empresa consolidada.

- Otras bases de datos de Microsoft Navision.

- Otros sistemas contables y de gestión empresarial (siempre que se
  pueda exportar la información del otro sistema a un archivo con el
  formato adecuado).

- En un archivo \"sin formato\" (.txt de versiones anteriores a la
  4.0) con campos de longitud fija o desde formato XML.

La empresa consolidada se configura en una base de datos de la misma
forma que otras empresas y su plan de cuentas es independiente de los
planes de cuentas de las otras empresas.

La funcionalidad de consolidación del sistema permite configurar una
lista de empresas a consolidar, comprobar los datos contables antes de
llevar a cabo la consolidación, importar archivos y bases de datos y
generar informes de consolidación.

### Pasos del proceso de consolidación

- Configuración de la empresa y las subsidiarias de consolidación.

- Exportación de los datos para consolidación (si fuera necesario por
  encontrarse en otra base de datos).

- Comprobación de los datos que se van a consolidar.

- Agregación de los datos que se van a consolidar.

- Procesamiento de las eliminaciones de consolidación.

## Configuración de la empresa y las subsidiarias de consolidación

Para poder llevar a cabo una consolidación, deberá haber configurado la
empresa consolidada. La empresa debe configurarse en el sistema del
mismo modo que cualquier

<img class="img-container"  src="/assets/img/articles/consolidacion-contable/image1.png">
<br><br><br>

### Configurar información de consolidación de las cuentas contables

En el plan de cuentas de la empresa que se vaya a consolidar se debe
especificar las cuentas de consolidación. Para cada cuenta auxiliar de
cada empresa, debe especificar la cuenta de la empresa consolidada a la
que se transferirá el saldo al realizar la consolidación. Ésta es una
asignación que permitirá que empresas con planes de cuentas distintos se
consoliden juntas.

<img class="img-container"  src="/assets/img/articles/consolidacion-contable/image2.png">
<br><br><br>

En esta pantalla, merece la pena comentar el campo de método traducción
consolidación.

**Método traduc. consolidación**

Este campo contiene el método de traducción de consolidación que se
usará para la cuenta. El método de traducción identifica el tipo de
cambio de divisa que se debe aplicar a la cuenta.

Las opciones posibles son:

- Tipo medio (manual) = Tipo medio del periodo que se va a consolidar.
  El tipo medio se calcula como promedio aritmético o como la mejor
  estimación y se introduce para cada empresa.

- Tipo cierre = Tipo de cambio en vigor en el mercado de divisas en la
  fecha para la que se está preparando el saldo o la regularización.
  El tipo de cambio se introduce para cada empresa.

- Tipo histórico = Tipo de cambio de la divisa extranjera en vigor en
  el momento de la transacción.

- Tipo compuesto = Los importes del periodo actual se traducen con el
  tipo medio y se agregan a saldo registrado anteriormente en la
  empresa consolidada. Este método se usa normalmente para cuentas
  de ingresos retenidos, porque contienen importes de distintos
  periodos y son, por ello, una combinación de importes convertidos
  con distintos tipos de cambio.

- Tipo neto = Es parecido al compuesto. El registro de las diferencias
  se hará en cuentas distintas.

Una vez se han introducido los números de cuenta de todas las cuentas y
se ha seleccionado el método de traducción de consolidación habrá que
repetir el procedimiento por cada y en cada una de las empresas
subsidiarias.

### Configurar la información de la empresa

Para consolidar los datos financieros de varias empresas en una empresa
consolidada, se debe introducir información acerca de las subsidiarias
como \"empresas\" que se van a incluir y acerca del nivel de inclusión
de las cifras.

La información de la empresa subsidiarias a consolidar debe configurarse
en la empresa consolidada dentro del menú Gestión financiera,
Contabilidad, Actividades Periódicas, Consolidación, Empresas.

### Ficha Empresa

Aparece la ventana Ficha de empresa:

<img class="img-container"  src="/assets/img/articles/consolidacion-contable/image3.png">
<br><br>

<img class="img-container"  src="/assets/img/articles/consolidacion-contable/image4.png">
<br><br><br>

Los campos a comentar son los siguientes:

**Cód. divisa**

Divisa local (DL) de la empresa. En blanco, se asume que los importes
del archivo de consolidación son en DL.

**Tabla de divisas**

El contenido de este campo indica qué tabla de tipos de cambio de divisa
utilizará el sistema al importar los datos de consolidación.

Las opciones son:

Local: Se usará la tabla de tipos de cambio de divisa de la empresa
actual (local).

Empresa: Se usará la tabla de tipos de cambio de divisa de la empresa.

**Consolidar**

Si está activado, la empresa se incluirá en la consolidación.

**% Integración**

Introduzca el porcentaje que indica la cantidad de cada cuenta de la
empresa que se va a incluir en la consolidación. Si deja el campo en
blanco, el sistema importará el 100% de cada cuenta de la empresa.

**Fecha inicial y Fecha final**

La Fecha inicial y la Fecha final del periodo para el que se importan
los datos. Se debe introducir las fechas inicial y final del ejercicio
de la empresa si no son las mismas que las de la empresa consolidada.

**Cta. bº neto ejerc. dif. pos. y Cta. bº neto ejerc. dif. neg.**

Cuenta de diferencias positivas y negativas de cambio del componente de
ajustes de conversión formado por todas las cuentas marcadas con el
método de traducción de consolidación \"Neto\".

Si se deja en blanco, se usarán las cuentas Cta. dif. positivas cambio y
Cta. dif. negativas cambio.

**Cuenta ajuste residual**

Si se producen importes residuales tras importar datos durante la
consolidación (como consecuencia de nuevos informes de saldo iniciales
de la hoja de saldo) el sistema los registra en esta cuenta.

**Cta. dif. pos. div. minorit. y Cta. dif. neg. div. minorit.**

Cuenta de diferencias positivas y negativas del componente de ajustes de
la conversión, que consta de la parte proporcional de los ajustes de
conversión totales que se deben atribuir a las partes minoritarias.

Si se deja en blanco, se usarán las cuentas Cta. dif. positivas cambio y
Cta. dif. negativas cambio.

### Especificar tipos de cambio para consolidaciones

Para el caso de las empresas subsidiarias en divisa extranjera, el
sistema utiliza tres tipos de cambio para consolidar sus saldos. Son
Tipo medio (manual), Tipo al cierre y Último cambio al cierre.

El Tipo de cambio medio (manual). Es el tipo de cambio medio en el
mercado de divisas en el periodo para la que se está preparando el saldo
o la regularización.

Tipo de cambio al cierre. Es el tipo de cambio en vigor en el mercado de
divisas en la fecha para la que se está preparando la consolidación o la
regularización.

Último cambio al cierre. Es el tipo usado en el último cierre del saldo.

Para especificar los tipos de cambio para consolidaciones, hay que
seleccionar la pestaña Navegar dentro de la ficha.

<img class="img-container"  src="/assets/img/articles/consolidacion-contable/image5.png">
<br><br><br>

### Configurar información de dimensiones para consolidaciones

Además de las cuentas contables, también se puede configurar información
de dimensiones.

Esta acción se realiza en el menú de Gestión financiera, Configuración,
Dimensiones, Dimensiones, Valores de dimensión

<img class="img-container"  src="/assets/img/articles/consolidacion-contable/image6.png">
<br><br><br>

El campo Cód. consolidación se utiliza para la consolidación. El que se
rellene o no el campo depende de si la información de dimensiones se va
a transferir a la empresa consolidada una vez realizada la
consolidación.

Si no se desea consolidar información de dimensiones, el campo se deberá
dejar en blanco.

El campo se debe informar cuando el código de valor de dimensión de la
empresa se vaya a consolidar con otro código de valor de dimensión en la
empresa consolidada. El campo deberá contener el código de valor de
dimensión de la empresa consolidada.

## Exportación de datos para consolidar

Si una empresa se encuentra en otra base de datos, es necesario exportar
los datos de consolidación a un archivo para poder consolidarlos. Cada
empresa deberá exportarse de forma independiente. Microsoft Navision
incluye un proceso para realizar la exportación.

El proceso se encuentra en Gestión financiera, Contabilidad, Actividades
Periódicas, Consolidación, Exportar consolidación

<img class="img-container"  src="/assets/img/articles/consolidacion-contable/image7.png">
<br><br><br>

Se debe seleccionar el formato de archivo en el que la empresa enviará
los datos. Si en la empresa se usa la versión 3.70 o anterior del
sistema, se enviarán en formato .txt. Si se usa la versión 4.00 o
posterior, enviarán un archivo .xml.

En el campo Cód. divisa ppal. se debe indicar el código de divisa de la
empresa principal.

Terminado el proceso, se genera un fichero con los movimientos
exportados. El fichero contiene los siguientes campos: N.º cuenta, Fecha
registro e Importe. Si se exportó también la información de dimensiones,
también se incluirán los códigos de dimensión y los valores de
dimensión.

Si el total de los campos Importe figura en él Debe, se exporta a cada
una de las líneas el número de cuenta que se configuró en el campo Cta.
consol. debe de la empresa. Si el total es un haber, se exporta a la
línea el número del campo Cta. consol. haber.

La fecha utilizada para cada línea exportada puede ser la fecha final
del periodo o, si se realiza la transferencia a diario, la fecha exacta
del cálculo.

El valor de dimensión exportado para el movimiento será el valor de
dimensión de la empresa consolidada que se configuró en el campo Cód.
consolidación de ese valor de dimensión. Si no se introdujo ningún valor
de dimensión de empresa consolidada en el campo Cód. consolidación para
ese valor de dimensión, se exporta simplemente el valor de dimensión a
la línea.

Los archivos XML, además, contienen los tipos de cambio de divisa del
periodo de consolidación. Dichos tipos de cambio se sitúan en una
sección independiente al principio del archivo.

## Comprobación de datos a consolidar

Antes de llevar a cabo la consolidación de los datos, conviene comprobar
si existen diferencias entre la información básica de las empresas y de
la empresa consolidada. Existen dos informes para llevar a cabo la
comprobación: uno para comprobar una base de datos y otro para comprobar
un archivo.

### Comprobar los datos de un archivo antes de realizar la consolidación

En el menú Gestión financiera, Contabilidad, Actividades Periódicas,
Consolidación, Empresas, Pestaña Acciones, Ejecutar la acción de Test
Archivo.

<img class="img-container"  src="/assets/img/articles/consolidacion-contable/image8.png">
<br><br><br>

El informe procesa los datos financieros de la empresa que se exportaron
al archivo y muestra las cuentas contables y las dimensiones que se
incluyeron en el archivo para la consolidación pero que no figuran en la
empresa consolidada.

Si en el resultado del proceso, se muestra una cuenta en el informe, es
que no se ha configurado en la empresa consolidada la cuenta de Debe o
la de Haber. Si, por el contrario, se muestra un valor de dimensión o
una dimensión, es por una de las siguientes razones:

- El código de consolidación que se configuró en la empresa no se
  corresponde con un valor de dimensión configurado en la empresa
  consolidada.

- Si no se utilizan códigos de consolidación, el código de valor de
  dimensión no se configura en la empresa consolidada.

- La dimensión del archivo no se ha configurado como dimensión en la
  empresa consolidada.

Deberán corregirse todos los errores antes de proceder a la importación
del archivo para la consolidación.

### Comprobar los datos de la base de datos antes de realizar la consolidación

La comprobación de los datos de empresa contenidos en la misma base de
datos que la empresa consolidada es similar a la combinación de los
pasos de exportación y comprobación del archivo descrito anteriormente.
Aunque no se crea ningún archivo, el informe generado es similar.

Para comprobar bases de datos antes de realizar la consolidación, en
punto de menú es el mismo que antes pero ahora ejecutamos la opción de
Test Base de Datos

<img class="img-container"  src="/assets/img/articles/consolidacion-contable/image9.png">
<br><br><br>

En la pestaña Código Empresa, se puede definir un filtro para
seleccionar las empresas que desea comprobar. Si no selecciona ningún
filtro, se comprobarán todas las empresas de la ventana Empresas cuyo
campo Consolidar esté activado.

## Consolidar los datos

El proceso de consolidar se puede realizar desde la misma base datos o
desde archivo. Describiremos el caso primero.

En el menú Gestión financiera, Contabilidad, Actividades Periódicas,
Consolidación, Empresas, Pestaña Acciones, Importar base de datos

<img class="img-container"  src="/assets/img/articles/consolidacion-contable/image10.png">
<br><br><br>

Se rellenan los campos y se clica en Aceptar para iniciar la
importación.

Si el periodo de consolidación no se incluye en el último ejercicio de
la empresa consolidada, el sistema mostrará un aviso donde se le
preguntará si desea continuar con la consolidación. Haga clic en Sí para
continuar.

El proceso lleva a cabo el procesamiento de todos los movimientos de la
empresa que se deben incluir en la consolidación. Una vez iniciado el
proceso, se mostrará en pantalla un mensaje de estado que indica la
empresa, el número de cuenta y la fecha que se están procesando.

Este proceso actualiza la tabla de movimientos directamente, no se crean
líneas de diario para registrarlos en la empresa consolidada. Los
movimientos registrados se consultan como un registro más de
contabilidad en Reg. Mov. Contab.

El total se transfiere a las cuentas de la empresa consolidada en base a
las cuentas configuradas en las subsidiarias como Cta. consol. debe y
Cta. consol. haber, dependiendo de si el total es un debe o un haber. Si
ya existen movimientos de la empresa en la empresa consolidada, se
sobrescribirán y se añadirá el siguiente texto: \"Modif. por consol. el
(fecha de trabajo)\".

Se utilizará para la generación de los informes Balance comprobación
consol. y Eliminaciones consolidación.

## Procesar las eliminaciones

Cuando se hayan consolidado todas las empresas, se debe realizar
movimientos de eliminación (movimientos para eliminar las transacciones
registradas en más de una empresa) o quitar los movimientos que
conlleven transacciones entre compañías.

Primero se deberán calcular las eliminaciones, de forma externa al
sistema y, a continuación, introducir los importes en un diario general.

### Introducir las eliminaciones en un diario general

Los movimientos de eliminación de consolidación relativos a estas
transacciones deben introducirse en la empresa consolidada en una
sección de diario cuyo campo Copiar conf. IVA a lín. día. esté
desactivado.

Manualmente, se deben introducir los registros pertenecientes a las
eliminaciones en un diario general, sección elegida tal y como se
realiza para el caso de un asiento de contabilidad general.

Una vez introducidos todas las líneas NO se registra el diario.

Antes de registrar las eliminaciones, se puede comprobar el efecto de la
empresa consolidada sobre el balance de comprobación mediante el informe
Eliminaciones consolidación. El informe muestra un balance de
comprobación provisional, es decir, muestra las consecuencias de la
eliminación de los movimientos por medio de la comparación de los
movimientos de la empresa consolidada con las eliminaciones que se
introdujeron en el diario general desde el que se registrarán más
adelante.

### Informe Eliminaciones

En el menú Gestión financiera, Contabilidad, Actividades Periódicas,
Consolidación, Empresas, Pestaña Informes, Eliminaciones.

<img class="img-container"  src="/assets/img/articles/consolidacion-contable/image11.png">
<br><br><br>

En esta ventana a parte de las Fechas de inicio - final y Código
Empresa, se debe indicar el Nombre libro diario y la Sección diario que
contiene las eliminaciones no registradas.

La opción de Muestra se activa si se desea que los importes se muestren
como Saldo periodo o como Saldo en la fecha actual.

El informe de Eliminaciones contiene la siguiente información.

Cada cuenta se muestra en una línea independiente (siguiendo la
estructura del plan de cuentas). La cuenta no se muestra cuando todos
los importes de la línea son 0. Se muestra la siguiente información para
cada cuenta:

- N.º cuenta

- Nombre cuenta

  - Si seleccionó un código de empresa en el campo Cód. empresa de
    la pestaña Opciones, se mostrará un total para la empresa
    consolidada que incluye las eliminaciones registradas, pero
    excluye los movimientos de consolidación y eliminaciones no
    registradas de la empresa seleccionada. Si dejó en blanco el
    campo Cód. empresa, se mostrará un total para la empresa
    consolidada que excluye las eliminaciones registradas y no
    registradas.

  - Si seleccionó un código de empresa en el campo Cód. empresa de
    la pestaña Opciones, se mostrará un total para los movimientos
    importados de la empresa. Si dejó en blanco el campo

- Cód. empresa, se mostrará un total para las eliminaciones
  registradas de la empresa consolidada.

El total de la empresa consolidada con todas las empresas y todas las
eliminaciones registradas.

Las eliminaciones no registradas que se deben llevar a cabo en la
empresa consolidada, es decir, los movimientos de la sección del diario
general que se seleccionó en la pestaña Opciones. Se muestra el total de
las eliminaciones no registradas en la parte inferior de todas las
páginas del informe.

El texto de registro copiado del diario general de eliminaciones no
registradas.

El total estimado de la empresa consolidada tras el registro de las
eliminaciones no registradas.

Si el total consolidado estimado de cada cuenta es razonable, podrá
proceder al registro de las eliminaciones no registradas. Por lo tanto,
el informe Eliminaciones consolidación puede considerarse como un test
de diario.

Finalmente, si todo es correcto, se registrará el diario con las
eliminaciones o bien se realizan los ajustes pertinentes en el diario y,
posteriormente, se vuelve a lanzar el informe de eliminaciones hasta
obtener el resultado deseado.

### Informes Consolidación

Una vez registradas las eliminaciones, se pueden imprimir los informes
de balance de comprobación consolidados para la empresa consolidada.

Existen dos informes.

**Balance comprobación consol.**

Este informe se recomienda para una empresa consolidada con más de
cuatro empresas. Contiene la información del saldo del periodo y de
saldo de cada cuenta antes y después de las eliminaciones junto con las
eliminaciones registradas de cada cuenta. Los totales de cada empresa
para una cuenta específica se muestran en líneas independientes.

<img class="img-container"  src="/assets/img/articles/consolidacion-contable/image12.png">
<br><br><br>

**Balance comprobación cons. (4)**

Este informe se recomienda para una empresa consolidada con un máximo de
cuatro empresas. La información de cada cuenta se muestra en una línea
individual. Los totales de cada empresa para una cuenta específica se
muestran en columnas independientes. Debido a los límites del ancho de
página, deberá seleccionar información de saldo o de saldo del periodo,
pero no ambos. La información que seleccione se muestra para cada cuenta
antes y después de las eliminaciones junto con las eliminaciones
registradas de cada cuenta.

Las opciones de los informes son:

**Fecha inicial y Fecha final**

Se introducen las fechas primera y última del periodo del que se
mostrarán los movimientos registrados de la empresa consolidada.

**Importes en miles**

Activar si desea que los importes del informe se muestren en miles.

**Muestra**

Se indica si se desea que los importes se muestren como Saldo periodo o
como Saldo en la fecha actual.
