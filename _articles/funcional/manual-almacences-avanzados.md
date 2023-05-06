---
title: Manual almacenes avanzados
summary: "Explicación detallada de la funcionalidad de almacenes avanzados"
layout: article
author: Esteve Sanpons
#cSpell:disable
category: [Funcional, Manuales, Almacen]
custom_type: Boveda
#cSpell:enable
date: 2022-08-22 07:29:00 +0200
---

###### Manual almacenes avanzados

<br>

## Configuraciones previas

### Ficha Almacén

Partiremos de la base de que este documento es para hacer almacenes avanzados por lo que marcaremos las opciones que salen en las capturas de pantalla. Si se diera el caso que el cliente quiere un almacén con algunas opciones de menos como podría ser el caso de un cliente que no quiere hacer documento de recepción este almacén pasaría a ser semi avanzado por el hecho de que le falta alguna de las opciones.

#### Almacén

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image1.png">
<br><br><br>

- Recepción requerida: Al recibir los productos en el almacén si hace falta el documento de recepción.

- Envió requerido: Especificas si al preparar un pedido se tiene que hacer con documentos de envió.

- Ubicación requerida: determinas si se va a trabajar con documentos de ubicación a la hora de recepcionar.

- Utilizar hoj. trab. ubicación: Si se ha marcado el punto anterior, es factible utilizar este. Al registrar una recepción, si no está marcada esta opción genera un documento de ubicación. Si está marcada, no la genera al registrar la recepción y se puede generar un solo documento de ubicación de todas las recepciones registradas.

- Picking requerido: Especifica si la preparación se hará con documentos de picking.

- Ubicac. obligatorio: Se determina si es obligatorio añadir en los documentos una ubicación para poder registrarlos. Al activar se habilita el campo Selección ubicación genérica.

- Ubicac. y pick. directo: Activa la funcionalidad de calcular las ubicaciones donde depositar el material, basándose en la plantilla de ubicar, definida en esta misma ficha del almacén y/o de la ficha de cada uno de los productos

- Selección ubicación genérica: determinas si la ubicación predeterminada es fija o se coge la última escogida.

- Tiempo manip. alm. Salida. Añade el tiempo indicado en este punto para el cálculo del servicio de los pedidos.

- Tiempo manip. alm. Entrada. Añade tiempo indicado en este punto, para el cálculo de la recepción de los pedidos de compra

- Calendario personalizado: Campo calculado, por si se ha variado el calendario base.

- Utilizar tránsito directo: Activa la funcionalidad, en las recepciones de transito directo. El transito directo permite ubicar producto que debe salir inmediatamente en las ubicaciones definidas para ello.

- Calcular fecha vto. tráns. directo: Esta fórmula de fecha, permite buscar que documentos son los que se pueden seleccionar para calcular el transito directo

#### Ubicaciones

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image2.png">
<br><br><br>

- Cód. ubicación recepción: especificas la ubicación predeterminada que se utilizara para recepcionar (muelle de entrada)

- Cód. ubicación envío: especificas la ubicación predeterminada que se utilizara para enviar. (Muelle de salida)

- Abre ubic. aprovision. Manual. Ubicación donde se deben situar los productos que se consuman automáticamente en la producción

- Cód. ubic. para producción. Ubicación donde se deben ubicar los productos que se deben consumir en la producción, manualmente.

- Cód. ubic. desde producción. Ubicación donde se sitúan los productos salidos de una orden de producción.

- Cód. ubicación ajuste: especificas la ubicación de ajustes, en esta ubicación es donde estarán todos tus ajustes negativos y positivos. Esta ubicación tendría que estar siempre a 0.

- Cód. ubicac. tránsito directo. Es la ubicación predeterminada donde propondrá ubicar los productos que deben salir inmediatamente.

- Cód. ubic. para ensamblado. Ubicación, donde se situarán los productos que se han de consumir para realizar un ensamblado.

- Cód. ubic. desde ensamblado. Ubicación de los productos que salen de un pedido de ensamblado.

