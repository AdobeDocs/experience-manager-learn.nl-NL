---
title: Het vormen het Comité van de Vooruitzichten van de Pensionering
seo-title: Configuring Retirement Outlook Panel
description: Dit is onderdeel 10 van een zelfstudie met meerdere stappen voor het maken van uw eerste interactieve communicatiedocument. In dit deel, zullen wij het Comité van de Vooruitzichten van de Ouder vormen door tekst en grafiekcomponenten toe te voegen.
seo-description: This is part 10 of a multi-step tutorial for creating your first interactive communications document. In this part, we will configure Retirement Outlook Panel by adding text and chart components.
uuid: 1d5119b5-e797-4bf0-9b10-995b3f051f92
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
topic: Development
role: Developer
level: Beginner
exl-id: 0dd8a430-9a4e-4dc7-ad75-6ad2490430f2
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 0%

---

# Het vormen het Comité van de Vooruitzichten van de Pensionering{#configuring-retirement-outlook-panel}

* Dit is onderdeel 10 van een zelfstudie met meerdere stappen voor het maken van uw eerste interactieve communicatiedocument. In dit deel, zullen wij het Comité van de Vooruitzichten van de Ouder vormen door tekst en grafiekcomponenten toe te voegen.

* Meld u aan bij AEM Forms en navigeer naar Adobe Experience Manager > Forms > Forms &amp; Documents.

* Open de map 401KStatement.

* Open het 401KStatement-document in de bewerkingsmodus.

**Doelgebied van LeftPanel configureren**

* Tik op het doelgebied LeftPanel aan de rechterkant en klik op het pictogram &quot;+&quot; om het dialoogvenster Component invoegen weer te geven.

* Tekstcomponent invoegen.

* Tik zachtjes op de nieuwe tekstcomponent om de componentwerkbalk weer te geven

* Selecteer het pictogram &#39;Potlood&#39; om de standaardtekst te bewerken.

* De standaardtekst vervangen door &quot;**Uw Outlook Inkomensresultaten voor pensioenen&quot;**

**Doelgebied van RightPanel configureren**

* Tik op het doelgebied van RightPanel aan de rechterkant en klik op het pictogram &quot;+&quot; om het dialoogvenster Component invoegen te openen.

* Tekstcomponent invoegen.

* Tik zachtjes op de nieuwe tekstcomponent om de componentwerkbalk weer te geven.

* Selecteer het pictogram &#39;Potlood&#39; om de standaardtekst te bewerken.

* De standaardtekst vervangen door &quot;**Geschat maandelijks pensioeninkomen&quot;**

## Outlook-documentfragment voor inkomens verhogen {#add-retirement-income-outlook-document-fragment}

* Klik op het middelenpictogram en pas het filter toe om elementen van het type &quot;Documentfragmenten&quot; weer te geven. Sleep RetirementIncomeOutlook-documentfragment naar het doelgebied van het linkerdeelvenster.

* U kunt [op deze pagina](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/partseven.html) bij het toevoegen van documentfragment aan inhoudsgebieden.

## Geschatte maandelijkse inkomensgrafiek toevoegen {#adding-estimated-monthly-income-chart}

* Klik op het doelgebied van RightPanel aan de rechterkant. Klik op het pictogram &quot;+&quot; om de diagramcomponent in te voegen. We gebruiken een kolomdiagram om het geschatte maandinkomen weer te geven. Tik zachtjes op de nieuw ingevoegde diagramcomponent. Selecteer het pictogram &quot;Sleutel&quot;om de configuratie eigenschappen sheet te openen.Vorm de grafiek met de volgende eigenschappen zoals aangetoond in het hieronder screenshot.

**AEM Forms 6.4 - Vormende Geschatte maandelijkse grafiek van de Inkomstenkolom**

![form64](assets/estimatedmonthlyincomechart.png)

**AEM Forms 6.5 - Vormende Geschatte maandelijkse grafiek van de Inkomstenkolom**

![forms65](assets/estimatedmonthlyincomechart65.PNG)

## Volgende stappen

[Cirkeldiagram configureren](./parteleven.md)
