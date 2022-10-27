---
title: Twee kolomlay-outs maken voor documenten met afdrukkanalen
seo-title: Creating two column layouts for print channel documents
description: Twee kolomlay-outs maken voor een document met een afdrukkanaal
seo-description: Create 2 column layouts for print channel document
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 416e3401-ba9f-4da3-8b07-2d39f9128571
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Twee kolomindelingen in document met afdrukkanaal

In dit korte artikel worden de stappen gemarkeerd die nodig zijn om twee kolommen in een afdrukkanaal te maken. Het gebruiksgeval is het genereren van documenten van twee pagina&#39;s met pagina 1 met een 2-kolomlay-out en pagina 2 met de standaard 1-kolomlay-out.

Hieronder vindt u een overzicht van de stappen op hoog niveau voor het maken van twee kolomlay-outs met AEM Forms Designer.

* 2 inhoudsgebieden maken op pagina 1 master pagina
* Geef de twee inhoudsgebieden de naam &quot;leftColumn&quot; en &quot;rightColumn&quot;
* Tweede master pagina maken met één inhoudsgebied (dit is de standaardinstelling)
* Selecteer het tabblad Paginering (naamloos Subformulier) (pagina 1) en (naamloos Subformulier) (pagina2) en stel de eigenschappen in zoals weergegeven in de onderstaande schermafbeeldingen.

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

Nadat de eigenschappen voor paginering zijn ingesteld, kunnen we subformulieren of doelgebieden toevoegen onder (naamloos subformulier) (Pagina 1).

Vervolgens kunnen documentfragmenten aan deze subformulieren of doelgebieden worden toegevoegd. Wanneer de linkerkolom vol is, loopt de inhoud naar de rechterkolom.

Download de aan dit artikel verwante elementen om dit op uw lokale server te testen. Omlaag naar de onderkant van deze pagina schuiven

* [Voorbeeldafdrukkanaaldocument downloaden en installeren met pakketbeheer](assets/print-channel-with-two-column-layout.zip)
* [Voorbeeld van afdrukkanaaldocument bekijken](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
