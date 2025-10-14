---
title: Aanklikbare afbeeldingscomponent maken
description: Aanklikbare afbeeldingscomponent maken in AEM Forms as a Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
exl-id: b635f171-775d-480e-bf7a-c92ab4af0aee
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Component maken

In dit artikel wordt ervan uitgegaan dat u enige ervaring hebt met het ontwikkelen voor AEM Forms CS. Er wordt ook aangenomen dat u een AEM Forms archetype-project hebt gemaakt.

Open uw AEM Forms-project in IntelliJ of een andere IDE van uw keuze. Een nieuw knooppunt maken met de naam svg under

```
apps\corecomponent\components\adaptiveForm
```

>[!NOTE]
>
> ``corecomponent`` is de appId die is opgegeven bij het maken van het Maven-project. Deze appID kan in uw omgeving anders zijn.


## .content.xml-bestand maken

Maak een bestand met de naam .content.xml onder het svg-knooppunt. Voeg de volgende inhoud toe aan het nieuwe bestand. U kunt de eigenschappen jcr:description,jcr:title en componentGroup naar wens wijzigen.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    jcr:description="USA MAP"
    jcr:primaryType="cq:Component"
    jcr:title="USA MAP"
    sling:resourceSuperType="wcm/foundation/components/responsivegrid"
    componentGroup="CustomCoreComponent - Adaptive Form"/>
```

## SVG.html maken

Maak het bestand svg.html. Dit dossier zal SVG van de V.S. map.Copy de inhoud van [&#x200B; svg.html &#x200B;](assets/svg.html) in het pas gecreëerde dossier teruggeven. Wat je hebt gekopieerd, is de kaart van SVG of USA. Sla het bestand op.

## Het project implementeren

Implementeer het project naar de lokale, voor de cloud geschikte instantie om de component te testen.

Om het project op te stellen, zult u aan de wortelomslag van uw project in het bevel snelle venster moeten navigeren en het volgende bevel uitvoeren.

```
mvn clean install -PautoInstallSinglePackage
```

Hiermee wordt het project geïmplementeerd op uw lokale AEM Forms-instantie en is de component beschikbaar voor opname in uw adaptieve formulier

![&#x200B; usa-kaart &#x200B;](./assets/usa-map.png)
