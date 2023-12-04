---
title: SetValue gebruiken in AEM Forms-workflow
description: Waarde van element instellen in door Adaptive Forms ingediende gegevens in AEM Forms OSGI
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 3919efee-6998-48e8-85d7-91b6943d23f9
last-substantial-update: 2020-01-09T00:00:00Z
duration: 140
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '503'
ht-degree: 0%

---

# SetValue gebruiken in AEM Forms-workflow

Stel de waarde van een XML-element in Adaptief Forms verzonden gegevens in de AEM Forms OSGI-workflow in.

![SetValue](assets/setvalue.png)

LiveCycle dat wordt gebruikt voor een ingestelde-waardecomponent waarmee u de waarde van een XML-element kunt instellen.

Als het formulier wordt gevuld met de XML, kunt u op basis van deze waarde bepaalde velden of deelvensters van het formulier verbergen/uitschakelen.

In AEM Forms OSGI- zullen wij een douane OSGi bundel moeten schrijven om waarde in XML te plaatsen. De bundel wordt geleverd als onderdeel van deze zelfstudie.
We gebruiken processtap in AEM workflow. Wij associëren de &quot;Vastgestelde Waarde van Element in de bundel van XML&quot;OSGi met deze processtap.
We moeten twee argumenten doorgeven aan de set value bundle. Het eerste argument is XPath van het element van XML de waarvan waarde moet worden geplaatst. Het tweede argument is de waarde die moet worden ingesteld.
In de bovenstaande schermafbeelding stellen we bijvoorbeeld de waarde van het element in de eerste stap in op &quot;N&quot;.
Op basis van deze waarde worden bepaalde deelvensters in de Adaptief Forms verborgen of weergegeven.
In ons voorbeeld hebben we een eenvoudig aanvraagformulier voor een time-off. De aanvrager van dit formulier vult zijn/haar naam en de datums in. Bij verzending gaat dit formulier naar &quot;admin&quot; voor revisie. Wanneer het formulier door beheerder wordt geopend, worden de velden in het eerste deelvenster uitgeschakeld. Dit omdat wij de waarde van het aanvankelijke stapelelement in XML aan &quot;N&quot;hebben geplaatst.

Op basis van de waarde van de velden voor de eerste stap wordt het tweede deelvenster weergegeven waarin de &quot;beheerder&quot; het verzoek kan goedkeuren of afwijzen

Gelieve te nemen een blik bij de regels die tegen &quot;Tijd van Gevraagd door&quot;gebied worden geplaatst gebruikend de regelredacteur.

Volg onderstaande stappen om de middelen op uw lokale systeem te implementeren:

* [De gebruikersbundel DevelopingWithService implementeren](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [De voorbeeldbundel implementeren](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dit is de aangepaste OSGI-bundel waarmee u de waarden van een element in de verzonden XML-gegevens kunt instellen

* [De inhoud van het ZIP-bestand downloaden en uitpakken](assets/setvalueassets.zip)
* Wijs uw browser aan [pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)
* Importeer en installeer de setValueWorkflow.zip. Dit heeft het model van het steekproefwerkschema.
* Wijs uw browser aan [Forms en Documenten](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Klik op Maken | Bestand uploaden
* De tijdOfRequestForm.zip uploaden
* Open de [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)
* Vul de 3 vereiste velden in en verzend
* Meld u aan als &#39;admin&#39; in bij AEM (als u dat nog niet hebt gedaan)
* Ga naar [&quot;AEM Inbox&quot;](http://localhost:4502/aem/inbox)
* Open het formulier &quot;Verzoek om een revisie op tijd uit&quot;
* De velden in het eerste deelvenster zijn uitgeschakeld. De reden hiervoor is dat het formulier wordt geopend door de controleur. U ziet ook dat het deelvenster voor het goedkeuren of afwijzen van de aanvraag nu zichtbaar is

>[!NOTE]
>
>U kunt foutopsporingslogbestand inschakelen door logger in te schakelen voor
>com.aemforms.setvalue.core.SetValueinXml
>door uw browser naar http://localhost:4502/system/console/slinglog te verwijzen

>[!NOTE]
>
>Zorg ervoor dat het pad naar het gegevensbestand in de verzendopties van het adaptieve formulier is ingesteld op &quot;Data.xml&quot;. Dit komt omdat de processtap naar een bestand met de naam Data.xml in de payload-map zoekt
