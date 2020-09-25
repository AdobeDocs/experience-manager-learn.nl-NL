---
title: 'Werken met InDesign-bestanden en Asset Templates in AEM Assets '
description: In deze videozelfstudie worden een InDesign-bestand en alle bijbehorende overwegingen gedefinieerd voor gebruik in de functie Asset Templates van AEM Assets.
feature: catalogs, asset-templates
topics: authoring, renditions, documents
audience: all
doc-type: tutorial
activity: understand
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 0%

---


# Werken met InDesign-bestanden en Asset Templates in AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

In deze videozelfstudie worden een InDesign-bestand en alle bijbehorende overwegingen gedefinieerd voor gebruik in de functie Asset Templates van AEM Assets.

## Het InDesign-sjabloonbestand samenstellen {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. De sjabloon voor het [**InDesign-bestand downloaden en openen**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Open het deelvenster Codes,** controleer de naamgevingsconventie voor tags en noteer dat de elementen die u voor de auteur kunt gebruiken in het InDesign-bestand al zijn gecodeerd. Vergeet niet dat alleen gelabelde elementen in AEM kunnen worden bewerkt.

   * **Venster > Hulpmiddelen > Labels**

3. Voeg op Pagina een nieuw tekstelement toe, geef de tekst &#39;Koptekst&#39; op en pas de alineastijl **Koptekst** toe.

   * **Venster > Stijlen > Alineastijlen**

   Maak vervolgens een nieuwe tag met de naam **Page2Heading en pas deze toe.**

4. Voeg de afbeelding FPO-logo ([opgegeven in het ZIP](assets/asset-templates-tutorial-video--supporting-files.zip)) toe aan het element Logo op de Master pagina.

   * **Klik met de rechtermuisknop** en selecteer **Aanpassen > Opties voor aanpassen aan kader... > Inhoud passend maken > Kader proportioneel vullen**
   [Meer informatie over de opties](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)voor aanpassen aan kader vindt u in de juiste richting.

5. Kopieer de koptekst (Logo en bedrijfsnaam) van de Master sjabloon naar Pagina en Pagina via Op plaats plakken.

   * Houd op pagina 1 Shift en Cmd ingedrukt en klik op MacOS of houd Shift en Alt ingedrukt en klik in Windows om de koptekst te selecteren die wordt weergegeven op de Master pagina en deze te verwijderen.
   * Kopieer de koptekst van de Master pagina naar pagina 1 via Op plaats plakken
   * De stappen voor pagina 2 herhalen

6. Open het deelvenster Structuur door op beide te dubbelklikken. Zorg ervoor dat alle structuurelementen overeenkomen met echte elementen in het InDesign-bestand. Verwijder niet-gebruikte of niet-benodigde elementen. Zorg ervoor dat alle tags semantisch zijn en dat de elementen correct zijn gecodeerd.

   >[!NOTE]
   >
   >Onthoud dat een slecht geconstrueerd InDesign-bestand de meest voorkomende oorzaak is voor problemen met AEM Asset Templates, zodat de codering en structuur schoon en correct zijn.

## Een assetsjabloon maken en ontwerpen in AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **Start InDesign Server** op poort 8080.
2. Zorg ervoor dat de instantie **AEM-auteur is geconfigureerd voor interactie met uw InDesign Server**(en omgekeerd).

   * [Configuratie van IDS Worker Cloud Service](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Configuratie van Cloud Service voor cloudproxy](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM configuratie ExternalExternalSGi](http://localhost:4502/system/console/configMgr)

3. **Het InDesign-bestand is geüpload naar AEM Assets** en AEM Workflow en InDesign Server staat toe de-bestanden volledig te verwerken.
4. **Maak een nieuwe sjabloon** onder **Middelen > Sjablonen** en selecteer het InDesign-bestand dat in stap 4 naar AEM is geüpload.
5. **Bewerk de Asset Template** die in Stap 5 is gemaakt en stel de bewerkbare velden samen.
6. Klik op **Gereed** om de uiteindelijke getrouwe uitvoeringen van de Asset Template te genereren.
7. Klik op de Asset Template-kaart om deze te openen en bekijk de uitvoeringen van Elementen om de hifi-uitvoeringen te downloaden.

## Aanvullende bronnen {#additional-resources}

InDesign-sjabloonbestand en ondersteunende afbeeldingen

Sjabloonbestand [InDesign downloaden en afbeeldingen ondersteunen](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [Proefversie van InDesign CC downloaden](https://creative.adobe.com/products/download/indesign)
* [Proefversie InDesign Server downloaden](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)
