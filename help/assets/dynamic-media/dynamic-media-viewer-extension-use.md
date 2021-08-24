---
title: Dynamic Media Viewers gebruiken met Adobe Analytics en Adobe Launch
description: Met de Dynamic Media Viewers-extensie voor het starten van Adobe en de release van Dynamic Media Viewers 5.13 kunnen klanten van Dynamic Media, Adobe Analytics en Adobe Launch gebeurtenissen en gegevens gebruiken die specifiek zijn voor de Dynamic Media Viewers in hun configuratie voor het starten van Adobe.
sub-product: ' Dynamic Media '
feature: 'Asset Insights '
version: 6.3, 6.4, 6.5
topic: Inhoudsbeheer
role: User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 4%

---


# Dynamic Media Viewers gebruiken met Adobe Analytics en Adobe Launch{#using-dynamic-media-viewers-adobe-analytics-launch}

Voor klanten met Dynamic Media en Adobe Analytics kunt u nu het gebruik van Dynamic Media Viewers op uw website bijhouden met de Dynamic Media Viewer Extension.

>[!VIDEO](https://video.tv.adobe.com/v/29308/?quality=12&learn=on)

>[!NOTE]
>
> Adobe Experience Manager uitvoeren in de Dynamic Media Scene7-modus voor deze functionaliteit. U moet ook [Adobe Experience Platform Launch integreren met uw AEM instantie](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html).

Met de introductie van de Dynamic Media Viewer-extensie biedt Adobe Experience Manager nu geavanceerde analyseondersteuning voor elementen die worden geleverd met Dynamic Media-viewers (5.13). Hierdoor hebt u meer gedetailleerde controle over het bijhouden van gebeurtenissen wanneer een Dynamic Media Viewer wordt gebruikt op een sitepagina.

Als u AEM Assets en Sites al hebt, kunt u uw eigenschap Launch integreren met de AEM auteur-instantie. Nadat de startintegratie aan uw website is gekoppeld, kunt u dynamische mediacomponenten aan uw pagina toevoegen met gebeurtenistracering voor viewers ingeschakeld.

Voor klanten met alleen AEM Assets of Dynamic Media Classic kunnen gebruikers de insluitcode voor een viewer ophalen en deze aan de pagina toevoegen. De bibliotheken van het Manuscript van de lancering kunnen dan manueel aan de pagina voor kijker gebeurtenis worden toegevoegd volgen.

De volgende tabel bevat een lijst met Dynamic Media Viewer-gebeurtenissen en de argumenten die hiervoor worden ondersteund:

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
         <td> PAUZEREN </td>
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
         <td> SWAP </td>
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

* [Adobe Experience Manager integreren met Adobe starten](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)
* [Adobe Experience Manager uitvoeren in Dynamic Media Scene7-modus](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
* [Dynamic Media-viewers integreren met Adobe Analytics en Adobe Launch](https://helpx.adobe.com/experience-manager/6-5/assets/using/launch.html)
