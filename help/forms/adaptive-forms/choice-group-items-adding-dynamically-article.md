---
title: Items toevoegen aan keuzegroep
seo-title: Items toevoegen aan keuzegroep
description: Items dynamisch toevoegen aan keuzerondjesgroepcomponent
seo-description: Items dynamisch toevoegen aan keuzerondjesgroepcomponent
feature: adaptieve vormen
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---



# Items dynamisch toevoegen aan keuzeselectiegroep

In AEM Forms 6.5 is de mogelijkheid geïntroduceerd om items dynamisch toe te voegen aan een adaptieve Forms-keuzerondjesgroepcomponent, zoals CheckBox, Radio Button en Afbeeldingslijst.

[Dit vermogen is beschikbaar live op de Server](https://forms.enablementadobe.com/content/samples/samples.html?query=0) van Steekproeven. Zoeken naar kaart met dynamische selectievakjes en klikken op &quot;Try It&quot;


U kunt punten toevoegen gebruikend de visuele redacteur evenals de coderedacteur afhankelijk van uw gebruiksgeval.

**De visuele editor gebruiken:** u kunt de items van de keuzegroep vullen met de resultaten van een functieaanroep of serviceaanroep. U kunt bijvoorbeeld de items van de keuzegroep instellen door de reactie van een REST API-aanroep te verbruiken.

In het onderstaande schermafbeelding stellen we de opties voor de periode(jaren) van de lening in op de resultaten van een serviceoproep met de naam getLoanPeriods.

![Regeleditor](assets/ruleeditor.png)

**De code-editor** gebruiken: Wanneer u de items in de keuzegroep dynamisch wilt instellen op basis van de waarden die u in het formulier hebt ingevoerd. In het volgende codefragment worden bijvoorbeeld de items van het selectievakje ingesteld op de waarden die zijn ingevoerd in de velden Naam en Muis van de aanvrager van het adaptieve formulier.

In het codefragment, plaatsen wij de punten van WorkingMember, die een checkbox component is. De array voor de items wordt dynamisch samengesteld door de waarden van de tekstvelden requestName en spouse van de adaptieve formulieren op te halen

```javascript
 
 if(MaritalStatus.value=="Married")
  {
WorkingMembers.items =["spouse="+spouse.value,"applicant="+applicantName.value];
  }
else
  {
    WorkingMembers.items =["applicant="+applicantName.value];
  }
```

De ingediende gegevens zijn als volgt

```xml
<afUnboundData>

<data>

<applicantName>John Jacobs</applicantName>

<MaritalStatus>Married</MaritalStatus>

<spouse>Gloria Rios</spouse>

<WorkingMembers>spouse,applicant</WorkingMembers>

</data>

</afUnboundData>
```

**Items toevoegen met de regeleditor**

>[!VIDEO](https://video.tv.adobe.com/v/26847?quality=12&learn=on)

**Items toevoegen met de code-editor**

>[!VIDEO](https://video.tv.adobe.com/v/26848?quality=12&learn=on)

U kunt dit als volgt op uw systeem testen:

**Items toevoegen met de code-editor**

* [De middelen downloaden](assets/usingthecodeeditor.zip)
* [Forms en documenten openen](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Klik op &quot;Maken | Bestand uploaden&quot; en het bestand uploaden dat u in de vorige stap hebt gedownload
* [Een voorbeeld van de formulieren bekijken](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* Voer de naam van de aanvrager in en selecteer de huwelijksstatus die gehuwd moet worden
* Voer de naam van de echtgenoot in
* Klik op Next
* Het selectievakje moet worden ingevuld met de naam van de aanvrager en met de naam van de echtgenoot als de huwelijkse staat gehuwd is

**De visuele editor gebruiken om items toe te voegen**

* [De middelen downloaden](assets/usingthevisualeditor.zip)
* Installeer Tomcat als u dit nog niet hebt. [Hier zijn instructies voor het installeren van tomcat beschikbaar](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)
* [SampleRest.war-bestand distribueren in Tomcat](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
* [Forms en documenten openen](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Klik op &quot;Maken | Bestand uploaden&quot; en het bestand uploaden dat u in de vorige stap hebt gedownload
* [Een voorbeeld van de formulieren bekijken](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* Voer de hoeveelheid leningen en de tab uit het veld in. Dit zal de regel teweegbrengen die het gebied van de leningsperiode toont.
* Selecteer de toepasselijke leningsperiode (de posten voor de leningsperiode worden ingevuld van de rest van de oproep)
* Selecteer de rentevoet en klik op &quot;Amortization-schema aanvragen&quot;
* De afschrijvingstabel moet worden ingevuld. Het amortisatieschema wordt opgehaald met behulp van een REST-aanroep.

>[!NOTE]
> Er wordt aangenomen dat tomcat wordt uitgevoerd op poort 8080 en AEM op poort 4502
