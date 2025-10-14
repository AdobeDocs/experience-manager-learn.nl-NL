---
title: Dynamische media 360-video's en aangepaste videominiatuur gebruiken met AEM Assets
description: Dynamische media Viewer-verbeteringen in AEM 6.5 omvatten de toevoeging van ondersteuning voor 360 videorendering, 360 mediasviewers (video360Social en video360VR) en de mogelijkheid om aangepaste videominiaturen te selecteren.
feature: Video Profiles
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 4ee0b68f-3897-4104-8615-9de8dbb8f327
duration: 656
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 0%

---

# Dynamische media 360-video&#39;s en aangepaste videominiatuur gebruiken met AEM Assets

Dynamische media Viewer-verbeteringen in AEM 6.5 omvatten de toevoeging van ondersteuning voor 360 videorendering, 360 mediasviewers (video360Social en video360VR) en de mogelijkheid om aangepaste videominiaturen te selecteren.

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=12&learn=on)

>[!NOTE]
>
>Video gaat ervan uit dat uw AEM-instantie wordt uitgevoerd in de Dynamic Media S7-modus.  [&#x200B; de Instructies bij vestiging AEM met Dynamische Media kunnen hier worden gevonden &#x200B;](https://helpx.adobe.com/nl/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). Wanneer u een video uploadt, door gebrek, verwerkt Dynamische Media het lengte als 360 video, als het een aspectverhouding van 2:1 heeft. De verhouding tussen breedte en hoogte is dus 2:1.

>[!NOTE]
>
>Dynamische media 360-mediacomponenten ondersteunen alleen 360 video&#39;s.

## Dynamische media 360-video&#39;s

Video&#39;s van 360 graden, ook wel sferische video&#39;s genoemd, zijn video-opnamen waarbij een weergave in elke richting tegelijkertijd wordt opgenomen en die met een omnidirectionele camera of verzameling camera&#39;s zijn opgenomen. Tijdens het afspelen op een plat beeldscherm heeft de gebruiker controle over de weergaverichting en het afspelen op mobiele apparaten maakt doorgaans gebruik van ingebouwde gyroscoop-besturing.  Het laat u zich buiten de grenzen van enige fotografie uitbreiden. Marketers kunnen gebruikers een aantrekkelijke ervaring bieden met behulp van 360 video&#39;s.  Laten we beginnen. Het criterium van de beeldverhouding panoramische kan in de configuratie van DMS7 van het bedrijf worden gewijzigd door het dubbele bezit s7PanoramicAR bij /conf/global/settings/cloudconfigs/dmscene7/jcr:content te specificeren.

## Dynamische media 360-video&#39;s

Dynamische mediavideo ondersteunt nu de mogelijkheid om een aangepaste miniatuur voor uw video te selecteren. Een gebruiker kan een bestaand element selecteren in AEM Assets of een videoframe selecteren als miniatuur.

## Dynamische 360 mediaverviewers

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Video360Social Viewer**</td>
      <td>**Video360VR Viewer**</td>
   </tr>
   <tr>
      <td>Dynamische modus voor mediatoets</td>
      <td>Alleen dynamische Media Scene7-modus</td>
      <td>De dynamische Wijze van Media Scene7 slechts <br>
         <br>
      </td>
   </tr>
   <tr>
      <td>Hoofdletters gebruiken</td>
      <td>
         <p>Voor websites en apparaten die geen gyroscoop ondersteunen</p>
         <p> </p>
      </td>
      <td>
         <p>Biedt een virtuele realistische ervaring voor een apparaat dat gyroscoop ondersteunt </p>
      </td>
   </tr>
   <tr>
      <td>Audio, stereomodus</td>
      <td>Nee</td>
      <td>Ja</td>
   </tr>
   <tr>
      <td>Video afspelen</td>
      <td>Ja</td>
      <td>Ja</td>
   </tr>
   <tr>
      <td>Navigatieaanzicht</td>
      <td>
         <ul>
            <li>Muissleep (op desktopsystemen)</li>
            <li>Veeggebaar (aanraakapparaten)</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>Muis- en sleepopties zijn uitgeschakeld</li>
            <li>Gebruikt ingebouwde gyroscoop</li>
         </ul>
      </td>
   </tr>
   <tr>
      <td>HTML5-speler</td>
      <td>Ja</td>
      <td>Ja</td>
   </tr>
   <tr>
      <td>Opties voor sociaal delen</td>
      <td>Ja</td>
      <td>Nee</td>
   </tr>
</tbody>
</table>

## Aanvullende bronnen{#additional-resources}

[&#x200B; Vormend Dynamische Media op wijze Scene7 &#x200B;](https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/config-dms7.html)
