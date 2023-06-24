---
title: "Fuentes barcodes SaaS"
summary: "En este ejemplo lo que mostramos es como se pueden leer los codigos de barras en SaaS."
layout: article
author: Esteve Sanpons
#cSpell:disable
category: [Business_Central,Reports]
custom_type: Boveda
permalink: /boveda/fuentes-barcodes-saas
#cSpell:enable
date: 2023-06-24 03:00:00 +0200
LinkedIn: false
---

# Fuentes barcodes SaaS


En este ejemplo lo que mostramos es como se pueden leer los codigos de barras en SaaS.



### Código Objeto BC

```javascript
report 50100 "ItemBarCodeMGS"
{
    UsageCategory = Administration;
    ApplicationArea = All;
    DefaultLayout = RDLC;
    Caption = 'Item Barcodes';
    RDLCLayout = 'ItemBarcodes.rdl';

    dataset
    {
        dataitem(Item; Item)
        {
            DataItemTableView = sorting("No.");
            RequestFilterFields = "No.";
            RequestFilterHeading = 'Items';
            column(No_; "No.")
            {
            }
            column(Description; Description)
            {
            }
            column(Codabar; EncodeTextCodabar)
            {
            }
            column(Code128; EncodeTextCode128)
            {
            }
            column(Code39; EncodeTextCode39)
            {
            }
            column(Code93; EncodeTextCode93)
            {
            }
            column(Interleaved2of5; EncodeTextInterleaved2of5)
            {
            }
            column(MSI; EncodeTextMSI)
            {
            }


            trigger OnAfterGetRecord()
            begin
                GenerateCodabar();
                GenerateCode128();
                GenerateCode39();
                GenerateCode93();
                GenerateInterleaved2of5();
                GenerateInterMSI();
            end;

        }
    }
    local procedure GenerateCodabar()
    var
        BarcodeSymbology: Enum "Barcode Symbology";
        BarcodeFontProvider: Interface "Barcode Font Provider";
        BarcodeString: Text;
    begin
        BarcodeFontProvider := Enum::"Barcode Font Provider"::IDAutomation1D;
        BarcodeSymbology := Enum::"Barcode Symbology"::Codabar;
        BarcodeString := Item."No.";
        BarcodeFontProvider.ValidateInput(BarcodeString, BarcodeSymbology);
        EncodeTextCodabar := BarcodeFontProvider.EncodeFont(BarcodeString, BarcodeSymbology);
    end;

    local procedure GenerateCode128()
    var
        BarcodeSymbology: Enum "Barcode Symbology";
        BarcodeFontProvider: Interface "Barcode Font Provider";
        BarcodeString: Text;
    begin
        BarcodeFontProvider := Enum::"Barcode Font Provider"::IDAutomation1D;
        BarcodeSymbology := Enum::"Barcode Symbology"::Code128;
        BarcodeString := Item."No.";
        BarcodeFontProvider.ValidateInput(BarcodeString, BarcodeSymbology);
        EncodeTextCode128 := BarcodeFontProvider.EncodeFont(BarcodeString, BarcodeSymbology);
    end;

    local procedure GenerateCode39()
    var
        BarcodeSymbology: Enum "Barcode Symbology";
        BarcodeFontProvider: Interface "Barcode Font Provider";
        BarcodeString: Text;
    begin
        BarcodeFontProvider := Enum::"Barcode Font Provider"::IDAutomation1D;
        BarcodeSymbology := Enum::"Barcode Symbology"::Code39;
        BarcodeString := Item."No.";
        BarcodeFontProvider.ValidateInput(BarcodeString, BarcodeSymbology);
        EncodeTextCode39 := BarcodeFontProvider.EncodeFont(BarcodeString, BarcodeSymbology);
    end;

    local procedure GenerateCode93()
    var
        BarcodeSymbology: Enum "Barcode Symbology";
        BarcodeFontProvider: Interface "Barcode Font Provider";
        BarcodeString: Text;
    begin
        BarcodeFontProvider := Enum::"Barcode Font Provider"::IDAutomation1D;
        BarcodeSymbology := Enum::"Barcode Symbology"::Code93;
        BarcodeString := Item."No.";
        BarcodeFontProvider.ValidateInput(BarcodeString, BarcodeSymbology);
        EncodeTextCode93 := BarcodeFontProvider.EncodeFont(BarcodeString, BarcodeSymbology);
    end;

    local procedure GenerateInterleaved2of5()
    var
        BarcodeSymbology: Enum "Barcode Symbology";
        BarcodeFontProvider: Interface "Barcode Font Provider";
        BarcodeString: Text;
    begin
        BarcodeFontProvider := Enum::"Barcode Font Provider"::IDAutomation1D;
        BarcodeSymbology := Enum::"Barcode Symbology"::Interleaved2of5;
        BarcodeString := Item."No.";
        BarcodeFontProvider.ValidateInput(BarcodeString, BarcodeSymbology);
        EncodeTextInterleaved2of5 := BarcodeFontProvider.EncodeFont(BarcodeString, BarcodeSymbology);
    end;

    local procedure GenerateInterMSI()
    var
        BarcodeSymbology: Enum "Barcode Symbology";
        BarcodeFontProvider: Interface "Barcode Font Provider";
        BarcodeString: Text;
    begin
        BarcodeFontProvider := Enum::"Barcode Font Provider"::IDAutomation1D;
        BarcodeSymbology := Enum::"Barcode Symbology"::MSI;
        BarcodeString := Item."No.";
        BarcodeFontProvider.ValidateInput(BarcodeString, BarcodeSymbology);
        EncodeTextMSI := BarcodeFontProvider.EncodeFont(BarcodeString, BarcodeSymbology);
    end;

    var
        EncodeTextCodabar: Text;
        EncodeTextCode128: Text;
        EncodeTextCode39: Text;
        EncodeTextCode93: Text;
        EncodeTextInterleaved2of5: Text;
        EncodeTextMSI: Text;
}
```

