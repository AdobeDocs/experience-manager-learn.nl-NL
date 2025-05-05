---
title: Automatische tests gebruiken met AEM Adaptive Forms
description: Automatisch testen van Adaptive Forms met Calvin SDK
feature: Adaptive Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 5a1364f3-e81c-4c92-8972-4fdc24aecab1
last-substantial-update: 2020-09-10T00:00:00Z
duration: 101
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 0%

---

# Automatische tests gebruiken met AEM Adaptive Forms {#using-automated-tests-with-aem-adaptive-forms}

Automatisch testen van Adaptive Forms met Calvin SDK

Calvin SDK is een hulpprogramma-API waarmee ontwikkelaars van Adaptive Forms Adaptive Forms kunnen testen. Calvin SDK wordt gebouwd bovenop het [ Hobbes.js testende kader ](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=nl-NL). Calvin SDK is verkrijgbaar met AEM Forms 6.3 en hoger.

In deze zelfstudie maakt u het volgende:

* Testsuite
* Testsuite bevat een of meer testgevallen
* Testgevallen bevatten een of meer acties

## Aan de slag {#getting-started}

[ Download en installeer Assets gebruikend de Manager van het Pakket ](assets/testingadaptiveformsusingcalvinsdk1.zip) Het pakket bevat steekproefmanuscripten en verscheidene Adaptieve Forms.Deze Aangepaste Forms wordt gebouwd gebruikend versie AEM Forms 6.3. Het wordt aanbevolen nieuwe formulieren te maken die specifiek zijn voor uw versie van AEM Forms als u deze test op AEM Forms 6.4 of hoger. De voorbeeldscripts tonen aan dat er verschillende Calvin SDK API&#39;s beschikbaar zijn om Adaptive Forms te testen. De algemene stappen voor het testen van AEM Adaptive Forms zijn:

* Navigeer naar het formulier dat u wilt testen
* Waarde van veld instellen
* Het adaptieve formulier verzenden
* Controleren op foutberichten

De voorbeeldscripts in het pakket tonen alle bovenstaande handelingen aan.
Bekijk de code van `mortgageForm.js`

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

De bovenstaande code maakt een nieuwe testsuite.

* De naam van de TestSuite is in dit geval &#39; `Mortgage Form Test` &#39;.
* Opgegeven is het absolute pad in AEM naar het JS-bestand dat de testsuite bevat.
* De registerparameter wanneer deze is ingesteld op &#39; `true` &#39;, maakt de testsuite beschikbaar in de testgebruikersinterface.

```javascript
.addTestCase(new hobs.TestCase("Calculate amount to borrow")
        // navigate to the mortgage form  which is to be tested
        .navigateTo("/content/forms/af/cal/mortgageform.html?wcmmode=disabled")
  .asserts.isTrue(function () {
            return calvin.isFormLoaded()
        })
```

>[!NOTE]
>
>Als u deze functie test op AEM Forms 6.4 of hoger, maakt u een nieuw adaptief formulier en gebruikt u dit voor de test. Het gebruik van het adaptieve formulier dat bij het pakket wordt geleverd, wordt niet aanbevolen.

Testgevallen kunnen worden toegevoegd aan een testsuite die kan worden uitgevoerd op een adaptief formulier.

* Als u een testcase wilt toevoegen aan een testsuite, gebruikt u de methode `addTestCase` van het object TestSuite.
* De methode `addTestCase` neemt een voorwerp TestCase als parameter.
* Gebruik de methode `hobs.TestCase(..)` om TestCase te maken.
* Nota: De eerste parameter is de naam van het Geval van de Test dat in UI zal verschijnen.
* Nadat u een testcase hebt gemaakt, kunt u vervolgens handelingen toevoegen aan uw testcase.
* Acties, waaronder `navigateTo` en `asserts.isTrue` , kunnen als handelingen aan het testhoofdlettergebruik worden toegevoegd.

## De geautomatiseerde tests uitvoeren {#running-the-automated-tests}

[ Openthetestsuite ](http://localhost:4502/libs/granite/testing/hobbes.html) breidt de Reeks van de Test uit en stelt de tests in werking. Als alles goed werkt, ziet u de volgende uitvoer.

![ calvinsdk ](assets/calvinimage.png)

## Probeer de testsuites van het monster uit {#try-out-the-sample-test-suites}

Als onderdeel van de monsterverpakking zijn er drie extra testreeksen. U kunt ze uitproberen door de desbetreffende bestanden op te nemen in het bestand js.txt van de clientbibliotheek, zoals hieronder wordt weergegeven:

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
