---
title: HTML5 Forms vooraf vullen met gegevenskenmerk.
seo-title: HTML5 Forms vooraf vullen met gegevenskenmerk.
description: HTML5-formulieren vullen door gegevens op te halen van de achterste bron.
seo-description: HTML5-formulieren vullen door gegevens op te halen van de achterste bron.
feature: Adaptieve Forms
topics: mobile-forms
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5.
uuid: 889d2cd5-fcf2-4854-928b-0c2c0db9dbc2
discoiquuid: 3aa645c9-941e-4b27-a538-cca13574b21c
topic: Ontwikkeling
role: Developer
level: Ervaren
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '515'
ht-degree: 0%

---


# HTML5 Forms vooraf vullen met gegevenskenmerk {#prepopulate-html-forms-using-data-attribute}

Ga naar de pagina [AEM Forms samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0) voor een koppeling naar een live demo van deze mogelijkheid.

XDP-sjablonen die worden gerenderd in HTML-indeling met AEM Forms, worden HTML5 of Mobile Forms genoemd. Deze formulieren worden vaak ingevuld wanneer ze worden gegenereerd.

Er zijn twee manieren om gegevens met de xdp-sjabloon samen te voegen wanneer deze als HTML worden gerenderd.

**dataRef**: U kunt de parameter dataRef in URL gebruiken. Deze parameter geeft het absolute pad aan van het gegevensbestand dat met de sjabloon wordt samengevoegd. Deze parameter kan een URL zijn naar een andere service die de gegevens retourneert in XML-indeling.

**gegevens**: Deze parameter geeft de UTF-8-gecodeerde gegevensbytes aan die met de sjabloon worden samengevoegd. Als deze parameter is opgegeven, negeert het HTML5-formulier de parameter dataRef. Als beste praktijken, adviseren wij het gebruiken van de gegevensbenadering.

De aanbevolen methode is om het gegevenskenmerk in de aanvraag in te stellen met de gegevens die u vooraf in het formulier wilt invullen.

slingRequest.setAttribute(&quot;data&quot;, content);

In dit voorbeeld stellen we het gegevenskenmerk in met de inhoud. De inhoud vertegenwoordigt de gegevens waarmee u het formulier vooraf wilt invullen. Typisch zou u de &quot;inhoud&quot;door een REST vraag aan de interne dienst halen.

Voor dit gebruiksgeval moet u een aangepast profiel maken. De details over het maken van een aangepast profiel worden duidelijk beschreven in [AEM Forms-documentatie hier](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html).

Zodra u uw douaneprofiel creeert, zult u dan een JSP dossier creëren dat de gegevens door vraag aan uw achterste deelsysteem zal halen. Nadat de gegevens zijn opgehaald, gebruikt u de slingRequest.setAttribute(&quot;data&quot;, content); om het formulier vooraf in te vullen

Wanneer XDP wordt teruggegeven, kunt u in sommige parameters tot xdp ook overgaan en op de waarde van de parameter gebaseerd u de gegevens van het achterste deelsysteem kunt halen.

[Deze URL heeft bijvoorbeeld een naamparameter](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

JSP die u schrijft zal toegang tot de naamparameter door request.getParameter (&quot;naam&quot;) hebben. Vervolgens kunt u de waarde van deze parameter doorgeven aan uw back-endproces om de vereiste gegevens op te halen.
Volg de onderstaande stappen om deze functie aan uw systeem te laten werken:

* [Download en importeer de middelen in AEM met ](assets/prepopulatemobileform.zip)
pakketbeheer. Het pakket installeert de volgende

   * CustomProfile
   * Voorbeeld XDP
   * Het eindpunt van de POST van de steekproef dat gegevens zal terugkeren om uw vorm te bevolken

>[!NOTE]
>
>Als u uw vorm door werkbench proces wilt bevolken, kunt u callWorkbenchProcess.jsp in uw /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp in plaats van setdata.jsp willen omvatten

* [Wijs uw favoriete browser naar deze URL](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). Het formulier moet vooraf worden ingevuld met de waarde van de parameter name