- Cód. ubic. ens. contra-pedido. Solo se utiliza en la configuración de un almacén semi avanzado. Es donde se sitúan los productos ensamblados de un pedido de ensamblado para un pedido de venta.

#### Directivas ubicación

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image2.png">
<br><br><br>

Solo se activa la pestaña si el almacén es avanzado.

- Equipo especial: Determinas si se necesita algún equipo especial

- Directiva capacidad ubicación: Indica el control que realiza el sistema sobre las capacidades de las ubicaciones donde se ubicarán los productos.

- Permite división bulto: Si se permite dividir un bulto dentro del almacén. Si tenemos un producto ubicado con la UM PALET y la base son UNIDADES, indica que se puede romper los pallets para obtener unidades en un picking

- Cód. plantilla ubicar: Escogeremos la plantilla de las configuraciones de las políticas que utilizaremos para nuestros picking y documentos de ubicación.

- Crea siempre lín. Ubicación: Crea la línea de la ubicación, aunque no encuentre lugar donde poder ubicar o colocar los productos.

- Crea siempre lín. Picking: Crear líneas de picking, aunque no se encuentre lugar de donde poder escoger o sacar los productos.

- Picking según FEFO (Primero en caducar, primero en salir)

#### Plantillas ubicar

Aquí configuraremos las plantillas para las configuraciones de las políticas que utilizaremos para nuestros documentos de ubicación.

Ejecuta, secuencialmente, las condiciones indicadas, hasta que encuentra una que cumpla

Lo aconsejable es que en la última pongamos al menos la búsqueda de ubicación aleatoria.

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image4.png">
<br><br><br>

- Busca ubicación fija: Primero busca la ubicación prefijada en el producto.

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image5.png">
<br><br><br>

- Busca ubicación aleatoria: Busca una ubicación aleatoria.

- Busca mismo producto: Busca una ubicación donde haya el mismo producto.

- Busca misma unidad medida: Igual, pero con la misma unidad de medida de la ubicación

- Busca ubic. menor cdad. mín.: en la configuración de contenidos ubicación si lo tienes configurado buscara la que tenga con el mínimo configurado.

- Busca ubicación vacía: busca la primera ubicación vacía.

#### Zonas

El almacén se divide en zonas que cumplen unas determinadas características para determinadas tareas de almacén. Serían subdivisiones del almacén en partes lógicas.

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image6.png">
<br><br><br>

Los valores asignados a una zona **_NO_** los heredan las ubicaciones creadas en esa zona.

- Cód. tipo ubicación: para determinar qué tipo de ubicación pondremos. Mas adelante se explica con más detalle.

- Cód. clase almacén: Son clasificaciones de la zona, podría ser una nevera y poner frio o una zona para productos peligrosos. Solo aceptará productos que tengan asignada la misma clase. Si se definen clases, se deben generar ubicaciones con esta clase de tipo:

  - Recepción

  - Envío

  - Transito directo

- Cód. equipo especial: Que tipo de maquinaria podemos utilizar en cada zona. Se puede configurar también en el producto.

- Ranking zona: Se utiliza para los pickings, primero busca en las ubicaciones de mayor ranking.

- Zona ubicac. tráns. Directo: Es una zona en la que se ubicarán los productos que deben salir inmediatamente.

#### Ubicaciones

La ubicación es la unidad más pequeña en la que se pueden colocar y registrar productos. Es donde ubicaremos nuestra mercancía dentro del almacén.

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image7.png">
<br><br><br>

- Cód. zona: zona del almacén a la que pertenece la ubicación.

- Cód. tipo ubicación: para determinar qué tipo de ubicación pondremos. Mas adelante es explica con más detalle.

- Movimiento bloqueado: Si este campo está activado el sistema no permitirá el registro de movimientos en esta ubicación. El bloqueo puede hacerse para todos los tipos de movimiento, para los de entrada o para los de salida.

- Cubicaje máximo: Determina la capacidad máxima (en volumen) de la ubicación.

