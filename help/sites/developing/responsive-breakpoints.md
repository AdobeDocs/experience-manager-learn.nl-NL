---
title: Responsieve onderbrekingspunten
description: Leer hoe u nieuwe responsieve onderbrekingspunten configureert voor AEM responsieve paginaeditor.
version: Cloud Service
feature: Page Editor
topic: Mobile, Development
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-01-05T00:00:00Z
kt: 11664
thumbnail: kt-11664.jpeg
source-git-commit: c82965636ddeef7dc165e0bea079c99f1a16e0ca
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Responsieve onderbrekingspunten

Leer hoe u nieuwe responsieve onderbrekingspunten configureert voor AEM responsieve paginaeditor.

## CSS-onderbrekingspunten maken

Maak eerst mediafuncties in de CSS van het AEM responsieve raster waaraan de responsieve AEM site voldoet.

In `/ui.apps/src/main/content/jcr_root/apps/[app name]/clientlibs/clientlib-grid/less/grid.less` , maakt u de onderbrekingspunten die u samen met de mobiele emulator wilt gebruiken. Noteer de `max-width` voor elk onderbrekingspunt, aangezien dit de CSS breekpunten aan de AEM responsieve onderbrekingspunten van de Redacteur van de Pagina in kaart brengt.

![Nieuwe responsieve onderbrekingspunten maken](./assets/responsive-breakpoints/create-new-breakpoints.jpg)

## De onderbrekingspunten van de sjabloon aanpassen

Open de `ui.content/src/main/content/jcr_root/conf/<app name>/settings/wcm/templates/page-content/structure/.content.xml` bestand en update `cq:responsive/breakpoints` met uw nieuwe definities van breekpuntknooppunten. Elk [CSS-breekpunt](#create-new-css-breakpoints) moet een corresponderend knooppunt hebben onder `breakpoints` met `width` eigenschap ingesteld op het CSS-onderbrekingspunt `max-width`.

![De responsieve onderbrekingspunten van de sjabloon aanpassen](./assets/responsive-breakpoints/customize-template-breakpoints.jpg)

## Emulatoren maken

Er moeten AEM emulators zijn gedefinieerd waarmee auteurs de responsieve weergave kunnen selecteren die u wilt bewerken in de Pagina-editor.

Emulatorknooppunten maken onder `/ui.apps/src/main/content/jcr_root/apps/<app name>/emulators`

Bijvoorbeeld, `/ui.apps/src/main/content/jcr_root/apps/wknd-examples/emulators/phone-landscape`. Een referentie-emulatorknooppunt kopiëren uit `/libs/wcm/mobile/components/emulators` in CRXDE Lite aan en werk het exemplaar bij om knoopdefinitie te versnellen.

![Nieuwe emulators maken](./assets/responsive-breakpoints/create-new-emulators.jpg)

## Apparaatgroep maken

Emulatoren groeperen naar [ze beschikbaar stellen in AEM paginaeditor](#update-the-templates-device-group).

Maken `/apps/settings/mobile/groups/<name of device group>` knooppuntstructuur onder `/ui.apps/src/main/content/jcr_root`.

![Nieuwe apparaatgroep maken](./assets/responsive-breakpoints/create-new-device-group.jpg)

Een `.content.xml` bestand in `/apps/settings/mobile/groups/<device group name>` en definieer de nieuwe emulators met code die vergelijkbaar is met de onderstaande code:

![Nieuw apparaat maken](./assets/responsive-breakpoints/create-new-device.jpg)

## De apparaatgroep van de sjabloon bijwerken

Ten slotte wijst u de apparaatgroep weer toe aan de paginasjabloon, zodat de emulators beschikbaar zijn in de Pagina-editor voor pagina&#39;s die met deze sjabloon zijn gemaakt.

Open de `ui.content/src/main/content/jcr_root/conf/[app name]/settings/wcm/templates/page-content/structure/.content.xml` bestand en werk de `cq:deviceGroups` eigenschap om te verwijzen naar de nieuwe mobiele groep (bijvoorbeeld `cq:deviceGroups="[mobile/groups/customdevices]"`)
