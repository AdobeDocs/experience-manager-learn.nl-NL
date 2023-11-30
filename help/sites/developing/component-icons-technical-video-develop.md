---
title: Componentpictogrammen aanpassen in Adobe Experience Manager Sites
description: Met componentpictogrammen kunnen auteurs snel een component met pictogrammen of betekenisvolle afkortingen identificeren. Auteurs kunnen nu de Componenten vinden die nodig zijn om hun webervaringen sneller dan ooit uit te bouwen.
version: 6.4, 6.5
feature: Core Components
topic: Development
role: User
level: Intermediate
doc-type: Technical Video
exl-id: 37dc26aa-0773-4749-8c8b-4544bd4d5e5f
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 0%

---

# Componentpictogrammen aanpassen {#developing-component-icons-in-aem-sites}

Met componentpictogrammen kunnen auteurs snel een component met pictogrammen of betekenisvolle afkortingen identificeren. Auteurs kunnen nu de Componenten vinden die nodig zijn om hun webervaringen sneller dan ooit uit te bouwen.

>[!VIDEO](https://video.tv.adobe.com/v/16778?quality=12&learn=on)

De Componentbrowser wordt nu in een consistent grijs thema weergegeven, met daarin het volgende:

* **[!UICONTROL Component Group]**
* **[!UICONTROL Component Title]**
* **[!UICONTROL Component Description]**
* **[!UICONTROL Component Icon]**
   * De eerste twee letters van de componenttitel *(standaard)*
   * Aangepaste PNG-afbeelding *(geconfigureerd door een ontwikkelaar)*
   * Aangepaste SVG-afbeelding *(geconfigureerd door een ontwikkelaar)*
   * CoralUI-pictogram *(geconfigureerd door een ontwikkelaar)*

## Opties voor configuratie van componentpictogrammen {#component-icon-configuration-options}

### Afkortingen {#abbreviations}

Standaard worden de eerste 2 tekens van de componenttitel (**[cq:Component]@jcr:titel**) worden gebruikt als de afkorting. Als **[cq:Component]@jcr:title=Article-lijst** de afkorting wordt weergegeven als &quot;**Ar**&quot;.

De afkorting kan worden aangepast via de **[cq:Component]@abbreletters** eigenschap. Hoewel deze waarde meer dan 2 tekens kan bevatten, wordt aangeraden de afkorting te beperken tot 2 tekens om visuele stoornissen te voorkomen.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUI-pictogrammen {#coralui-icons}

Pictogrammen van CoralUI, opgegeven door AEM, kunnen worden gebruikt voor componentpictogrammen. Als u een CoralUI-pictogram wilt configureren, stelt u een **[cq:Component]@cq:pictogram** eigenschap aan de gewenste waarde van het HTML-pictogramkenmerk van het pictogram CoralUI (opgesomd in de [CoralUI-documentatie](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG-afbeeldingen {#png-images}

PNG-afbeeldingen kunnen worden gebruikt voor componentpictogrammen. Als u een PNG-afbeelding wilt configureren als componentpictogram, voegt u de gewenste afbeelding toe als een **nt:bestand** benoemd **cq:icon.png** onder de **[cq:Component]**.

De PNG moet een transparante achtergrond hebben of een achtergrondkleur die is ingesteld op **#707070**.

De PNG-afbeeldingen worden geschaald naar **20 px bij 20 px**. Maar voor netvliesweergave **40 px** door **40 px** kan de voorkeur verdienen.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG-afbeeldingen {#svg-images}

SVG-afbeeldingen (op basis van vectoren) kunnen worden gebruikt voor componentpictogrammen. Als u een SVG-afbeelding wilt configureren als componentpictogram, voegt u de gewenste SVG toe als een **nt:bestand** benoemd **cq:icon.svg** onder de **[cq:Component]**.

Voor SVG-afbeeldingen moet een achtergrondkleur zijn ingesteld op **#707070** en een grootte van **20 px bij 20 px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Aanvullende bronnen {#additional-resources}

* [Beschikbare CoralUI-pictogrammen](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