- Peso máximo: Determina la capacidad máxima (en peso) de la ubicación.

- Vacío: Indica que está vacía.

- Cód. equipo especial: Que tipo de maquinaria podemos utilizar en cada ubicación. Se puede configurar también en el producto.

- Cód. clase almacén: Son clasificaciones de la zona, podría ser una nevera y poner frio o una zona para productos peligrosos. Solo aceptará productos que tengan asignada la misma clase. Si se definen clases, se deben generar ubicaciones con esta clase de tipo:

  - Recepción,

  - Envío

  - Transito directo

- Ubicación transito directo: Si la ubicación es de tránsito directo.

El botón contenido sirve para ver el contenido que hay en cada ubicación.

Tipos de ubicación

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image8.png">
<br><br><br>

La configuración de tipos de ubicación se indica qué acciones se permiten hacer y cuáles no en el tipo de ubicación correspondiente.

- Recepción: tipo solo para hacer recepciones. Esta opción solo se puede marcar sola.

- Envío: tipo solo para enviar. Esta opción solo se puede marcar sola.

- Ubicación: tipo para ubicar

- Picking: tipo para los picking.

### Empleados de almacén

Aquí determinaremos que empleados y en que almacenes tiene opción de trabajar. Se ha de configurar todos los almacenes a los cuales un usuario ha de tener acceso.

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image9.png">
<br><br><br>

En uno de ellos se ha de marcar el campo genérico. El sistema entiende que ese es el habitual del empleado y siempre se lo presenta como predeterminado.

### Configuración inventario

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image10.png">
<br><br><br>

Marcar este campo es muy importante, dado que, si no esté marcado nos deja hacer movimientos con almacén en blanco, de esta manera obligamos a informar el almacén.

### Configuración almacén

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image11.png">
<br><br><br>

Aquí lo único que nos afectaría son los dos campos de directiva que deberían tener en la opción de Parar y mostrar el primer error de registro.

Así al hacer los documentos nos mostrar los errores cuando los registremos.

## Circuito de compras

Después de haber hecho el pedido de compra de tenerlo lanzado crearemos el documento de recepción.

### Recepción almacén

El documento de recepción almacén que recopila todos nuestros pedidos para ser recepcionados todos juntos o por separado.

Hay dos maneras de crear este documento.

### 1 pedido 1 recepción

Después de lanzar el pedido vamos a acciones y hay un botón para crear una recepción almacén del pedido actual.

Si el pedido tuviera en sus líneas más de un almacén crearía una recepción almacén por cada almacén, juntando todas las líneas de ese almacén

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image12.png">
<br><br><br>

Dentro de la recepción almacén veremos una cabecera y unas líneas donde podremos modificar algunos de los campos para acabar de afinar nuestra recepción.

Cuando lo tenemos todo rellenado o revisado, registraremos y se creara la ubicación, siempre que no se haya marcado que el almacén utiliza la hoja de trabajo de ubicación.

#### Multitud de pedidos 1 recepción

Para crear un único documento de recepción almacén con muchos pedidos dentro para poder agruparlos, nos tendríamos que ir a la lista y darle a nuevo.

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image13.png">
<br><br><br>

Añadimos el almacén en el documento. Añadimos todos los datos necesarios del documento y le damos al botón de traer doc. origen.

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image14.png">
<br><br><br>

En esta ventana seleccionaremos todos los pedidos que queremos añadir en nuestra recepción almacén. Solo se ven los pedidos que se han lanzado. Si no están lanzados no se verán en esta lista.

Cuando lo tenemos todo rellenado o revisado, registraremos y se creara la ubicación.

### Documento de ubicaciones

Este documento lo que hace es decirle que ubicamos los productos desde la recepción a una ubicación en concreto.

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image15.png">
<br><br><br>

Después de rellenar donde lo queremos ubicar y la cantidad a mover registramos la ubicación, siempre que no se haya marcado que el almacén utiliza la hoja de trabajo de ubicación.

