---
title: Werken met InDesign- en Asset Templates in AEM Assets
description: In deze videozelfstudie worden een InDesign-bestand en alle bijbehorende overwegingen gedefinieerd voor gebruik in de functie Asset Templates van AEM Assets.
version: 6.4, 6.5
topic: Content Management
feature: Templates
role: User
level: Intermediate
doc-type: Tutorial
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
duration: 909
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 0%

---

# Werken met InDesign- en Asset Templates in AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

In deze videozelfstudie worden een InDesign-bestand en alle bijbehorende overwegingen gedefinieerd voor gebruik in de functie Asset Templates van AEM Assets.

## Het sjabloonbestand voor InDesigns samenstellen {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293?quality=12&learn=on)

1. De download en opent het [**malplaatje van het het dossierdossier van het InDesign**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **open het paneel van Markeringen,** herzie de markering noemende overeenkomst, en merk op dat de auteur-able elementen in het dossier van het InDesign reeds geëtiketteerd zijn. Vergeet niet dat alleen gelabelde elementen in AEM kunnen worden bewerkt.

   * **Venster > Hulpmiddelen > Markeringen**

3. Voor Pagina, voeg een nieuw tekstelement toe, verstrek de tekst &quot;Kopbal&quot;en pas de **Kop** Stijl van de Paragraaf toe.

   * **Venster > Stijlen > de Stijlen van de Paragraaf**

   Dan, creeer en pas een nieuwe Markering toe genoemd **Page2Heading.**

4. Voeg het beeld van het Logo FPO ([ dat in ZIP ](assets/asset-templates-tutorial-video--supporting-files.zip) wordt verstrekt) aan het element van het Logo op de Hoofdpagina toe.

   * **klik met de rechtermuisknop** en selecteer **Aanpassen > Opties voor aanpassen aan kader.. > Inhoud aanpassen > Kader proportioneel vullen**

   [ Leer meer over Kader het Aanpassen opties ](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames), en die voor uw gebruiksgeval juist is.

5. Kopieer de koptekst (Logo en bedrijfsnaam) van de hoofdsjabloon naar Pagina en Pagina via Op plaats plakken.

   * Houd op pagina 1 Shift en Cmd ingedrukt en klik op macOS of houd Shift en Alt ingedrukt en klik op Windows om de koptekst te selecteren die wordt weergegeven op de stramienpagina en deze te verwijderen.
   * Kopieer de koptekst van de stramienpagina naar pagina 1 via Op plaats plakken
   * De stappen voor pagina 2 herhalen

6. Open het deelvenster Structuur door op elke gewenste structuur te dubbelklikken. Zorg ervoor dat alle structuurelementen overeenkomen met de echte elementen in het bestand InDesign. Verwijder niet-gebruikte of niet-benodigde elementen. Zorg ervoor dat alle tags semantisch zijn en dat de elementen correct zijn gecodeerd.

   >[!NOTE]
   >
   >Onthoud dat een slecht geconstrueerd InDesign de meest voorkomende oorzaak is voor problemen met AEM Asset Templates, zodat de codering en structuur schoon en correct zijn.

## Een assetsjabloon maken en ontwerpen in AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294?quality=12&learn=on)

1. **InDesign Server van het Begin** op haven 8080.
2. Verzeker de **AEM instantie van de Auteur wordt gevormd om met uw InDesign Server** (en vice versa) in wisselwerking te staan.

   * [ Configuratie van de Cloud Service van de Arbeider IDS ](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [ de configuratie van de Cloud Service van de Volmacht van de Wolk ](http://localhost:4502/etc/cloudservices/proxy.html)
   * [ AEM Configuratie ExternalExternalSGi ](http://localhost:4502/system/console/configMgr)

3. **uploadde het dossier van het InDesign aan AEM Assets** en staat AEM Werkschema en InDesign Server toe om de activa volledig te verwerken.
4. **creeer een nieuw Malplaatje** onder **Assets > Malplaatjes** en selecteer het dossier van het InDesign dat aan AEM in Stap #4 wordt geupload.
5. **geeft het Malplaatje van Activa** uit in Stap #5 wordt gecreeerd, en auteur de editable gebieden.
6. Klik **Gereed** om de definitieve high-fidelity vertoningen van het Malplaatje van Activa te produceren.
7. Klik op de Asset Template-kaart om deze te openen en bekijk de uitvoeringen van Elementen om de hifi-uitvoeringen te downloaden.

## Aanvullende bronnen {#additional-resources}

Sjabloonbestand voor InDesigns en ondersteunende afbeeldingen

Het dossier van het InDesign van de download [ en ondersteunende Beelden ](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [ InDesign CC proefdownload ](https://creative.adobe.com/products/download/indesign)
* De proefversie van het InDesign Server kan van [ de plaats van de Prerelease van de Adobe ](https://www.adobeprerelease.com/) worden gedownload of [ de klanten van de Onderneming van CC kunnen hun Uitvoerder van de Rekening contacteren om de vergunning van de InDesign Server te verzoeken ](https://www.adobe.com/products/indesignserver/faq.html)
