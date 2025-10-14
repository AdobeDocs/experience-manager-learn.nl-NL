---
title: Waarde van Json-gegevenselement instellen in AEM Forms Workflow
description: Aangezien een adaptief formulier naar verschillende gebruikers wordt gerouteerd in de AEM-workflow, zijn er vereisten voor het verbergen of uitschakelen van bepaalde velden of deelvensters afhankelijk van de persoon die het formulier reviseert. Om aan deze gebruiksgevallen te voldoen, stellen wij typisch een waarde van een verborgen gebied in. Op basis van de waarde van dit verborgen veld kunnen bedrijfsregels worden ontworpen om de juiste deelvensters of velden te verbergen/uitschakelen.
feature: Adaptive Forms
version: Experience Manager 6.4
topic: Development
role: Developer
level: Experienced
exl-id: fbe6d341-7941-46f5-bcd8-58b99396d351
last-substantial-update: 2021-06-09T00:00:00Z
duration: 126
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '656'
ht-degree: 0%

---

# Waarde van JSON-gegevenselement instellen in AEM Forms-workflow {#setting-value-of-json-data-element-in-aem-forms-workflow}

Aangezien een adaptief formulier naar verschillende gebruikers wordt gerouteerd in de AEM-workflow, zijn er vereisten voor het verbergen of uitschakelen van bepaalde velden of deelvensters afhankelijk van de persoon die het formulier reviseert. Om aan deze gebruiksgevallen te voldoen, stellen wij typisch een waarde van een verborgen gebied in. Op basis van de waarde van dit verborgen veld kunnen bedrijfsregels worden ontworpen om de juiste deelvensters of velden te verbergen/uitschakelen.

![&#x200B; Plaatsende waarde van een element in jsgegevens &#x200B;](assets/capture-3.gif)

In AEM Forms OSGi - moeten we een aangepaste OSGi-bundel maken om de waarde van het JSON-gegevenselement in te stellen. De bundel wordt geleverd als onderdeel van deze zelfstudie.

We gebruiken Processtap in de AEM-workflow. Wij associëren de &quot;Vastgestelde Waarde van Element in Json&quot;OSGi bundel met deze processtap.

We moeten twee argumenten doorgeven aan de set value bundle. Het eerste argument is het pad naar het element waarvan de waarde moet worden ingesteld. Het tweede argument is de waarde die moet worden ingesteld.

In de bovenstaande schermafbeelding stellen we bijvoorbeeld de waarde van het element intialStep in op &quot;N&quot;

afData.afUnboundData.data.initialStep,N

In ons voorbeeld hebben we een eenvoudig aanvraagformulier voor een time-off. De aanvrager van dit formulier vult zijn/haar naam en de datums in. Bij verzending gaat dit formulier naar &quot;manager&quot; voor revisie. Wanneer de manager het formulier opent, worden de velden in het eerste deelvenster uitgeschakeld. Dit omdat wij de waarde van het aanvankelijke stapelement in de gegevens JSON aan N hebben geplaatst.

Op basis van de waarde van de velden voor de eerste stap tonen we in het venster met fiatteurs waar de &#39;manager&#39; het verzoek kan goedkeuren of afwijzen.

Gelieve te nemen een blik bij de regels die tegen &quot;Aanvankelijke Stap&quot;worden geplaatst. Op basis van de waarde van het veld initialStep ophalen we de gebruikersgegevens met behulp van het formuliergegevensmodel, vullen de desbetreffende velden in en verbergen/uitschakelen de desbetreffende deelvensters.

De elementen op uw lokale systeem implementeren:

* [DevelopingWithServiceUserBundle downloaden en implementeren](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [&#x200B; Download en stel de setvalue bundel &#x200B;](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) op. Dit is de aangepaste OSGI-bundel waarmee u de waarden van een element in de verzonden JSON-gegevens kunt instellen.

* [De inhoud van het ZIP-bestand downloaden en uitpakken](assets/set-value-jsondata.zip)
   * Punt uw browser aan [&#x200B; pakketmanager &#x200B;](http://localhost:4502/crx/packmgr/index.jsp)
      * Importeer en installeer de SetValueOfElementInJSONDataWorkflow.zip.This het pakket heeft het model van de steekproefwerkstroom en het Model van de Gegevens van de Vorm verbonden aan de vorm.

* Punt uw browser aan [&#x200B; Forms en Documenten &#x200B;](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Klik op Maken | Bestand uploaden
* Het bestand TimeOffRequestForm.zip uploaden
  **Deze vorm werd gebouwd gebruikend AEM Forms 6.4. Zorg dat je op AEM Forms 6.4 of hoger staat**
* Open de [&#x200B; vorm &#x200B;](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* Vul de begin- en einddatum in en verzend het formulier.
* Ga naar [&#x200B; &quot;Inbox&quot;](http://localhost:4502/aem/inbox)
* Open het formulier dat aan de taak is gekoppeld.
* De velden in het eerste deelvenster zijn uitgeschakeld.
* U ziet dat het deelvenster voor het goedkeuren of afwijzen van de aanvraag nu zichtbaar is.

>[!NOTE]
>
>Aangezien wij pre-bevolkt de Aangepaste Vorm gebruikend gebruikersprofiel, zorg de informatie van het admin [&#x200B; gebruikersprofiel &#x200B;](http://localhost:4502/security/users.html) ervoor. Zorg er minimaal voor dat u de veldwaarden FirstName, LastName en Email hebt ingesteld.
>U kunt zuivert registreren toelaten door registreerapparaat voor com.aemforms.setvalue.core.SetValueInJson [&#x200B; van hier &#x200B;](http://localhost:4502/system/console/slinglog) toe te laten

>[!NOTE]
>
>De bundel OSGi voor het plaatsen van waarde van gegevenselementen in Gegevens JSON steunt momenteel de capaciteit om één elementwaarde in één keer te plaatsen. Als u meerdere elementwaarden wilt instellen, moet u processtappen meerdere keren gebruiken.
>
>Zorg ervoor dat het pad naar het gegevensbestand in de verzendopties van het adaptieve formulier is ingesteld op &quot;Data.xml&quot;. De reden hiervoor is dat de code in de processtap naar een bestand met de naam Data.xml zoekt in de payload-map.
