---
title: Manual Producción
summary: "Explicación detallada de la funcionalidad del módulo de producción"
layout: article
author: Esteve Sanpons
#cSpell:disable
category: [Funcional, Manuales, Produccion]
custom_type: Boveda
permalink: /boveda/manual-produccion
#cSpell:enable
date: 2022-08-22 09:00:00 +0200
---

###### Manual Producción

<br>

## Configuraciones previas

### Producto

La ruta dentro del menusuite es: Departamentos/ Fabricación/ Diseño de productos/ Productos

Reposición

<img class="img-container"  src="/assets/img/articles/manual-production/image1.jpg">
<br><br><br>

- Sistema reposición: "Prod. Pedido" esto lo que hace es configurar este producto para que con la hoja de planificación puedas crear ordenes de producción.

- Directivas fabricación: te está diciendo como lo fabricaras si por stock o por pedido.

  - Fab-contra-stock: Fabricas solo para stock

  - Fab-contra-pedido: fabricas solo para los pedidos de venta que te piden.

- N.º Ruta: Le especificamos la ruta que tenemos que hacer. Entendemos ruta por operaciones y departamentos de fabrica que pasara.

- N.º L.M. producción: La lista de materiales es la lista de componentes que se necesitan para crear el producto final.

- Precisión redondeo: esto redondeara las cantidades a lo que tengamos configurado aquí. Importante si son unidades poner un 1. En la parte de producción si consumes este producto lo que hace es redondear las cantidades en o que ponemos aquí.

- Método de baja: esto es como se consumirá el componente

  - Manual: Se tiene que hacer manualmente a través de un diario.

  - Adelante: En el momento que se lanza la orden de fabricación se consume los componentes que están marcados de este modo.

  - Atrás: Cuando hemos acabado la fase donde se utiliza el producto, hará los consumos o cuando cierre la orden de fabricación.

- \% Rechazo: es el porcentaje de rechazo de la fabricación de este producto. Normalmente esto de informa en la lista de materiales. Este campo no se suele utilizar.

### Planificación

<img class="img-container"  src="/assets/img/articles/manual-production/image2.jpg">
<br><br><br>

- Cantidad mínima pedido: Si en la hoja de planificación te dice que has de fabricar alguna cosa, la cantidad mínima te dirá que es lo mínimo a fabricar.

- Cantidad máxima pedido: No se pueden hacer ordenes de fabricación con una cantidad máxima de la que este informada. Por lo que hará es hacer tantas ordenes como sean necesaria, pero con el límite que se ponga aquí.

### L.M. producción

La ruta dentro del menusuite es: Departamentos/ Fabricación/ Diseño de productos/ L.M. producción

#### Cabecera

<img class="img-container"  src="/assets/img/articles/manual-production/image3.jpg">
<br><br><br>

- Estado: En el estado de la cabecera si está certificada es que la puedes utilizar. Si está en otro estado no puedes utilizarla.

- Cód. unidad medida: puedes configurar cualquier unidad de medida ya que solo se puede poner cantidad hasta 5 decimales. Puede ser diferente a la unidad de medida base per siempre tiene que estar en la configuración de la unidad de medida del producto.

#### Líneas

<img class="img-container"  src="/assets/img/articles/manual-production/image4.jpg">
<br><br><br>

- Cantidad por: Cantidades necesarias para fabricar el producto padre.

- Cód. unidad de medida: es igual que en la cabecera.

- \% Rechazo: Es el porcentaje que en esta lista de materiales perderá de los diferentes productos.

- Cód. conexión ruta: es un código para enlazar con la ruta para que este producto que este configurado en la ruta configurada, no requiera el producto hasta el momento de la ruta configurada. Si no lo informas estas diciendo que todo lo requieres en la primera fase de la ruta.

### Ruta

La ruta dentro del menusuite es: Departamentos/ Fabricación/ Diseño de productos/ Rutas

#### Cabecera

<img class="img-container"  src="/assets/img/articles/manual-production/image5.jpg">
<br><br><br>

- Estado: En el estado de la cabecera si está certificada es que la puedes utilizar. Si está en otro estado no puedes utilizarla.

- Tipo: Manera distinta de producir.

  - En serie: Va en orden empieza por la primera fase y acaba por la ultima.

  - En paralelo: Se pueden hacer varias fases a la vez, se requiere jugar con los campos N.º operación siguiente y N.º operación anterior.

