---
title: Dynamic Media 360-video's en aangepaste videominiatuur gebruiken voor AEM Assets
description: De verbeteringen van de Dynamic Media Viewer in AEM 6.5 omvatten de toevoeging van ondersteuning voor 360 videorendering, 360 mediasviewers (video360Social en video360VR) en de mogelijkheid om aangepaste videominiaturen te selecteren.
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 4ee0b68f-3897-4104-8615-9de8dbb8f327
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 0%

---

# Dynamic Media 360-video&#39;s en aangepaste videominiatuur gebruiken voor AEM Assets

De verbeteringen van de Dynamic Media Viewer in AEM 6.5 omvatten de toevoeging van ondersteuning voor 360 videorendering, 360 mediasviewers (video360Social en video360VR) en de mogelijkheid om aangepaste videominiaturen te selecteren.

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=12&learn=on)

>[!NOTE]
>
>De video veronderstelt dat uw AEM instantie op Dynamic Media S7 wijze loopt.  [Hier vindt u instructies voor het instellen van AEM met Dynamic Media](https://helpx.adobe.com/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). Wanneer u een video uploadt, verwerkt Dynamic Media het beeldmateriaal standaard als een 360-video, als de hoogte-breedteverhouding 2:1 is. De verhouding tussen breedte en hoogte is dus 2:1.

>[!NOTE]
>
>Dynamic Media 360 Media-componenten bieden alleen ondersteuning voor 360 video&#39;s.

## Dynamic Media 360-video&#39;s

Video&#39;s van 360 graden, ook wel sferische video&#39;s genoemd, zijn video-opnamen waarbij een weergave in elke richting tegelijkertijd wordt opgenomen en die met een omnidirectionele camera of verzameling camera&#39;s zijn opgenomen. Tijdens het afspelen op een plat beeldscherm heeft de gebruiker controle over de weergaverichting en het afspelen op mobiele apparaten maakt doorgaans gebruik van ingebouwde gyroscoop-besturing.  Het laat u zich buiten de grenzen van enige fotografie uitbreiden. Marketers kunnen gebruikers een aantrekkelijke ervaring bieden met behulp van 360 video&#39;s.  Laten we beginnen. Het criterium van de beeldverhouding panoramische kan in de configuratie van DMS7 van het bedrijf worden gewijzigd door het dubbele bezit s7PanoramicAR bij /conf/global/settings/cloudconfigs/dmscene7/jcr:content te specificeren.

## Dynamic Media 360-video&#39;s

Dynamic Media-video ondersteunt nu de mogelijkheid om een aangepaste miniatuur voor uw video te selecteren. Een gebruiker kan een bestaand element selecteren in AEM Assets of een videoframe selecteren als miniatuur.

## Dynamische 360 mediaverviewers

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Video360Social Viewer**</td>
      <td>**Video360VR Viewer**</td>
   </tr>
   <tr>
      <td>Dynamic Media Run-modus</td>
      <td>Alleen Dynamic Media Scene7-modus</td>
      <td>Alleen Dynamic Media Scene7-modus<br>
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

[Dynamic Media configureren in de Scene7-modus](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
