---
title: Gebruikersinterface van POST openen bij verzenden
seo-title: Gebruikersinterface van POST openen bij verzenden
description: Dit is onderdeel 11 van een zelfstudie met meerdere stappen voor het maken van uw eerste interactieve communicatiedocument voor het afdrukkanaal. In dit deel starten we de gebruikersinterface van de agent voor het maken van ad-hoccorrespondentie over het verzenden van formulieren.
seo-description: Dit is onderdeel 11 van een zelfstudie met meerdere stappen voor het maken van uw eerste interactieve communicatiedocument voor het afdrukkanaal. In dit deel starten we de gebruikersinterface van de agent voor het maken van ad-hoccorrespondentie over het verzenden van formulieren.
uuid: 96f34986-a5c3-400b-b51b-775da5d2cbd7
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6168
thumbnail: 40122.jpg
translation-type: tm+mt
source-git-commit: 824efde8d90dd77d41dce093998b4215db2532ae
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---


# Gebruikersinterface van POST openen bij verzenden

In dit deel starten we de gebruikersinterface van de agent voor het maken van ad-hoccorrespondentie over het verzenden van formulieren.

Dit artikel zal u door de stappen lopen betrokken bij het openen van agent ui interface bij het voorleggen van een vorm. Doorgaans wordt een formulier door een medewerker van de klantenservice ingevuld met enkele invoerparameters en op de verzendagent van het formulier wordt de gebruikersinterface geopend met gegevens die vooraf zijn ingevuld bij de Prefill-service van het formuliergegevensmodel.De invoerparameters voor de Prefill-service van het formuliergegevensmodel worden uit de formulierverzending geëxtraheerd.

In de volgende video ziet u hoe u hoofdletters/kleine letters gebruikt

>[!VIDEO](https://video.tv.adobe.com/v/40122/?quality=9&learn=on)

```java
String accountNumber = request.getParameter("accountnumber"))
ParameterMap parameterMap = new ParameterMap();
RequestParameter icLetterId[] = new RequestParameter[1];
icLetterId[0] = new FormFieldRequestParameter("/content/dam/formsanddocuments/retirementstatementprint");
parameterMap.put("documentId", icLetterId);
RequestParameter Random[] = new RequestParameter[1];
Random[0] = new FormFieldRequestParameter("209457");
parameterMap.put("Random", Random);
Map map = new HashMap();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,parameterMap,"GET");
wrapperRequest.getRequestDispatcher("/aem/forms/createcorrespondence.html").include(wrapperRequest, response);
```

Regel 1: Hiermee wordt het accountnummer opgehaald uit de parameter request

Lijn 2-8: Maak parametertoewijzing en stel de juiste sleutels en waarden in om de willekeurige document-id weer te geven.

Lijn 9-10: Maak een ander object Map voor de invoerparameter die in het formuliergegevensmodel is gedefinieerd.

Regel 11: Stel het kenmerk slingRequest &quot;paramMap&quot; in

Lijn 12-13: Het verzoek doorsturen naar de servlet

Deze mogelijkheid testen op uw server

* [Importeer en installeer de aan dit artikel gerelateerde elementen met gebruik van pakketbeheer.](assets/launch-agent-ui.zip)
* [Aanmelden bij configMgr](http://localhost:4502/system/console/configMgr)
* Zoeken naar _Adobe granite CSRF-filter_
* _/content/getprintchannel_ toevoegen in de uitgesloten paden
* Sla uw wijzigingen op.
* [Open POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp). Zorg ervoor dat de tekenreeks die aan FormFieldRequestParameter is doorgegeven, geldig documentId is.(regel 19).
* [Open de ](http://localhost:4502/content/OpenPrintChannel.html) webpagina, voer het accountnummer in en verzend het formulier.
* De interface van de agent UI zou met de gegevens moeten openen pre-bevolkt specifiek voor het rekeningsaantal ingegaan in de vorm.

>[!NOTE]
>
>Zorg ervoor dat de de inputparameter van de verrichting van de Gegevens van uw Model van de Vorm wordt gebonden aan het Attribuut van het Verzoek genoemd &quot;accountnumber&quot;voor dit om te werken. Als u de naam van de bindingwaarde in een andere naam verandert, zorg ervoor de verandering op lijn 25 van POST.jsp wordt weerspiegeld

