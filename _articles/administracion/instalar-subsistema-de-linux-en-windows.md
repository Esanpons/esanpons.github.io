---
title: "Instalar Subsistema de Linux en Windows"
summary: "Proceso para la instalación del subsistema de Linux con una distribución de ubuntu, dentro del propio Windows"
layout: article
#cSpell:disable
category: ["Administracion"]
custom_type: Boveda
permalink: /boveda/instalar-subsistema-de-linux-en-windows
#cSpell:enable
author: Esteve Sanpons
date: 2023-06-24 08:00:00 +0200
LinkedIn: false
---



1.  Abrir powershell como administrador  y ingresar
```powershell
wsl --install
```

2. Reiniciar la maquina.

3. A veces puede darse el error de que falta actualización del kernel
<input type="checkbox" id="image-checkbox-01" class="image-checkbox">
<label for="image-checkbox-01"  class="image-label">
    <img class="img-container" src="/assets/img/articles/instalar-subsistema-de-linux-en-windows/imagen01.png">
</label>

4. Si nos aparece este mensaje es porque no tenemos la virtualization activada en la BIOS
<input type="checkbox" id="image-checkbox-02" class="image-checkbox">
<label for="image-checkbox-02"  class="image-label">
    <img class="img-container" src="/assets/img/articles/instalar-subsistema-de-linux-en-windows/imagen02.png">
</label>

5. Si no ha salido al reiniciar ejecutamos la terminal de ubuntu. 
<input type="checkbox" id="image-checkbox-03" class="image-checkbox">
<label for="image-checkbox-03"  class="image-label">
    <img class="img-container" src="/assets/img/articles/instalar-subsistema-de-linux-en-windows/imagen03.png">
</label>

6. Cuando ejecutamos el ubuntu por primera vez nos pedirá que añadamos un usuario y contraseña. 
Añadimos el usuario y la contraseña del root.
<input type="checkbox" id="image-checkbox-04" class="image-checkbox">
<label for="image-checkbox-04"  class="image-label">
    <img class="img-container" src="/assets/img/articles/instalar-subsistema-de-linux-en-windows/imagen04.png">
</label>

7. Listo ya lo tenemos instalado.

8. Para ir a la carpeta del subsistema podemos hacerlo con la siguiente ruta:
>  \\\wsl$\