---
title: Waarde van Json-gegevenselement instellen in AEM Forms-workflow
seo-title: Waarde van Json-gegevenselement instellen in AEM Forms-workflow
description: Aangezien een adaptief formulier naar verschillende gebruikers wordt gerouteerd in AEM workflow, zijn er vereisten voor het verbergen of uitschakelen van bepaalde velden of deelvensters afhankelijk van de persoon die het formulier reviseert. Om aan deze gebruiksgevallen te voldoen, stellen wij typisch een waarde van een verborgen gebied in. Op basis van de waarde van dit verborgen veld kunnen bedrijfsregels worden ontworpen om de juiste deelvensters of velden te verbergen/uitschakelen.
seo-description: Aangezien een adaptief formulier naar verschillende gebruikers wordt gerouteerd in AEM workflow, zijn er vereisten voor het verbergen of uitschakelen van bepaalde velden of deelvensters afhankelijk van de persoon die het formulier reviseert. Om aan deze gebruiksgevallen te voldoen, stellen wij typisch een waarde van een verborgen gebied in. Op basis van de waarde van dit verborgen veld kunnen bedrijfsregels worden ontworpen om de juiste deelvensters of velden te verbergen/uitschakelen.
uuid: a4ea6aef-a799-49e5-9682-3fa3b7a442fb
feature: Adaptive Forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4
discoiquuid: 548fb2ec-cfcf-4fe2-a02a-14f267618d68
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 0%

---


# Waarde instellen van JSON-gegevenselement in AEM Forms-workflow {#setting-value-of-json-data-element-in-aem-forms-workflow}

Aangezien een adaptief formulier naar verschillende gebruikers wordt gerouteerd in AEM workflow, zijn er vereisten voor het verbergen of uitschakelen van bepaalde velden of deelvensters afhankelijk van de persoon die het formulier reviseert. Om aan deze gebruiksgevallen te voldoen, stellen wij typisch een waarde van een verborgen gebied in. Op basis van de waarde van dit verborgen veld kunnen bedrijfsregels worden ontworpen om de juiste deelvensters of velden te verbergen/uitschakelen.

![Waarde instellen voor een element in JPEG-gegevens](assets/capture-3.gif)

In AEM Forms OSGI moeten we een aangepaste OSGi-bundel schrijven om de waarde van het JSON-gegevenselement in te stellen. De bundel wordt geleverd als onderdeel van deze zelfstudie.

We gebruiken processtap in AEM workflow. Wij associëren de &quot;Vastgestelde Waarde van Element in Json&quot;OSGi bundel met deze processtap.

We moeten twee argumenten doorgeven aan de set value bundle. Het eerste argument is het pad naar het element waarvan de waarde moet worden ingesteld. Het tweede argument is de waarde die moet worden ingesteld.

In de bovenstaande schermafbeelding stellen we bijvoorbeeld de waarde van het element intialStep in op &quot;N&quot;

afData.afUnboundData.data.initialStep,N

In ons voorbeeld hebben we een eenvoudig aanvraagformulier voor een time-off. De aanvrager van dit formulier vult zijn/haar naam en de datums in. Bij verzending gaat dit formulier naar &quot;manager&quot; voor revisie. Wanneer de manager het formulier opent, worden de velden in het eerste deelvenster uitgeschakeld. Dit omdat we de waarde van het element met de eerste stap in de JSON-gegevens hebben ingesteld op N.

Op basis van de waarde van de velden voor de eerste stap tonen we in het venster met fiatteurs waar de &#39;manager&#39; het verzoek kan goedkeuren of afwijzen.

Gelieve te nemen een blik bij de regels die tegen &quot;Aanvankelijke Stap&quot;worden geplaatst. Op basis van de waarde van het veld initialStep ophalen we de gebruikersgegevens met behulp van het formuliergegevensmodel, vullen de desbetreffende velden in en verbergen/uitschakelen de desbetreffende deelvensters.

De elementen op uw lokale systeem implementeren:

* [DevelopingWithServiceUserBundle downloaden en implementeren](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Download en implementeer de setvalue-bundel](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dit is de aangepaste OSGI-bundel waarmee u de waarden van een element in de verzonden JSON-gegevens kunt instellen.

* [De inhoud van het ZIP-bestand downloaden en uitpakken](assets/set-value-jsondata.zip)
   * Wijs uw browser aan [pakketmanager](http://localhost:4502/crx/packmgr/index.jsp)
      * Importeer en installeer de SetValueOfElementInJSONDataWorkflow.zip.This het pakket heeft het model van de steekproefwerkstroom en het Model van de Gegevens van de Vorm verbonden aan de vorm.

* Wijs uw browser aan [Forms en Documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Klik op Maken | Bestand uploaden
* Het bestand TimeOffRequestForm.zip uploaden
   **Dit formulier is gemaakt met AEM Forms 6.4. Controleer of je op AEM Forms 6.4 of hoger bent**
* Open het [formulier](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* Vul de begin- en einddatum in en verzend het formulier.
* Naar [&quot;Inbox&quot;](http://localhost:4502/aem/inbox)
* Open het formulier dat aan de taak is gekoppeld.
* De velden in het eerste deelvenster zijn uitgeschakeld.
* U ziet dat het deelvenster voor het goedkeuren of afwijzen van de aanvraag nu zichtbaar is.

>[!NOTE]
>
>Aangezien het adaptieve formulier al wordt ingevuld met behulp van een gebruikersprofiel, moet u de [gegevens van het gebruikersprofiel ](http://localhost:4502/security/users.html) beheren. Zorg er minimaal voor dat u de veldwaarden FirstName, LastName en Email hebt ingesteld.
>U kunt registratie van foutopsporing inschakelen door logger voor com.aemforms.setvalue.core.SetValueInJson [hier](http://localhost:4502/system/console/slinglog) in te schakelen

>[!NOTE]
>
>De bundel OSGi voor het plaatsen van waarde van gegevenselementen in Gegevens JSON steunt momenteel de capaciteit om één elementwaarde in één keer te plaatsen. Als u meerdere elementwaarden wilt instellen, moet u processtappen meerdere keren gebruiken.
>
>Zorg ervoor dat het pad naar het gegevensbestand in de verzendopties van het adaptieve formulier is ingesteld op &quot;Data.xml&quot;. De reden hiervoor is dat de code in de processtap naar een bestand met de naam Data.xml zoekt in de payload-map.
