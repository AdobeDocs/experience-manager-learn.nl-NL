---
title: HTML5 Forms vooraf vullen met gegevenskenmerk.
description: U kunt HTML5-formulieren vullen door gegevens op te halen van de eindbron.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: ab0f5282-383b-4be6-9c57-cded6ab37528
last-substantial-update: 2020-01-09T00:00:00Z
duration: 94
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---

# HTML5 Forms vooraf vullen met gegevenskenmerk {#prepopulate-html-forms-using-data-attribute}


XDP-sjablonen die worden gerenderd in de HTML-indeling, worden HTML5 of Mobile Forms genoemd. Deze formulieren worden vaak ingevuld wanneer ze worden gegenereerd.

Er zijn twee manieren om gegevens samen te voegen met de xdp-sjabloon wanneer deze worden gerenderd als HTML.

**dataRef**: U kunt de parameter dataRef in URL gebruiken. Deze parameter geeft het absolute pad aan van het gegevensbestand dat met de sjabloon wordt samengevoegd. Deze parameter kan een URL zijn naar een andere service die de gegevens retourneert in XML-indeling.

**gegevens**: Deze parameter specificeert UTF-8 gecodeerde gegevensbytes die met het malplaatje worden samengevoegd. Als deze parameter wordt opgegeven, negeert het HTML5-formulier de parameter dataRef. Als beste praktijken, adviseren wij het gebruiken van de gegevensbenadering.

De aanbevolen methode is om het gegevenskenmerk in de aanvraag in te stellen met de gegevens die u vooraf in het formulier wilt invullen.

slingRequest.setAttribute(&quot;data&quot;, content);

In dit voorbeeld stellen we het gegevenskenmerk in met de inhoud. De inhoud vertegenwoordigt de gegevens waarmee u het formulier vooraf wilt invullen. Typisch zou u de &quot;inhoud&quot;door een REST vraag aan de interne dienst halen.

Voor dit gebruiksgeval moet u een aangepast profiel maken. De details bij het creëren van douaneprofiel worden duidelijk gedocumenteerd in [&#x200B; documentatie van AEM Forms hier &#x200B;](https://helpx.adobe.com/nl/aem-forms/6/html5-forms/custom-profile.html).

Zodra u uw douaneprofiel creeert, zult u dan een JSP dossier creëren dat de gegevens door vraag aan uw achterste deelsysteem zal halen. Nadat de gegevens zijn opgehaald, gebruikt u de slingRequest.setAttribute(&quot;data&quot;, inhoud); om het formulier vooraf in te vullen

Wanneer XDP wordt teruggegeven, kunt u in sommige parameters tot xdp ook overgaan en op de waarde van de parameter gebaseerd u de gegevens van het achterste deelsysteem kunt halen.

[&#x200B; bijvoorbeeld heeft deze url naamparameter &#x200B;](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

JSP die u schrijft zal toegang tot de naamparameter door request.getParameter (&quot;naam&quot;) hebben. Vervolgens kunt u de waarde van deze parameter doorgeven aan uw back-endproces om de vereiste gegevens op te halen.
Volg de onderstaande stappen om deze functie aan uw systeem te laten werken:

* [&#x200B; Download en voer de activa in AEM in gebruikend pakketmanager &#x200B;](assets/prepopulatemobileform.zip)
Het pakket installeert het volgende

   * CustomProfile
   * Voorbeeld XDP
   * Voorbeeld van POST-eindpunt dat gegevens retourneert om het formulier te vullen

>[!NOTE]
>
>Als u uw vorm door werkbench proces wilt bevolken, kunt u callWorkbenchProcess.jsp in uw /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp in plaats van setdata.jsp willen omvatten

* [&#x200B; Punt uw favoriete browser aan dit url &#x200B;](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). Het formulier moet vooraf worden ingevuld met de waarde van de parameter name