#### Crear de nuevo el documento de ubicación

Si se diera que el documento de ubicación no se ha creado, pero si se ha registrado la recepción y el producto estuviera en la recepción. Podemos crear de nuevo el documento de ubicación.

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image16.png">
<br><br><br>

- Crear ubicación: si no hay ni ubicaciones, ni ubicaciones registradas y el producto está en la recepción, podremos generar de nuevo el documento de ubicación pendiente de registrar

- Líneas ubicación: aquí podemos ver las líneas de ubicación que hay relacionadas con nuestra recepción registrada.

- Líns. ubicaciones registradas: Aquí podemos ver si hay ubicaciones registradas. Si hay alguna no se podrá generar de nuevo la ubicación.

#### Dividir línea ubicaciones

Tenemos la opción de dividir la línea por si queremos ubicar parte de la cantidad en un lote o ubicación y parte en otro.

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image17.png">
<br><br><br>

Cambiar la unidad de medida nos permite ubicar el producto en otra UM definida en la ficha del producto.

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image18.png">
<br><br><br>

## Circuito ventas

Como en todo circuito de ventas empezaríamos con una posible oferta siguiendo con su pedido de venta.

Al tener el pedido de venta acabado y lanzado podremos disponer de crear nuestro envió almacén.

### Envió almacén

Él envió almacén es el documento que recopila todos nuestros pedidos para ser enviados todos juntos o por separado con un mismo servicio de transporte.

Hay dos maneras de crear él envió almacén.

#### 1 pedido 1 envió

Después de lanzar el pedido vamos a acciones y hay un botón para crear un envió almacén del pedido actual.

Si el pedido tuviera en sus líneas más de un almacén crearía un envió almacén por cada almacén, juntando todas las líneas de ese almacén.

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image19.png">
<br><br><br>

Dentro del envió almacén veremos una cabecera y unas líneas donde podremos modificar algunos de los campos para acabar de afinar nuestro envió.

Cuando lo tenemos todo rellenado o revisado, crearemos el picking. El picking solo lo creará de los productos que tenga stock, a no ser que en la configuración del almacén se haya indicado que siempre se crea picking, dejando la ubicación en blanco.

#### Multitud de pedidos 1 envió

Para crear un único envió almacén con muchos pedidos dentro para poder agruparlos, nos tendríamos que ir a la lista de envíos almacén y darle a nuevo.

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image20.png">
<br><br><br>

Añadimos el almacén en él envió almacén. Añadimos todos los datos necesarios del documento y le damos al botón de traer doc. origen.

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image21.png">
<br><br><br>

En esta ventana seleccionaremos todos los pedidos que queremos añadir en nuestro envió almacén. Los pedidos que se muestran son los que estén lanzados.

Cuando lo tenemos todo rellenado o revisado, crearemos el picking. El picking solo lo creará de los productos que tenga stock, a no ser que en la configuración del almacén se haya indicado que siempre se crea picking, dejando la ubicación en blanco.

### Picking

En él envió almacén tenemos una opción que es la de crear picking, al clicarla nos creara el picking del envió almacén en el que estemos.

Existe otra opción para generar pickings, la opción Preparar hoja trabajo pedido, que nos permite seleccionar todos los pedidos de venta, ensamblado, transferencia o devoluciones a proveedor y generar un documento de picking

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image22.png">
<br><br><br>

Para acceder al picking tendremos que ir a la acción de líneas de picking y des de allí darle a la ficha para poder abrir el picking

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image23.png">
<br><br><br>

En el picking rellenaremos las cantidades a enviar, el lote, la ubicación si hicieran falta cambiar tanto de la línea del traer como la del colocar.

#### Dividir línea picking

Tenemos la opción de dividir la línea por si tenemos parte de la cantidad en un lote o ubicación y parte en otro.
<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image17.png">
<br><br><br>

#### Cambiar Unidad de medida

Al igual que en las ubicaciones, en los pickings tenemos la posibilidad de cambiar la unidad de medida en el picking, de las que tiene definido el producto en su ficha.

