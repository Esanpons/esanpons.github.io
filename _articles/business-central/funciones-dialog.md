---
title: "Potencia tus Aplicaciones en Business Central y Navision con Funciones de Interacción"
summary: "Descubre cómo utilizar las poderosas funciones Confirm, Error, Message y STRMENU en Business Central y Navision para mejorar la interacción con tus usuarios y proporcionar información relevante. Aprende a combinar estas funciones para crear una experiencia de usuario más dinámica y personalizada en tus aplicaciones empresariales."
layout: article
#cSpell:disable
category: ["Business_Central", Navision]
custom_type: Boveda
permalink: /boveda/funciones-dialog
#cSpell:enable
author: Esteve Sanpons
date: 2023-07-21 02:00:00 +0200
LinkedIn: false
---

Este articulo esta dedicado a las funciones Confirm, Error, Message y STRMENU en el lenguaje de programación Business Central y Navision. Estas funciones son herramientas imprescindibles para interactuar con los usuarios y proporcionar información relevante en tus aplicaciones.

<br>

## Función Confirm

Documentación oficial en el [Link](https://learn.microsoft.com/en-us/dynamics-nav/confirm-function--dialog-).

Cuando surgen errores en tu aplicación, es esencial informar al usuario de manera clara y concisa. Para esto, la función Error entra en juego. Esta función muestra un mensaje de error en un cuadro de diálogo que obliga al usuario a hacer clic en "Aceptar" para cerrarlo. Así, le informarás sobre cualquier problema que afecte la operación actual:

```javascript
    procedure EjecuteConfirm() ReturnValue: Boolean
    begin
        If Confirm('¿Está seguro de que desea eliminar este registro?') Then begin
            // El usuario ha hecho clic en "Sí"
            // Realiza la acción de eliminación aquí
            ReturnValue := true;
        end else begin
            // El usuario ha hecho clic en "No"
            // Cancela la acción de eliminación aquí
            ReturnValue := false;
        end;

    end;

```

<br><br>

## Función Error

Documentación oficial en el [Link](https://learn.microsoft.com/en-us/dynamics-nav/error-function--dialog-)

Cuando surgen errores en tu aplicación, es esencial informar al usuario de manera clara y concisa. Para esto, la función Error entra en juego. Esta función muestra un mensaje de error en un cuadro de diálogo que obliga al usuario a hacer clic en "Aceptar" para cerrarlo. Así, le informarás sobre cualquier problema que afecte la operación actual:

```javascript
    procedure EjecuteError()
    begin
        Error('No se pudo completar la operación debido a un error en el sistema. Por favor, inténtelo de nuevo más tarde.');
    end;
```

<br><br>

## Función Message

Documentación oficial en el [Link](https://learn.microsoft.com/en-us/dynamics-nav/message-function--dialog-)

Cuando deseas proporcionar información al usuario sobre acciones completadas con éxito, la función Message es tu mejor aliada. Al igual que la función Error, muestra un cuadro de diálogo, pero esta vez con un mensaje informativo. Por ejemplo, puedes usarla para indicar que un registro se ha guardado correctamente:

```javascript
    procedure EjecuteMessage()
    begin
        Message('El registro se ha guardado correctamente.');
    end;

```

<br><br>

## Función STRMENU

Documentación oficial en el [Link](https://learn.microsoft.com/en-us/dynamics-nav/strmenu-function--dialog-)

Ahora, si deseas ofrecer opciones al usuario en forma de un menú desplegable, la función STRMENU es la solución. Mostrarás un menú con varias opciones y el usuario podrá seleccionar una de ellas antes de cerrarlo al hacer clic en "Aceptar". Esta función resulta útil cuando quieres dar al usuario más control sobre ciertas acciones:

```javascript
    procedure EjecuteStrMenu() ReturnValue: Integer;
    begin
        ReturnValue := StrMenu('Opción 1,Opción 2,Opción 3', 0, 'Seleccione una opción:');
    end;
```

<br><br>

## Combinando las funciones

Ahora es cuando combinamos estas funciones para crear interacciones más sofisticadas. Por ejemplo, puedes utilizar la función Confirm para pedirle al usuario que confirme una acción y, según su respuesta, mostrar un mensaje de error o informativo. Veamos un ejemplo donde utilizamos todas las funciones para ofrecer diferentes opciones al usuario antes de llevar a cabo una acción:

```javascript
    procedure CombinacionDeFunciones()
    var
        SelOption: Integer;
    begin

        If Confirm('¿Está seguro de que desea realizar esta acción?') Then begin
            // El usuario ha hecho clic en "Sí"
            // Realiza la acción aquí
            Message('La acción se ha realizado correctamente.');
        end else begin
            // El usuario ha hecho clic en "No"
            // Muestra un mensaje de error
            Error('La acción ha sido cancelada.');
        end;

        SelOption := StrMenu('Eliminar registro,Editar registro,Cancelar', 0, 'Seleccione una opción:');

        case SelOption of
            1:
                begin
                    // El usuario ha seleccionado "Eliminar registro"
                    If Confirm('¿Está seguro de que desea eliminar este registro?') Then begin
                        // El usuario ha hecho clic en "Sí"
                        // Realiza la acción de eliminación aquí
                        Message('El registro ha sido eliminado correctamente.');
                    end else begin
                        // El usuario ha hecho clic en "No"
                        // Muestra un mensaje de error
                        Error('La eliminación del registro ha sido cancelada.');
                    end;
                end;
            2:
                begin
                    // El usuario ha seleccionado "Editar registro"
                    // Realiza la acción de edición aquí
                    Message('El registro ha sido editado correctamente.');
                end;
            3:
                begin
                    // El usuario ha seleccionado "Cancelar" o ha cerrado el menú desplegable
                    // No se realiza ninguna acción
                    Message('La acción ha sido cancelada.');
                end;
        end;
    end;

```

<br><br>

En resumen, estas funciones integradas en el lenguaje de programación AL son valiosas herramientas para mejorar la interacción con los usuarios y proporcionar información relevante en tus aplicaciones de Business Central. Al combinarlas de manera inteligente, lograrás una experiencia de usuario más agradable y efectiva.

<br><br>

GitHub del ejemplo en el [Link](https://github.com/Esanpons/ejemplos-blog/tree/main/AL/FuncionesDialogo)
