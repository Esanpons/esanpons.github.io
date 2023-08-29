---
title: "Propiedad SingleInstance en Programación: Compartir Variables Mediante Instancia Única"
summary: "Microsoft, añade la propiedad de SingleInstance para que des de cualquier lugar se llame a la codeunit y se puedan visualizar los mismos datos que se añadieron"
layout: article
author: Esteve Sanpons
#cSpell:disable
category: [Informacion_Otros, "Business_Central"]
custom_type: Boveda
permalink: /boveda/SingleInstance
#cSpell:enable
date: 2023-08-29 03:00:00 +0200
---

## Microsoft, añade la propiedad de SingleInstance para que des de cualquier lugar se llame a la codeunit y se puedan visualizar los mismos datos que se añadieron

El "SingleInstance" es una propiedad en programación que controla si se crea una sola instancia de una unidad de código o varias.

-   **Función:** Define si una unidad de código tendrá una única copia compartida de sus variables para diferentes partes del código.
-   **Valor:**
    -   Si es `true`, se crea una sola instancia compartida.
    -   Si es `false` (por defecto), no se limita a una sola instancia.
-   **Efecto:** Si es `true`, todas las partes del código que usan esa unidad comparten las mismas variables internas. Permanece activa hasta que se cierre la empresa.

En resumen, "SingleInstance" facilita compartir variables entre diferentes partes del código mediante una única instancia de unidad de código.

<br>
<br>

<div align="center">
  <a href="https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/developer/properties/devenv-singleinstance-property">
    <img src="https://learn.microsoft.com/en-us/media/open-graph-image.png" alt="Texto alternativo" width="50%" height="50%">
  </a>
</div>

<br>

La web origen de Microsoft Learn es [Link](https://learn.microsoft.com/)