#### Líneas

<img class="img-container"  src="/assets/img/articles/manual-production/image6.jpg">
<br><br><br>

- Tipo:

  - Centro trabajo: Conjunto de centros máquina de la misma tipología.

  - Centro máquina: Es la máquina para hacer la fase.

- Tiempo preparación: Tiempo necesario para la preparación de la fase de la ruta.

- Tiempo ejecución: Tiempo de ejecución que se tardara en esta fase.

- Tiempo espera: El tiempo de espera entre fase y fase.

- Tiempo movimiento: Es el tiempo que se requiere para llevar entre fases.

- \% Factor rechazo: Es el rechazo que tendrá esta ruta todos los productos que estén en ella.

- Cdad. A adelantar: Te sirve para adelantar. Te sirve para saber cuántas unidades se requiere para empezar la siguiente fase.

- Cód. conexión ruta: Aquí es donde especificaremos el código de conexión entre la lista de materiales y la ruta. Si no lo informas estas diciendo que todo lo requieres en la primera fase de la ruta.

- N.º operación siguiente: Es para decirle que fase es la siguiente después de acabar la actual.

- N.º operación anterior: Es para decirle que fase es la anterior.

- Cód. ud. Medida tiempo: aquí hay varios campos que son configurables para determinar el tiempo que hemos configurado arriba en que tiempo es, si minutos, horas, días.

### Unidades medida capacidad

La ruta dentro del menusuite es: /Departamentos/ Fabricación/ Administración/ Configuración/ Unidades de medida capacidad

Aquí es donde especificaremos las unidades de tiempo necesarias para las rutas.

<img class="img-container"  src="/assets/img/articles/manual-production/image7.jpg">
<br><br><br>

### Turnos trabajo

La ruta dentro del menusuite es: /Departamentos/ Fabricación/ Administración/ Configuración/ Turnos trabajo

Especificaremos todos los turnos posibles que tendrá nuestra empresa.

<img class="img-container"  src="/assets/img/articles/manual-production/image8.jpg">
<br><br><br>

### Calendarios planta

La ruta dentro del menusuite es: /Departamentos/ Fabricación/ Administración/ Configuración/ Calendarios planta

Especifica los calendarios de la planta de fabricación.

<img class="img-container"  src="/assets/img/articles/manual-production/image9.jpg">
<br><br><br>

### Días laborables

Asignas los días laborables y sus horarios

<img class="img-container"  src="/assets/img/articles/manual-production/image10.jpg">
<br><br><br>

### Vacaciones

Se especifica los días de vacaciones que no se trabajaran en el horario especificado.

<img class="img-container"  src="/assets/img/articles/manual-production/image11.jpg">
<br><br><br>

### Grupo centro trabajo

La ruta dentro del menusuite es: /Departamentos/ Fabricación/ Administración/ Configuración/ Grupo centro trabajo

Creas los grupos de centro de trabajo que tendrá tu empresa. Estos son agrupaciones o de centros maquina o de centros de trabajo.

<img class="img-container"  src="/assets/img/articles/manual-production/image12.jpg">
<br><br><br>

### Centro trabajo

La ruta dentro del menusuite es: Departamentos/ Fabricación/ Capacidades/ Centro trabajo

Es la configuración de las agrupaciones de centros maquina de una zona en concreto de la planta de fabricación.

<img class="img-container"  src="/assets/img/articles/manual-production/image13.jpg">
<br><br><br>

#### General

- Cód. grupo centro trabajo: Asignas el grupo de centro de trabajo al cual pertenecerá este centro de trabajo.

- Alternar centro trab.: si no se pudiera utilizar este centro de trabajo le puedes indicar que otro centro de trabajo se puede coger como alternativo.

#### Registro

- Calculo coste unitario: Se puede valorar o por tiempo o por unidades.

- Costes unit. directo: es para especificar el precio / calculo coste directo que se le asignara al centro de trabajo.

- \% Coste indirecto:

- Tasa costes generales: Es para imputarle una parte de la estructura tales como la nave o la zona de fabricación.

- Coste unitario: Es el cálculo total de los campos antes mencionados.

- Coste unitario especifico: es solo para centros de trabajo que sean de subcontratación, normalmente el coste no lo tendrás en el centro si no en la ruta.

