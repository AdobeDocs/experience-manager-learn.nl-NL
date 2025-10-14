---
title: Panorama en Verticale afbeeldingsviewer gebruiken met AEM Assets Dynamic Media
description: Dynamische media Viewer-verbeteringen in AEM 6.4 zijn onder andere de toevoeging van Panoramische Image Viewer, Panoramische Virtual Reality Image Viewer en Vertical Image Viewer. Panoramische Viewer biedt een eenvoudige manier om een boeiende, meeslepende ervaring met de ruimte, eigenschap, locatie of landschap te bieden, zonder dat er aangepaste ontwikkeling nodig is.
feature: Video Profiles, Video Profiles, 360 VR Video
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 6b2f7533-8ce0-4134-b1ae-b3c5d15a05e6
duration: 535
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 0%

---

# Panorama en Verticale afbeeldingsviewer gebruiken met AEM Assets Dynamic Media{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

Dynamische media Viewer-verbeteringen in AEM 6.4 zijn onder andere de toevoeging van Panoramische Image Viewer, Panoramische Virtual Reality Image Viewer en Vertical Image Viewer. Panoramische Viewer biedt een eenvoudige manier om een boeiende, meeslepende ervaring met de ruimte, eigenschap, locatie of landschap te bieden, zonder dat er aangepaste ontwikkeling nodig is.

>[!VIDEO](https://video.tv.adobe.com/v/24156?quality=12&learn=on)

>[!NOTE]
>
>Video gaat ervan uit dat uw AEM-instantie wordt uitgevoerd in de Dynamic Media S7-modus. [&#x200B; de Instructies bij vestiging AEM met Dynamische Media kunnen hier worden gevonden.](https://helpx.adobe.com/nl/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Panoramische en panoramische VR-viewer

Een afbeelding wordt op basis van de hoogte-breedteverhouding of trefwoorden als panoramisch beschouwd. Een afbeelding met de hoogte-breedteverhouding 2 wordt standaard beschouwd als een panoramische afbeelding. Voorinstellingen voor de Panoramische afbeeldingsviewer worden beschikbaar voor een voorvertoning als deze aan de bovenstaande criteria voldoen. Het criterium van de beeldverhouding panoramische kan in de configuratie van DMS7 van het bedrijf worden gewijzigd door het dubbele bezit s7PanoramicAR bij /conf/global/settings/cloudconfigs/dmscene7/jcr:content te specificeren. De trefwoorden worden opgeslagen in de eigenschap dc:keyword van het metagegevensknooppunt van het element. Als de trefwoorden een van de volgende combinaties bevatten:

* rechthoekig,
* bolvormig + panoramisch,
* bolvormig + panorama,

wordt beschouwd als een Panoramisch afbeeldingselement, ongeacht de hoogte-breedteverhouding ervan.

## Verticale afbeeldingsviewer

Bij horizontale stalen zijn de stalen soms pas zichtbaar wanneer de gebruiker de pagina omlaag schuift, afhankelijk van de grootte van het scherm op het bureaublad. Door gebruik te maken van een verticale viewer voor afbeeldingen en verticale stalen te plaatsen, zorgt u ervoor dat de stalen zichtbaar zijn, ongeacht de schermgrootte. De hoofdafbeelding wordt ook gemaximaliseerd. Bij horizontale stalen moest ruimte op de pagina worden gereserveerd, zodat deze zeer waarschijnlijk zichtbaar is en de hoofdafbeelding kleiner wordt. Bij een verticale lay-out hoeft u zich geen zorgen te maken over de toewijzing van deze ruimte en kunt u daarom de grootte van de hoofdafbeelding maximaliseren.

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>Panorama's en VR-viewer</td>
   <td>Verticale afbeeldingsviewer</td>
  </tr>
  <tr>
   <td>Dynamische modus voor mediatoets</td>
   <td>Alleen dynamische Media Scene7-modus</td>
   <td>DMS7 en dynamische media</td>
  </tr>
  <tr>
   <td>Hoofdletters gebruiken</td>
   <td><p>De Panoramische viewer en de Virtual Reality viewer bieden gebruikers een boeiendere ervaring. Een gebruiker kan een hotelkamer uitchecken zelfs voordat hij of zij een boeking maakt, of een huurbezit uitchecken zonder een afspraak te moeten plannen. Een gebruiker kan een locatie en nog veel meer mogelijkheden uitchecken. Het belangrijkste doel is om de consument een betere ervaring te bieden wanneer hij of zij uw website bezoekt en uiteindelijk de conversiesnelheid te verhogen.</p> <p> </p> </td> 
   <td><p>De verticale kijker van het Beeld helpt productbeelden het bekijken ervaring maximaliseren om consumenten de beste vertegenwoordiging van het product te geven, daardoor drijvende omzetting en minimaliserend terugkeer.</p> <p> </p> </td>
  </tr>
  <tr>
   <td>Beschikbaar </td>
   <td>OOTB</td>
   <td>OOTB</td>
  </tr>
 </tbody>
</table>

[&#x200B; Vormend Dynamische Media op wijze Scene7 &#x200B;](https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/config-dms7.html)

[&#x200B; Vormend Dynamische Media op Hybride wijze &#x200B;](https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>Panoramische viewers werken met panoramische afbeeldingen en zijn niet bedoeld voor gebruik met normale afbeeldingen.