#### Registros picking y envió

Al acabar registraremos el picking y después si no hay nada más que añadir rellenaremos las cantidades a enviar del envió almacén y registraremos él envió.

Al registrar él envió se generará un albarán de venta, Albarán de entrega de devolución de proveedor o el albarán de entrega de un pedido de transferencia.

Al registrar él envió se puede generar automáticamente la factura si así lo requieres y el documento lo requiere.

## Pedidos de transferencia

Los pedidos de transferencia se utilizan sobre todo para hacer movimientos de productos entre los almacenes.

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image24.png">
<br><br><br>

Si los almacenes son avanzados tendremos que hacer en el almacén de envió el, envió almacén y el picking, y en el almacén de recepción, la recepción y la ubicación.

## Diarios

### Diarios reclasificación almacén

Este diario lo podemos utilizar para el traslado de productos entre ubicaciones del mismo almacén o el cambio del número de lote o número de serie

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image25.png">
<br><br><br>

### Diario producto almacén

Este diario lo podemos utilizar para ajuste positivos y negativos en los almacenes avanzados.

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image26.png">
<br><br><br>

Después tendremos que pasar el cálculo de ajustes de almacén que está en el diario de producto.

### Diario producto (calcular ajustes almacén)

Esta funcionalidad lo que hará es calcular todos los ajustes pendientes que quedan en los almacenes y te montara el diario de producto con esas líneas.

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image27.png">
<br><br><br>

### Diarios de inventario físico almacén

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image28.png">
<br><br><br>

Este diario sirve para hacer los inventarios de los almacenes avanzados.

Si clicamos en el botón de calcular inventario nos añadirá las líneas de inventario que hay en el almacén.

En este proceso podemos filtrar por zona, ubicación, producto.... Así podemos acotar el inventario que queremos hacer.

## Hojas de trabajo

Las hojas de trabajo nos ayudan a traer o agrupar los documentos de movimiento, ubicación y picking.

### Hoja trabajo movimiento

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image29.png">
<br><br><br>

Trae todo el contenido de una ubicación en concreto para poder hacer un movimiento de almacén a otra ubicación.

### Hoja de trabajo ubicación

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image20.png">
<br><br><br>

Esta hoja de trabajo lo que podremos crear son des de las líneas de recepción registradas, un documento de ubicación del almacén.

### Preparar hoja trabajo pedido

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image21.png">
<br><br><br>

Esta hoja de trabajo lo que podremos crear son des de las líneas de envió pendientes de enviar y que no están en un picking, un documento de picking del almacén.

## Contenido ubicación

Es una ventana de consulta donde podemos visualizar el contenido que hay en cada ubicación/producto.
<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image22.png">
<br><br><br>

Además, también podemos ver las cantidades que hay de ese producto/ubicación añadidas a un picking o en ajustes o en ubicaciones....

## Cambiar el almacén avanzado de estándar ha avanzado

Este proceso estándar lo que hace habilitar una ubicación del almacén para añadir todo el inventario existente, es necesario después hacer un inventario físico para saber dónde estará cada producto en cada ubicación.

Este proceso muchas veces da errores y no podemos llegar a completar el cambio de tipo de almacén.

<img class="img-container"  src="/assets/img/articles/manual-almacenes-avanzados/image23.png">
<br><br><br>

### Método extra

Este método lo que hace es lo mismo, pero más fiable.

- Si hay, todos los documentos pendientes del almacén se tienen que eliminar. Los envíos, las recepciones, ubicaciones, picking y diarios.

- Haces un diario de inventario poniendo todo el stock a 0.

- Marcar las configuraciones de almacén para hacerlo avanzado.

- Hacer un diario de producto almacén con todas las líneas en sus ubicaciones, se pueden añadir las líneas con una plantilla de RapidStart. (Aquí el cliente tendría que hacer un inventario en su almacén para saber dónde este cada producto en cada ubicación)

- Hacer el calculo de ajustes del diario de producto.
