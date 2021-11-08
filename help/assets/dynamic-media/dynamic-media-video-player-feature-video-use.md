---
title: De videospeler in AEM Dynamic Media gebruiken
description: AEM Dynamic Media-videospeler die gebruikmaakte van Flash-runtime voor ondersteuning van adaptieve videostreaming op desktopclients en browsers, werd agressiever bij het streamen van inhoud op basis van flash. Met de introductie van HLS (Apple Live HTTP Streaming video Delivery Protocol) kan inhoud nu worden gestreamd zonder dat Flash nodig is.
sub-product: dynamic-media
feature: Video Profiles
version: 6.3, 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 7e4cb782-836d-4ec0-97d0-645b91ea43e0
source-git-commit: c921594d5c352f98e0d830d7a85e026844fd5da6
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 0%

---


# De videospeler in AEM Dynamic Media gebruiken{#using-the-video-player-in-aem-dynamic-media}

AEM Dynamic Media-videospeler die gebruikmaakte van Flash-runtime voor ondersteuning van adaptieve videostreaming op desktopclients en browsers, werd agressiever bij het streamen van inhoud op basis van flash. Met de introductie van HLS (Apple Live HTTP Streaming video Delivery Protocol) kan inhoud nu worden gestreamd zonder dat Flash nodig is.

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## Snel zoeken naar niet-Flash videospeler {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

De browserondersteuning van HLS is als volgt: voor niet-ondersteunde browsers kunnen we terugvallen op progressieve videoverstrekking

>[!NOTE]
>
> Dynamic Media Hybrid biedt na mei 2022 GEEN ondersteuning voor videostreaming in Internet Explorer 11.

<table> 
 <thead> 
  <tr> 
   <th> <p>Apparaat</p> </th>
   <th> <p>Browser</p> </th>
   <th > <p>Video-afspeelmodus</p> </th>
  </tr>
 </thead>
 <tbody>
  <tr> 
   <td> <p>Desktop</p> </td>
   <td> <p>Internet Explorer 9 en 10</p> </td>
   <td> <p>Progressieve download</p> </td>
  </tr>
  <tr>
   <td> <p>Desktop</p> </td>
   <td> <p>Internet Explorer 11+</p> </td>
   <td> <p>HLS-videostreaming</p> </td>
  </tr>
  <tr>
   <td> <p>Desktop</p> </td>
   <td> <p>Firefox 23-44</p> </td>
   <td> <p>Progressieve download</p> </td>
  </tr>
  <tr> 
   <td> <p>Desktop</p> </td>
   <td> <p>Firefox 45 of hoger</p> </td>
   <td> <p>HLS-videostreaming</p> </td>
  </tr>
  <tr> 
   <td> <p>Desktop</p> </td>
   <td> <p>Chroom</p> </td>
   <td> <p>HLS-videostreaming</p> </td>
  </tr>
  <tr> 
   <td> <p>Desktop</p> </td>
   <td> <p>Safari (Mac)</p> </td>
   <td> <p>HLS-videostreaming</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobiel</p> </td>
   <td> <p>Chrome (Android 6 of eerder)</p> </td>
   <td> <p>Progressieve download</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobiel</p> </td>
   <td> <p>Chrome (Android 7 of hoger)</p> </td>
   <td> <p>HLS-videostreaming</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobiel</p> </td>
   <td> <p>Android (standaardbrowser)</p> </td>
   <td> <p>Progressieve download</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobiel</p> </td>
   <td> <p>Safari (iOS)</p> </td>
   <td> <p>HLS-videostreaming</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobiel</p> </td>
   <td> <p>Chrome (iOS)</p> </td>
   <td> <p>HLS-videostreaming</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobiel</p> </td>
   <td> <p>Blackberry</p> </td>
   <td> <p>HLS-videostreaming</p> </td>
  </tr>
 </tbody>
</table>
