---
title: "Eventos Inicio BC"
summary: "Estos eventos son dados antes de que el area de trabajo la pueda visualizar el ususario"
layout: article
author: Esteve Sanpons
#cSpell:disable
category: [Business_Central]
custom_type: Boveda
permalink: /boveda/eventos-inicio-bc
#cSpell:enable
date: 2023-06-24 03:00:00 +0200
LinkedIn: false
---

# Eventos Inicio BC

Estos eventos son dados antes de que el area de trabajo la pueda visualizar el ususario

<br><br>

```javascript
    //Evento para despues de añadir el usuario y la contraseña
    [EventSubscriber(ObjectType::Codeunit, Codeunit::"System Initialization", 'OnAfterLogin', '', false, false)]
    local procedure MyProcedure()
    begin
        WorkDate(Today);
    end;



    //Evento para despues de abrirse la empresa
    [EventSubscriber(ObjectType::Codeunit, Codeunit::"Company Triggers", 'OnCompanyOpenCompleted', '', false, false)]
    local procedure OnCompanyOpenCompleted()
    begin
       WorkDate(Today);
    end;


```


