---
title: Items toevoegen aan keuzegroep
description: Items dynamisch toevoegen aan keuzerondjesgroepcomponent
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: User
level: Beginner
exl-id: 8fbea634-7949-417f-a4d6-9e551fff63f3
last-substantial-update: 2021-09-10T00:00:00Z
duration: 337
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '456'
ht-degree: 0%

---

# Items dynamisch toevoegen aan keuzeselectiegroep

In AEM Forms 6.5 is de mogelijkheid geïntroduceerd om items dynamisch toe te voegen aan een adaptieve Forms-keuzerondjesgroepcomponent, zoals CheckBox, Radio Button en Afbeeldingslijst.


U kunt punten toevoegen gebruikend de visuele redacteur evenals de coderedacteur afhankelijk van uw gebruiksgeval.

**Gebruikend de visuele redacteur:** u kunt de punten van de keuzevroep van de resultaten van een functievraag of de dienstvraag bevolken. U kunt bijvoorbeeld de items van de keuzegroep instellen door de reactie van een REST API-aanroep te verbruiken.

In het onderstaande schermafbeelding stellen we de opties voor de periode(jaren) van de lening in op de resultaten van een serviceoproep met de naam getLoanPeriods.

![ Redacteur van de Regel ](assets/ruleeditor.png)

**Gebruikend de coderedacteur**: Wanneer u de punten in de keuzevroep dynamisch wilt plaatsen die op de waarden in de vorm worden ingegaan. In het volgende codefragment worden bijvoorbeeld de items van het selectievakje ingesteld op de waarden die zijn ingevoerd in de velden Naam en Muis van de aanvrager van het adaptieve formulier.

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

**Toevoegend punten gebruikend de regelredacteur**

>[!VIDEO](https://video.tv.adobe.com/v/26847?quality=12&learn=on)

**Toevoegend punten die de coderedacteur** gebruiken

>[!VIDEO](https://video.tv.adobe.com/v/26848?quality=12&learn=on)

U kunt dit als volgt op uw systeem testen:

**Gebruikend de coderedacteur om punten** toe te voegen

* [De middelen downloaden](assets/usingthecodeeditor.zip)
* [ open Forms en documenten ](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Klik op &quot;Maken | Bestand uploaden&quot; en het bestand uploaden dat u in de vorige stap hebt gedownload
* [ voorproef de vormen ](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* Voer de naam van de aanvrager in en selecteer de huwelijksstatus die gehuwd moet worden
* Voer de naam van de echtgenoot in
* Klik op Volgende
* Het selectievakje moet worden ingevuld met de naam van de aanvrager en met de naam van de echtgenoot als de huwelijkse staat gehuwd is

**Gebruikend de visuele redacteur om punten** toe te voegen

* [De middelen downloaden](assets/usingthevisualeditor.zip)
* Installeer Tomcat als u dit nog niet hebt. [ Instructies om te installeren zijn hier beschikbaar ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html?lang=nl-NL)
* [Het bestand SampleRest.war uit dit ZIP-bestand in uw Tomcat-bestand implementeren](assets/sample-rest.zip)
* [ open Forms en documenten ](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Klik op &quot;Maken | Bestand uploaden&quot; en het bestand uploaden dat u in de vorige stap hebt gedownload
* [ voorproef de vormen ](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* Voer de hoeveelheid leningen en de tab uit het veld in. Dit zal de regel teweegbrengen die het gebied van de leningsperiode toont.
* Selecteer de toepasselijke leningsperiode (de posten voor de leningsperiode worden ingevuld van de rest van de oproep)
* Selecteer de rentevoet en klik op &quot;Amortization-schema aanvragen&quot;
* De afschrijvingstabel moet worden ingevuld. Het amortisatieschema wordt opgehaald met behulp van een REST-aanroep.

>[!NOTE]
> Er wordt aangenomen dat tomcat wordt uitgevoerd op poort 8080 en AEM op poort 4502
