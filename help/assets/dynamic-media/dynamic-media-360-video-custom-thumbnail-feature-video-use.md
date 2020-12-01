---
title: Dynamische media 360-video's en aangepaste videominiatuur gebruiken met AEM Assets
seo-title: Dynamische media 360-video's en aangepaste videominiatuur gebruiken met AEM Assets
description: Dynamische media Viewer-verbeteringen in AEM 6.5 omvatten de toevoeging van ondersteuning voor 360 videorendering, 360 mediasviewers (video360Social en video360VR) en de mogelijkheid om aangepaste videominiaturen te selecteren.
seo-description: Dynamische media Viewer-verbeteringen in AEM 6.5 omvatten de toevoeging van ondersteuning voor 360 videorendering, 360 mediasviewers (video360Social en video360VR) en de mogelijkheid om aangepaste videominiaturen te selecteren.
uuid: 44b91c22-635c-48c2-af27-49bdbfb61639
discoiquuid: 67d5e0f2-3fde-4ea7-9e53-4fc0cf8b9f2a
sub-product: dynamic-media
feature: video-profiles, viewer-presets
topics: images, videos, renditions, authoring, integrations, publishing, metadata
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 2%

---


# Dynamische media 360-video&#39;s en aangepaste videominiatuur gebruiken met AEM Assets

Dynamische media Viewer-verbeteringen in AEM 6.5 omvatten de toevoeging van ondersteuning voor 360 videorendering, 360 mediasviewers (video360Social en video360VR) en de mogelijkheid om aangepaste videominiaturen te selecteren.

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=9&learn=on)

>[!NOTE]
>
>De video veronderstelt dat uw AEM instantie op Dynamische Media S7 wijze loopt.  [Hier](https://helpx.adobe.com/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html) vindt u instructies voor het instellen van AEM met Dynamic Media. Wanneer u een video uploadt, door gebrek, verwerkt Dynamische Media het lengte als 360 video, als het een aspectverhouding van 2:1 heeft. De verhouding tussen breedte en hoogte is dus 2:1.

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
      <td>**Video360VR-viewer**</td>
   </tr>
   <tr>
      <td>Dynamische modus voor mediatoets</td>
      <td>Alleen dynamische media Scene7-modus</td>
      <td>Alleen dynamische media Scene7-modus<br>
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
      <td>Navigatieweergave</td>
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
      <td>HTML5 Player</td>
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

[Dynamische media configureren in Scene7-modus](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
