---
title: Ontwikkelen voor paginaverschil in AEM Sites
description: In deze video ziet u hoe u aangepaste stijlen voor de functionaliteit Paginaverschil van AEM-sites kunt gebruiken.
feature: Authoring
topics: development
audience: developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
topic: Ontwikkeling
role: Developer
level: Begin
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 1%

---


# Ontwikkelen voor paginaverschil {#developing-for-page-difference}

In deze video ziet u hoe u aangepaste stijlen voor de functionaliteit Paginaverschil van AEM-sites kunt gebruiken.

## Paginaverschilstijlen aanpassen {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871/?quality=9&learn=on)

>[!NOTE]
>
>Deze video voegt douane CSS aan de wij.Retail cliëntbibliotheek toe, waar aangezien deze veranderingen in het project van AEM Sites van de klant zouden moeten worden aangebracht; in de onderstaande voorbeeldcode: `my-project`.

AEM paginaverschil verkrijgt de OOTB CSS via een directe lading van `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`.

Wegens deze directe lading van CSS eerder dan het gebruiken van een categorie van de cliëntbibliotheek, moeten wij een ander injectiepunt voor de douanestijlen vinden, en dit douaneinjectiepunt is het auteursclientlib van het project.

Dit heeft het voordeel om deze de stijloverschrijvingen van douanestijlen toe te staan om huurspecifiek te zijn.

### Maak de ontwerpclientlib {#prepare-the-authoring-clientlib} klaar

Zorg voor het bestaan van een `authoring` client-lib voor uw project op `/apps/my-project/clientlib/authoring.`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### Aangepaste CSS opgeven {#provide-the-custom-css}

Voeg toe aan `authoring` clientlib a `css.txt` dat aan minder dossier richt dat de met voeten tredende stijlen zal verstrekken. [De ](https://lesscss.org/) voorkeur gaat uit naar Minder vanwege de vele handige functies, waaronder klasseomloop, die in dit voorbeeld wordt gebruikt.

```shell
base=./css

htmldiff.less
```

Maak het `less`-bestand dat de stijloverschrijvingen bij `/apps/my-project/clientlibs/authoring/css/htmldiff.less` bevat en geef desgewenst de overtrekstijlen op.

```css
/* Wrap with body to gives these rules more specificity than the OOTB */
body {

    /* .html-XXXX are the styles that wrap text that has been added or removed */

    .html-added {
        background-color: transparent;
     color: initial;
        text-decoration: none;
     border-bottom: solid 2px limegreen;
    }

    .html-removed {
        background-color: transparent;
     color: initial;
        text-decoration: none;
     border-bottom: solid 2px red;
    }

    /* .cq-component-XXXX require !important as the class these are overriding uses it. */

    .cq-component-changed {
        border: 2px dashed #B9DAFF !important;
        border-radius: 8px;
    }
    
    .cq-component-moved {
        border: 2px solid #B9DAFF !important;
        border-radius: 8px;
    }

    .cq-component-added {
        border: 2px solid #CCEBB8 !important;
        border-radius: 8px;
    }

    .cq-component-removed {
        border: 2px solid #FFCCCC !important;
        border-radius: 8px;
    }
}
```

### De CSS van de ontwerpclient opnemen via de paginacomponent {#include-the-authoring-clientlib-css-via-the-page-component}

Neem de categorie client-clients voor het schrijven op in de basispagina van het project `/apps/my-project/components/structure/page/customheaderlibs.html` direct vóór de tag `</head>` om ervoor te zorgen dat de stijlen worden geladen.

Deze stijlen moeten worden beperkt tot de WCM-modi [!UICONTROL Edit] en [!UICONTROL preview].

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

Het eindresultaat van een diff&#39;d-pagina met de bovenstaande stijlen zou er als volgt uitzien (HTML toegevoegd en Component gewijzigd).

![Paginaverschil](assets/page-diff.png)

## Aanvullende bronnen {#additional-resources}

* [Download de voorbeeldsite Web.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Werken met AEM clientbibliotheken](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Minder CSS-documentatie](https://lesscss.org/)