<br>
A continuación, creamos un diseño para el informe.
<br><br>

<input type="checkbox" id="image-checkbox-01" class="image-checkbox">
<label for="image-checkbox-01"  class="image-label">
    <img class="img-container" src="/assets/img/articles/fuentes-barcodes-saas/imagen001.png">
</label>
<br><br>


Dado que la fuente no está instalada localmente, debe configurar la fuente correcta para el campo de código de barras.
Haga clic con el botón derecho en el cuadro de texto y, a continuación, **seleccione Propiedades del cuadro de texto**.
<br><br>

<input type="checkbox" id="image-checkbox-02" class="image-checkbox">
<label for="image-checkbox-02"  class="image-label">
    <img class="img-container" src="/assets/img/articles/fuentes-barcodes-saas/imagen002.png">
</label>
<br><br>

Rellene la fuente utilizada por Barcode.
<br><br>

<input type="checkbox" id="image-checkbox-03" class="image-checkbox">
<label for="image-checkbox-03"  class="image-label">
    <img class="img-container" src="/assets/img/articles/fuentes-barcodes-saas/imagen003.png">
</label>
<br><br>

Puede encontrar estas fuentes en el sitio web de fuentes de código de barras [link](https://idautomation.com/)
<br><br>

<input type="checkbox" id="image-checkbox-04" class="image-checkbox">
<label for="image-checkbox-04"  class="image-label">
    <img class="img-container" src="/assets/img/articles/fuentes-barcodes-saas/imagen004.png">
</label>
<br><br>

Por ejemplo, Código 39 Nombres de fuentes y datos de aplicaciones
<br><br>

<input type="checkbox" id="image-checkbox-05" class="image-checkbox">
<label for="image-checkbox-05"  class="image-label">
    <img class="img-container" src="/assets/img/articles/fuentes-barcodes-saas/imagen005.png">
</label>
<br><br><br><br>


## Fuentes de prueba: solo para referencia
### Codabar : IDAutomationHCBL
<input type="checkbox" id="image-checkbox-06" class="image-checkbox">
<label for="image-checkbox-06"  class="image-label">
    <img class="img-container" src="/assets/img/articles/fuentes-barcodes-saas/imagen006.png">
</label>
<br><br>

### Código 128 : IDAutomationC128S
<input type="checkbox" id="image-checkbox-07" class="image-checkbox">
<label for="image-checkbox-07"  class="image-label">
    <img class="img-container" src="/assets/img/articles/fuentes-barcodes-saas/imagen007.png">
</label>
<br><br>

### Código 39 : IDAutomationHC39M
<input type="checkbox" id="image-checkbox-08" class="image-checkbox">
<label for="image-checkbox-08"  class="image-label">
    <img class="img-container" src="/assets/img/articles/fuentes-barcodes-saas/imagen008.png">
</label>
<br><br>

### Código 93 : IDAutomationC93M
<input type="checkbox" id="image-checkbox-09" class="image-checkbox">
<label for="image-checkbox-09"  class="image-label">
    <img class="img-container" src="/assets/img/articles/fuentes-barcodes-saas/imagen009.png">
</label>
<br><br>

### Intercalado 2 de 5 : IDAutomationHI25M
<input type="checkbox" id="image-checkbox-10" class="image-checkbox">
<label for="image-checkbox-10"  class="image-label">
    <img class="img-container" src="/assets/img/articles/fuentes-barcodes-saas/imagen010.png">
</label>
<br><br>

### MSI : IDAutomatizaciónMSIM
<input type="checkbox" id="image-checkbox-11" class="image-checkbox">
<label for="image-checkbox-11"  class="image-label">
    <img class="img-container" src="/assets/img/articles/fuentes-barcodes-saas/imagen011.png">
</label>
<br><br>



<br><br>
Extractos sacados de [link](https://yzhums.com/11763/)
<br><br>
Ejemplo oficial [link](https://docs.microsoft.com/es-es/learn/modules/understand-report-triggers-functions/4a-adding-barcodes)

