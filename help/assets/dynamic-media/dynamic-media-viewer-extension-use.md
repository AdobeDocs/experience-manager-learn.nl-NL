---
title: Dynamic Media Viewers gebruiken met Adobe Analytics en tags
description: Met de extensie Dynamic Media Viewers voor tags, in combinatie met de release van Dynamic Media Viewers 5.13, kunnen klanten van Dynamic Media, Adobe Analytics en tags gebeurtenissen en gegevens gebruiken die specifiek zijn voor de Dynamic Media Viewers in hun tagconfiguratie.
sub-product: Dynamic Media
feature: Asset Insights
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 9d807f4c-999c-45e6-a9db-6c1776bddda1
duration: 576
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 0%

---

# Dynamic Media Viewers gebruiken met Adobe Analytics en tags{#using-dynamic-media-viewers-adobe-analytics-tags}

Voor klanten met Dynamic Media en Adobe Analytics kunt u nu het gebruik van Dynamic Media Viewers op uw website bijhouden met de Dynamic Media Viewer Extension.

>[!VIDEO](https://video.tv.adobe.com/v/29308?quality=12&learn=on)

>[!NOTE]
>
> Adobe Experience Manager uitvoeren in Dynamic Media Scene7-modus voor deze functionaliteit. U moet ook [ markeringen met uw instantie van AEM ](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html) integreren.

Met de introductie van de Dynamic Media Viewer-extensie biedt Adobe Experience Manager nu geavanceerde analyseondersteuning voor elementen die worden geleverd met Dynamic Media Viewer (5.13). Hierdoor hebt u meer gedetailleerde controle over het bijhouden van gebeurtenissen wanneer een Dynamic Media Viewer wordt gebruikt op een sitepagina.

Als u al AEM Assets en Sites hebt, kunt u uw eigenschap tags integreren met de AEM-auteur. Nadat de startintegratie aan uw website is gekoppeld, kunt u dynamische mediacomponenten aan uw pagina toevoegen met gebeurtenistracering voor viewers ingeschakeld.

Voor klanten met alleen AEM Assets of Dynamic Media Classic kan de gebruiker de insluitcode voor een viewer ophalen en toevoegen aan de pagina. De bibliotheken van het Manuscript van markeringen kunnen dan manueel aan de pagina voor kijker gebeurtenis het volgen worden toegevoegd.

In de volgende tabel worden de gebeurtenissen van de Dynamic Media Viewer en de ondersteunde argumenten weergegeven:

<table>
   <tbody>
      <tr>
         <td>Naam van viewergebeurtenis</td>
         <td>Argument reference</td>
      </tr>
      <tr>
         <td> VAAK </td>
         <td> %event.detail.dm.objID% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.compClass% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.instName% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.timeStamp% </td>
      </tr>
      <tr>
         <td> BANNER <br></td>
         <td> %event.detail.dm.BANNER.asset% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANNER.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANNER.label% </td>
      </tr>
      <tr>
         <td> HREF </td>
         <td> %event.detail.dm.HREF.rollover% </td>
      </tr>
      <tr>
         <td> ITEM </td>
         <td> %event.detail.dm.ITEM.rollover% </td>
      </tr>
      <tr>
         <td> LADEN </td>
         <td> %event.detail.dm.LOAD.applicationname% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.asset% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.company% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.sdkversion% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.viewertype% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.viewerversion% </td>
      </tr>
      <tr>
         <td> METAGEGEVENS </td>
         <td> %event.detail.dm.METADATA.length% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.METADATA.type% </td>
      </tr>
      <tr>
         <td> MILESTONE </td>
         <td> %event.detail.dm.MILESTONE.milestone% </td>
      </tr>
      <tr>
         <td> PAGINA </td>
         <td> %event.detail.dm.PAGE.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.PAGE.label% </td>
      </tr>
      <tr>
         <td> PAUZE </td>
         <td> %event.detail.dm.PAUSE.timestamp% </td>
      </tr>
      <tr>
         <td> AFSPELEN </td>
         <td> %event.detail.dm.PLAY.timestamp% </td>
      </tr>
      <tr>
         <td> SPIN </td>
         <td> %event.detail.dm.SPIN.framenumber% </td>
      </tr>
      <tr>
         <td> STOPPEN </td>
         <td> %event.detail.dm.STOP.timestamp% </td>
      </tr>
      <tr>
         <td> WISSELEN </td>
         <td> %event.detail.dm.SWAP.asset% </td>
      </tr>
      <tr>
         <td> STAAL </td>
         <td> %event.detail.dm.SWATCH.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.SWATCH.label% </td>
      </tr>
      <tr>
         <td> TARG </td>
         <td> %event.detail.dm.TARG.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.TARG.label% </td>
      </tr>
      <tr>
         <td> ZOOMEN </td>
         <td> %event.detail.dm.ZOOM.scale% </td>
      </tr>
   </tbody>
</table>

## Aanvullende bronnen{#additional-resources}

* [ Integrerend AEM met markeringen in Adobe Experience Platform ](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)
* [ lopende Adobe Experience Manager op Dynamische wijze Scene7 van Media ](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/config-dms7.html?lang=en)
* [ Integrating Dynamic Media Viewers met Adobe Analytics gebruikend markeringen ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/dynamic-media-viewer-extension-use.html)