- Método de baja: es para definir como damos de baja los productos.

  - Manual: Se tiene que hacer manualmente a través de un diario.

  - Anticipada: En el momento que se lanza la orden de fabricación se consume los componentes que están marcados de este modo.

  - Retroactiva: Cuando hemos acabado la fase donde se utiliza el producto, hará los consumos o cuando cierre la orden de fabricación.

- Grupo contable producto: Es para hacer toda la parte de asientos contables de producción.

#### Programación

- Cód. unidad medida: aquí puedes asignarle el código de tiempo del centro de trabajo.

- Capacidad: Le dices cuantas maquinas tienes en el centro de trabajo.

- Eficiencia: Es para determinar cuanta eficiencia tiene en tiempo el centro de trabajo. Si somos un cien por cien eficientes pues ponemos un 100.

- Calendario consolidado: Si esta opción está marcada lo que hará es sumar los calendarios de los centros maquina asignados al centro de trabajo para que ese sea su calendario. Si no está marcado lo que tiene que coger es lo que este parametrizado en el campo Cód. calend. planta.

- Cód. calend. planta: aquí se configura el calendario del centro de trabajo, solo si no está marcado el campo calendario consolidado.

### Centro de maquina

La ruta dentro del menusuite es: Departamentos/ Fabricación/ Capacidades/ Centros máquina

Sería la configuración de una maquina u operario que normalmente está vinculado a un centro de trabajo.

<img class="img-container"  src="/assets/img/articles/manual-production/image14.jpg">
<br><br><br>

#### General

- N.º centro trabajo: especificamos a que centro de trabajo pertenece este centro máquina.

#### Registro

- Costes unit. directo: es para especificar el precio del centro de trabajo

- \% Coste indirecto:

- Tasa costes generales: Es para imputarle una parte de la estructura tales como la nave o la zona de fabricación.

- Coste unitario: Es el cálculo total de los campos antes mencionados.

- Método de baja: es para definir como damos de baja los productos.

  - Manual: Se tiene que hacer manualmente a través de un diario.

  - Anticipada: En el momento que se lanza la orden de fabricación se consume los componentes que están marcados de este modo.

  - Retroactiva: Cuando hemos acabado la fase donde se utiliza el producto, hará los consumos o cuando cierre la orden de fabricación.

- Grupo contable producto: Es para hacer toda la parte de asientos contables de producción.

#### Programación

- Cód. unidad medida tiempo cola: aquí puedes asignarle el código de tiempo del centro máquina.

- Capacidad: Le dices cuantas maquinas tienes en el centro máquina.

- Eficiencia: Es para determinar cuanta eficiencia tiene en tiempo el centro máquina. Si somos un cien por cien eficientes pues ponemos un 100.

#### Configuración ruta

Aquí puedes configurarles los tiempos predeterminados en la fase de la ruta.

## Ordenes de producción

El circuito suele empezar con las hojas de planificación, aunque podemos crear una O.P planificada manualmente.

### Hoja de planificación (_MRP_)

La ruta dentro del menusuite es:Departamentos/ Fabricación/ Planificación/ Tareas/ Hoja planificación

<img class="img-container"  src="/assets/img/articles/manual-production/image15.jpg">
<br><br><br>

<!---
"FALTA EXPLICAR ESTA PARTE"
-->

### O.P. Planificadas

La ruta dentro del menusuite es:Departamentos/ Fabricación/ Ejecución/ O.P. Planificadas

Habitualmente en esta ventana no aparecerá nada, porque cada vez que abres la hoja de planificación se borran todas las O.P. planificadas.

Se pueden crear las O.P planificadas manualmente, pero se aconseja hacerlo a través de las hojas de planificación.

Cuando queramos cambiar el estado de la orden solo tendremos que presionar el botón de cambiar estado y seleccionar el estado a cambiar.

<img class="img-container"  src="/assets/img/articles/manual-production/image16.jpg">
<br><br><br>

### O.P. Planificadas en firme

La ruta dentro del menusuite es:Departamentos/ Fabricación/ Ejecución/ O.P. Planificadas en firme

Al pasarlo a estado planificado en firme lo que estamos haciendo es lanzar todos los pedidos de compra necesarias de las listas de materiales de la O.P. planificada en firme.

Este estado es un estado volátil, aun puedes modificar datos dentro de la orden.

Si tenemos ordenes planificadas en firme no tendrá presente estos productos a la hora de plantear nuevas ordenes en la hoja de planificación.

