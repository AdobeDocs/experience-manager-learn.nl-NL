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
jira: KT-11664
thumbnail: kt-11664.jpeg
exl-id: 8b48c28f-ba7f-4255-be96-a7ce18ca208b
duration: 52
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# Responsieve onderbrekingspunten

Leer hoe u nieuwe responsieve onderbrekingspunten configureert voor AEM responsieve paginaeditor.

## CSS-onderbrekingspunten maken

Maak eerst mediafuncties in de CSS van het AEM responsieve raster waaraan de responsieve AEM site voldoet.

Maak in het bestand `/ui.apps/src/main/content/jcr_root/apps/[app name]/clientlibs/clientlib-grid/less/grid.less` de onderbrekingspunten die u samen met de mobiele emulator wilt gebruiken. Noteer `max-width` voor elk onderbrekingspunt, aangezien dit de CSS breekpunten aan de AEM responsieve onderbrekingspunten van de Redacteur van de Pagina in kaart brengt.

![ creeer nieuwe ontvankelijke breekpunten ](./assets/responsive-breakpoints/create-new-breakpoints.jpg)

## De onderbrekingspunten van de sjabloon aanpassen

Open het `ui.content/src/main/content/jcr_root/conf/<app name>/settings/wcm/templates/page-content/structure/.content.xml` -bestand en werk `cq:responsive/breakpoints` bij met de nieuwe definities voor onderbrekingspunten. Elk [ CSS breekpunt ](#create-new-css-breakpoints) zou een overeenkomstige knoop onder `breakpoints` met zijn `width` bezit moeten hebben dat aan CSS breekpunt `max-width` wordt geplaatst.

![ pas de responsieve breekpunten van het malplaatje aan ](./assets/responsive-breakpoints/customize-template-breakpoints.jpg)

## Emulatoren maken

Er moeten AEM emulators zijn gedefinieerd waarmee auteurs de responsieve weergave kunnen selecteren die u wilt bewerken in de Pagina-editor.

Emulatorknooppunten maken onder `/ui.apps/src/main/content/jcr_root/apps/<app name>/emulators`

Bijvoorbeeld `/ui.apps/src/main/content/jcr_root/apps/wknd-examples/emulators/phone-landscape` . Kopieer een referentie-emulatorknooppunt van `/libs/wcm/mobile/components/emulators` in CRXDE Lite naar en werk de kopie bij om de definitie van het knooppunt te versnellen.

![ creeer nieuwe mededingers ](./assets/responsive-breakpoints/create-new-emulators.jpg)

## Apparaatgroep maken

Groepeer de mededingers om [ hen beschikbaar te maken in AEM Redacteur van de Pagina ](#update-the-templates-device-group).

Maak een knooppuntstructuur van `/apps/settings/mobile/groups/<name of device group>` onder `/ui.apps/src/main/content/jcr_root` .

![ creeer nieuwe apparatengroep ](./assets/responsive-breakpoints/create-new-device-group.jpg)

Maak een `.content.xml` -bestand in `/apps/settings/mobile/groups/<device group name>` en definieer
de nieuwe emulators gebruiken code die vergelijkbaar is met de onderstaande code:

![ creeer nieuw apparaat ](./assets/responsive-breakpoints/create-new-device.jpg)

## De apparaatgroep van de sjabloon bijwerken

Ten slotte wijst u de apparaatgroep weer toe aan de paginasjabloon, zodat de emulators beschikbaar zijn in de Pagina-editor voor pagina&#39;s die met deze sjabloon zijn gemaakt.

Open het bestand `ui.content/src/main/content/jcr_root/conf/[app name]/settings/wcm/templates/page-content/structure/.content.xml` en werk de eigenschap `cq:deviceGroups` bij om naar de nieuwe mobiele groep te verwijzen (bijvoorbeeld `cq:deviceGroups="[mobile/groups/customdevices]"` )
