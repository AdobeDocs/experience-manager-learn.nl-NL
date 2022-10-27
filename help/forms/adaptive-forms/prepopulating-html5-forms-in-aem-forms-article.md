---
title: PrePopulate HTML5 Forms die gegevensattribuut gebruikt.
description: Vul HTML5-formulieren door gegevens op te halen van de achterste bron.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: ab0f5282-383b-4be6-9c57-cded6ab37528
last-substantial-update: 2020-01-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---

# HTML5 Forms met gegevenskenmerk prePopulate {#prepopulate-html-forms-using-data-attribute}


XDP-sjablonen die worden gerenderd in de HTML-indeling met AEM Forms, worden HTML5 of Mobile Forms genoemd. Deze formulieren worden vaak ingevuld wanneer ze worden gegenereerd.

Er zijn twee manieren om gegevens met het xdp malplaatje samen te voegen wanneer het als HTML wordt teruggegeven.

**dataRef**: U kunt de parameter dataRef in URL gebruiken. Deze parameter geeft het absolute pad aan van het gegevensbestand dat met de sjabloon wordt samengevoegd. Deze parameter kan een URL zijn naar een andere service die de gegevens retourneert in XML-indeling.

**data**: Deze parameter geeft de UTF-8-gecodeerde gegevensbytes aan die met de sjabloon worden samengevoegd. Als deze parameter wordt opgegeven, negeert de HTML5-vorm de parameter dataRef. Als beste praktijken, adviseren wij het gebruiken van de gegevensbenadering.

De aanbevolen methode is om het gegevenskenmerk in de aanvraag in te stellen met de gegevens die u vooraf in het formulier wilt invullen.

slingRequest.setAttribute(&quot;data&quot;, content);

In dit voorbeeld stellen we het gegevenskenmerk in met de inhoud. De inhoud vertegenwoordigt de gegevens waarmee u het formulier vooraf wilt invullen. Typisch zou u de &quot;inhoud&quot;door een vraag REST aan de interne dienst te maken halen halen.

Voor dit gebruiksgeval moet u een aangepast profiel maken. De details over het maken van een aangepast profiel worden duidelijk beschreven in [Hier AEM Forms-documentatie](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html).

Zodra u uw douaneprofiel creeert, zult u dan een JSP dossier creëren dat de gegevens door vraag aan uw achterste deelsysteem zal halen. Nadat de gegevens zijn opgehaald, gebruikt u de slingRequest.setAttribute(&quot;data&quot;, content); om het formulier vooraf in te vullen

Wanneer XDP wordt teruggegeven, kunt u in sommige parameters tot xdp ook overgaan en op de waarde van de parameter gebaseerd u de gegevens van het achterste deelsysteem kunt halen.

[Deze URL heeft bijvoorbeeld een naamparameter](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

JSP die u schrijft zal toegang tot de naamparameter door request.getParameter (&quot;naam&quot;) hebben. Vervolgens kunt u de waarde van deze parameter doorgeven aan uw back-endproces om de vereiste gegevens op te halen.
Volg de onderstaande stappen om deze functie aan uw systeem te laten werken:

* [Elementen downloaden en in AEM importeren met pakketbeheer](assets/prepopulatemobileform.zip)
Het pakket installeert het volgende

   * CustomProfile
   * Voorbeeld XDP
   * Het eindpunt van de POST van de steekproef dat gegevens zal terugkeren om uw vorm te bevolken

>[!NOTE]
>
>Als u uw vorm door werkbench proces wilt bevolken, kunt u callWorkbenchProcess.jsp in uw /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp in plaats van setdata.jsp willen omvatten

* [Uw favoriete browser naar deze URL verwijzen](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). Het formulier moet vooraf worden ingevuld met de waarde van de parameter name
