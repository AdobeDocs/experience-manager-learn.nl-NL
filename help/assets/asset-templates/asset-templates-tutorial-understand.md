---
title: Werken met InDesign-bestanden en Asset Templates in AEM Assets
description: In deze videozelfstudie worden een InDesign-bestand en alle bijbehorende overwegingen gedefinieerd voor gebruik in de functie Asset Templates van AEM Assets.
version: 6.4, 6.5
topic: Content Management
role: User
level: Intermediate
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 0%

---

# Werken met InDesign-bestanden en Asset Templates in AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

In deze videozelfstudie worden een InDesign-bestand en alle bijbehorende overwegingen gedefinieerd voor gebruik in de functie Asset Templates van AEM Assets.

## Het InDesign-sjabloonbestand samenstellen {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. Download en open de [**Sjabloon voor InDesign-bestand**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Open het deelvenster Labels,** Controleer de naamgevingsconventie voor tags en noteer dat de elementen die door de auteur kunnen worden gemaakt in het InDesign-bestand al zijn gecodeerd. Vergeet niet dat alleen gelabelde elementen in AEM kunnen worden bewerkt.

   * **Venster > Hulpmiddelen > Labels**

3. Voeg op de pagina een nieuw tekstelement toe, geef de tekst &quot;Koptekst&quot; op en pas de tekst **Kop** Alineastijl.

   * **Venster > Stijlen > Alineastijlen**

   Maak vervolgens een nieuwe tag met de naam **Pagina2Kop.**

4. Voeg de afbeelding voor het FPO-logo toe ([meegeleverd in het postvak](assets/asset-templates-tutorial-video--supporting-files.zip)) aan het element Logo op de Master pagina.

   * **Klikken met rechtermuisknop** en selecteert u **Aanpassen > Opties voor aanpassen aan kader... > Inhoud passend maken > Kader proportioneel vullen**
   [Meer informatie over opties voor aanpassen aan kader](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)en die het meest geschikt is voor uw gebruik.

5. Kopieer de koptekst (Logo en bedrijfsnaam) van de Master sjabloon naar Pagina en Pagina via Op plaats plakken.

   * Houd op pagina 1 Shift en Cmd ingedrukt en klik op macOS of houd Shift en Alt ingedrukt en klik op Windows om de koptekst te selecteren die wordt weergegeven op de Master pagina en deze te verwijderen.
   * Kopieer de koptekst van de Master pagina naar pagina 1 via Op plaats plakken
   * De stappen voor pagina 2 herhalen

6. Open het deelvenster Structuur door op beide te dubbelklikken. Zorg ervoor dat alle structuurelementen overeenkomen met echte elementen in het InDesign-bestand. Verwijder niet-gebruikte of niet-benodigde elementen. Zorg ervoor dat alle tags semantisch zijn en dat de elementen correct zijn gecodeerd.

   >[!NOTE]
   >
   >Onthoud dat een slecht geconstrueerd InDesign-bestand de meest voorkomende oorzaak is voor problemen met AEM Asset Templates, zodat de codering en structuur schoon en correct zijn.

## Een assetsjabloon maken en ontwerpen in AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **InDesign Server starten** op poort 8080.
2. Zorg ervoor dat de **AEM Author Instance wordt gevormd om met uw InDesign Server in wisselwerking te staan**(en omgekeerd).

   * [Configuratie van IDS Worker Cloud Service](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Configuratie van Cloud Service voor cloudproxy](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM configuratie ExternalExternalSGi](http://localhost:4502/system/console/configMgr)

3. **Het InDesign-bestand is geüpload naar AEM Assets** en staat AEM Workflow en InDesign Server toe om de middelen volledig te verwerken.
4. **Een nieuwe sjabloon maken** krachtens **Middelen > Sjablonen** en selecteer het InDesign-bestand dat in Stap 4 naar AEM is geüpload.
5. **De middelensjabloon bewerken** gemaakt in Stap 5 en ontwerpt de bewerkbare velden.
6. Klikken **Gereed** om de definitieve afrekeningen met hoge getrouwheid van de activasjabloon te genereren.
7. Klik op de Asset Template-kaart om deze te openen en bekijk de uitvoeringen van Elementen om de hifi-uitvoeringen te downloaden.

## Aanvullende bronnen {#additional-resources}

InDesign-sjabloonbestand en ondersteunende afbeeldingen

Downloaden [InDesign-sjabloonbestand en ondersteunende afbeeldingen](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [Proefversie van InDesign CC downloaden](https://creative.adobe.com/products/download/indesign)
* De proefversie van InDesign Server kan worden gedownload van [Adobe Prerelease-site](https://www.adobeprerelease.com/) of [CC Enterprise-klanten kunnen contact opnemen met hun accountmanager om een licentie voor een InDesign Server-proefversie aan te vragen](https://www.adobe.com/products/indesignserver/faq.html)
