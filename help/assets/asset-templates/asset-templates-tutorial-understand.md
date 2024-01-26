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
duration: 941
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 0%

---

# Werken met InDesign- en Asset Templates in AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

In deze videozelfstudie worden een InDesign-bestand en alle bijbehorende overwegingen gedefinieerd voor gebruik in de functie Asset Templates van AEM Assets.

## Het sjabloonbestand voor InDesigns samenstellen {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293?quality=12&learn=on)

1. Download en open de [**Sjabloon voor InDesign-bestand**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Open het deelvenster Labels,** Controleer de naamgevingsconventie voor tags en noteer dat de elementen die door de auteur kunnen worden gemaakt, al zijn gecodeerd. Vergeet niet dat alleen gelabelde elementen in AEM kunnen worden bewerkt.

   * **Venster > Hulpmiddelen > Labels**

3. Voeg op de pagina een nieuw tekstelement toe, geef de tekst &quot;Koptekst&quot; op en pas de tekst **Kop** Alineastijl.

   * **Venster > Stijlen > Alineastijlen**

   Maak vervolgens een nieuwe tag met de naam **Pagina2Kop.**

4. Voeg de afbeelding FPO-logo toe ([meegeleverd in het postvak](assets/asset-templates-tutorial-video--supporting-files.zip)) aan het element Logo op de stramienpagina.

   * **Klikken met rechtermuisknop** en selecteert u **Aanpassen > Opties voor aanpassen aan kader.. > Inhoud passend maken > Kader proportioneel vullen**

   [Meer informatie over opties voor aanpassen aan kader](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)en die het meest geschikt is voor uw gebruik.

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

1. **InDesign Server starten** op poort 8080.
2. Zorg ervoor dat **AEM instantie Auteur is geconfigureerd voor interactie met uw InDesign Server**(en omgekeerd).

   * [Configuratie van IDS Worker Cloud Service](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Configuratie van Cloud Service voor cloudproxy](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM configuratie ExternalExternalSGi](http://localhost:4502/system/console/configMgr)

3. **Het bestand InDesign is geüpload naar AEM Assets** en AEM workflow en InDesign Server in staat stellen de middelen volledig te verwerken.
4. **Een nieuwe sjabloon maken** krachtens **Middelen > Sjablonen** en selecteert u het InDesign dat naar AEM is geüpload in stap 4.
5. **De middelensjabloon bewerken** gemaakt in Stap 5 en ontwerpt de bewerkbare velden.
6. Klikken **Gereed** om de definitieve afrekeningen met hoge getrouwheid van de activasjabloon te genereren.
7. Klik op de Asset Template-kaart om deze te openen en bekijk de uitvoeringen van Elementen om de hifi-uitvoeringen te downloaden.

## Aanvullende bronnen {#additional-resources}

Sjabloonbestand voor InDesigns en ondersteunende afbeeldingen

Downloaden [Sjabloonbestand voor InDesigns en ondersteunende afbeeldingen](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [Proefversie van InDesign CC downloaden](https://creative.adobe.com/products/download/indesign)
* De proefversie van het InDesign Server kan worden gedownload van [Prerelease-site Adobe](https://www.adobeprerelease.com/) of [CC Enterprise-klanten kunnen contact opnemen met hun accountmanager om een proeflicentie voor een InDesign Server aan te vragen](https://www.adobe.com/products/indesignserver/faq.html)
