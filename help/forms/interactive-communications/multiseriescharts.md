---
title: Diagrammen uit meerdere reeksen in AEM Forms
seo-title: Diagrammen uit meerdere reeksen in AEM Forms
description: Maak het juiste formuliergegevensmodel om grafieken met meerdere reeksen te maken in afdruk- en webkanaaldocumenten.
seo-description: Maak het juiste formuliergegevensmodel om grafieken met meerdere reeksen te maken in afdruk- en webkanaaldocumenten.
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 0%

---


# Diagrammen uit meerdere reeksen

AEM Forms 6.5 introduceerde de capaciteit om veelvoudige reekskaarten tot stand te brengen en te vormen. De veelvoudige reekskaarten worden typisch gebruikt in verband met Lijn, Bar, het grafiektype van de Kolom. De volgende grafiek is een goed voorbeeld van multi reeksengrafiek. De grafiek toont de groei van USD 10.000 in 3 verschillende onderlinge fondsen over een bepaalde periode. Als u dergelijke grafieken in AEM Forms wilt maken en gebruiken, moet u het juiste formuliergegevensmodel maken.

![multiseries](assets/seriescharts.jfif)

Als u grafieken met meerdere reeksen wilt maken in AEM Forms, moet u een geschikt formuliergegevensmodel maken met de benodigde entiteiten en koppelingen tussen de entiteiten. In de volgende schermafbeelding worden de entiteiten en de koppelingen tussen de drie entiteiten gemarkeerd. Op het hoogste niveau hebben we een entiteit genaamd &quot;Organisatie&quot;, die een één-op-veel associatie heeft met een fondsentiteit. De fondsentiteit heeft op haar beurt een een-op-een-relatie met de prestatieentiteit.

![formdatamodel](assets/formdatamodel.jfif)


## Formuliergegevensmodel maken voor grafieken uit meerdere reeksen

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)


### Lijstreekgrafieken configureren

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)


Voer de volgende stappen uit om dit op uw systeem te testen

* [Download en importeer het bestand MutualFundFactSheet.zip met AEM Package Manager.](assets/mutualfundfactsheet.zip)
* [Download SeriesChartSampleData.json op uw harde aandrijving.](assets/serieschartsampledata.json) Dit zijn de steekproefgegevens die zullen worden gebruikt om de grafiek te bevolken.
* [Navigeer naar Forms en Documenten.](https://helpx.adobe.com/aem/forms.html/content/dam/formsanddocuments.html)
* Selecteer voorzichtig de interactieve communicatiesjabloon &quot;MutualFundGrowthFactSheet&quot;.
* Klik op Voorvertoning | Voorbeeldgegevens uploaden.
* Blader naar het bestand met voorbeeldgegevens dat u als onderdeel van dit artikel hebt opgegeven.
* Geef een voorvertoning weer van het afdrukkanaal van de interactieve communicatie tussen &quot;MutualFundGrowthFactSheet&quot; en de voorbeeldgegevens die u in de vorige stap hebt gedownload.
