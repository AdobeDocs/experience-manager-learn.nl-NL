---
title: Automatische tests gebruiken met AEM adaptieve Forms
description: Automatisch testen van Adaptive Forms met Calvin SDK
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 5a1364f3-e81c-4c92-8972-4fdc24aecab1
last-substantial-update: 2020-09-10T00:00:00Z
duration: 117
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 0%

---

# Automatische tests gebruiken met AEM adaptieve Forms {#using-automated-tests-with-aem-adaptive-forms}

Automatisch testen van Adaptive Forms met Calvin SDK

Calvin SDK is een hulpprogramma-API waarmee Adaptive Forms-ontwikkelaars Adaptive Forms kunnen testen. Calvin SDK is bovenop de [Het testframework Hobbes.js](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html). Calvin SDK is beschikbaar bij AEM Forms 6.3 en hoger.

In deze zelfstudie maakt u het volgende:

* Testsuite
* Testsuite bevat een of meer testgevallen
* Testgevallen bevatten een of meer acties

## Aan de slag {#getting-started}

[De middelen downloaden en installeren met Package Manager](assets/testingadaptiveformsusingcalvinsdk1.zip)Het pakket bevat voorbeeldscripts en diverse Adaptieve Forms. Deze Adaptieve Forms is gebouwd met AEM Forms 6.3-versie. Het wordt aanbevolen nieuwe formulieren te maken die specifiek zijn voor uw versie van AEM Forms als u deze test op AEM Forms 6.4 of hoger. De voorbeeldscripts tonen verschillende Calvin SDK API&#39;s die beschikbaar zijn om Adaptive Forms te testen. De algemene stappen voor het testen AEM Adaptive Forms zijn:

* Navigeer naar het formulier dat u wilt testen
* Waarde van veld instellen
* Het adaptieve formulier verzenden
* Controleren op foutberichten

De voorbeeldscripts in het pakket tonen alle bovenstaande handelingen aan.
Laten we de code van `mortgageForm.js`

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

De bovenstaande code maakt een nieuwe testsuite.

* De naam van de TestSuite in dit geval is &#39; `Mortgage Form Test` &quot;.
* Opgegeven is het absolute pad in AEM naar het JS-bestand dat de testsuite bevat.
* De parameter register wanneer ingesteld op &#39; `true` &#39;, maakt de testsuite beschikbaar in de testgebruikersinterface.

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

* Als u een testcase wilt toevoegen aan een testsuite, gebruikt u de `addTestCase` methode van het object TestSuite.
* De `addTestCase` methode neemt een Voorwerp TestCase als parameter.
* Als u TestCase wilt maken, gebruikt u de opdracht `hobs.TestCase(..)` methode.
* Nota: De eerste parameter is de naam van het Geval van de Test dat in UI zal verschijnen.
* Nadat u een testcase hebt gemaakt, kunt u vervolgens handelingen toevoegen aan uw testcase.
* Acties inclusief `navigateTo`, `asserts.isTrue` kan als acties aan het testgeval worden toegevoegd.

## De geautomatiseerde tests uitvoeren {#running-the-automated-tests}

[Openthetestsuite](http://localhost:4502/libs/granite/testing/hobbes.html)Breid de Testsuite uit en voer de tests uit. Als alles goed werkt, ziet u de volgende uitvoer.

![calvinsdk](assets/calvinimage.png)

## Probeer de testsuites van het monster uit {#try-out-the-sample-test-suites}

Als onderdeel van de monsterverpakking zijn er drie extra testreeksen. U kunt ze uitproberen door de desbetreffende bestanden op te nemen in het bestand js.txt van de clientbibliotheek, zoals hieronder wordt weergegeven:

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