#### Cabecera

<img class="img-container"  src="/assets/img/articles/manual-production/image17.jpg">
<br><br><br>

_Botón Actualizar orden producción_

Si quieres crear las líneas, si quieres crear las rutas y si quieres crear los componentes de la orden o actualizarlos tendríamos que presionar este botón.

#### General

- Descripción: es la descripción del producto a fabricar

- Tipo procedencia mov.:

  - Producto: En este tipo lo que haremos es indicar el producto a fabricar

  - Familia: A la hora de fabricar se pueden fabricar mas de un producto a la vez

  - Cab. Venta: Se hace una orden de producción des de un pedido de venta, hay una acción en el pedido de venta que lo que hace es generarle una orden de producción de ese pedido.

- Cód. procedencia mov.: número del producto, familia o pedido a fabricar.

- Cantidad: Es la cantidad que queremos fabricar.

- Fecha de vencimiento: Aquí se especifica en que momento tiene que estar como máximo fabricada la orden.

#### Previsión

Aquí especificaremos las horas y días que se iniciara y que finalizara la orden.

#### Registro

Especificamos los datos para tener en cuenta a la hora de registrar dentro de nuestra orden de producción.

#### Líneas

_Botones de ruta y componentes_

Des de estos botones podremos ver la ruta y lo componentes que hay para esta orden en concreto.

<img class="img-container"  src="/assets/img/articles/manual-production/image18.jpg">
<br><br><br>

Cuando queramos cambiar el estado de la orden solo tendremos que presionar el botón de cambiar estado y seleccionar el estado a cambiar.

<img class="img-container"  src="/assets/img/articles/manual-production/image19.jpg">
<br><br><br>

### O.P. Lanzadas

La ruta dentro del menusuite es:Departamentos/ Fabricación/ Ejecución/ O.P. Lanzadas

El funcionamiento es igual que en las O.P. planificadas en firme con la diferencia que en las líneas está el diario de producción.

Se explicará este diario en el apartado de diarios.

<img class="img-container"  src="/assets/img/articles/manual-production/image20.jpg">
<br><br><br>

### O.P. Terminadas

La ruta dentro del menusuite es:Departamentos/ Fabricación/ Archivo/ Historial / O.P. Terminadas

Este es el histórico de las ordenes de producción.

Aquí podremos ver que es lo que se ha hecho y fabricado en cada orden de producción.

<img class="img-container"  src="/assets/img/articles/manual-production/image21.jpg">
<br><br><br>

### Diarios

Diario de producción

Este diario lo podemos encontrar en las líneas de las ordenes lanzadas

El diario de producción sirve para registrar los consumos de los productos y le dices a cada una de las operaciones lo que estas tardando realmente.

La última línea es la que te dirá la cantidad de producto que as creado con esta orden. La única línea que es obligatoria registrar antes de cerrar una O.P. lanzada es esta, las otras no son obligatorias, aunque son necesarias si queremos que los almacenes nos cuadren, puesto que no se harán loas ajustes en los productos de nuestra orden.

Solo se pueden hacer ajustes en las ordenes lanzadas, una vez hemos terminado la orden no se podrán hacer estos ajustes.

<img class="img-container"  src="/assets/img/articles/manual-production/image22.jpg">
<br><br><br>

### Diario consumo

La ruta dentro del menusuite es: Departamentos/ Fabricación/ Ejecución/ Tareas/ Diario consumo.

Este diario es otra manera de consumir los productos de las O.P. lanzadas.

Para traer las líneas de las ordenes lo haremos desde el botón de calcular consumo.

Es lo mismo que el diario de producción, pero no se pueden registrar las operaciones hechas en la orden de producción. Podemos traer varias ordenes y registrarlas todas juntas.

<img class="img-container"  src="/assets/img/articles/manual-production/image23.jpg">
<br><br><br>

### Diario salida

La ruta dentro del menusuite es: Departamentos/ Fabricación/ Ejecución/ Tareas/ Diario salida

Este diario es otra manera de registrar las operaciones de las O.P. lanzadas.

Para traer las líneas de las ordenes lo haremos desde el botón de desplegar ruta.

Es lo mismo que el diario de producción, pero no se pueden registrar los consumos de producto hechos en la orden de producción.

<img class="img-container"  src="/assets/img/articles/manual-production/image24.jpg">
<br><br><br>
