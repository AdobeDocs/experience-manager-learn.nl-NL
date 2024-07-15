---
title: Aangepaste functies in AEM Forms
description: Aangepaste functies maken en gebruiken in adaptief formulier
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
jira: KT-9685
exl-id: 07fed661-0995-41ab-90c4-abde35a14a4c
last-substantial-update: 2021-06-09T00:00:00Z
duration: 286
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 0%

---

# Aangepaste functies

AEM Forms 6.5 introduceerde de capaciteit om de functies van JavaScript te bepalen die in het bepalen van complexe bedrijfsregels kunnen worden gebruikt gebruikend de regelredacteur.
AEM Forms biedt een aantal van dergelijke aangepaste functies uit de verpakking, maar u hebt de behoefte om uw eigen aangepaste functies te definiëren en deze in meerdere formulieren te gebruiken.

Voer de volgende stappen uit om uw eerste aangepaste functie te definiëren:
* [ Logboek in crx ](http://localhost:4502/crx/de/index.jsp#/apps/experience-league/clientlibs)
* Een nieuwe map maken onder de apps met de naam Experience-League (deze mapnaam kan een naam naar keuze zijn)
* Sla uw wijzigingen op.
* Maak in de map Experience-League een nieuw knooppunt van het type cq:ClientLibraryFolder genaamd clientlibs.
* Selecteer de map Clientlibs die u net hebt gemaakt en voeg de eigenschappen allowProxy en category toe, zoals weergegeven in de schermafbeelding, en sla uw wijzigingen op.

![ cliënt-lib ](assets/custom-functions.png)
* Creeer een omslag genoemd **js** onder de **clientlibs** omslag
* Creeer een dossier genoemd **functions.js** onder de **js** omslag
* Creeer een dossier genoemd **js.txt** onder de **clientlibs** omslag. Sla uw wijzigingen op.
* De mapstructuur moet eruitzien als de onderstaande schermafbeelding.

![ Redacteur van de Regel ](assets/folder-structure.png)

* Dubbelklik op functions.js om de editor te openen.
Kopieer de volgende code naar functions.js en sla uw wijzigingen op.

```javascript
/**
* Get List of County names
* @name getCountyNamesList Get list of county names
* @return {OPTIONS} drop down options 
 */
function getCountyNamesList()
{
    var countyNames= [];
    countyNames[0] = "Santa Clara";
    countyNames[1] = "Alameda";
    countyNames[2] = "Buxor";
    countyNames[3] = "Contra Costa";
    countyNames[4] = "Merced";

    return countyNames;

}
/**
* Covert UTC to Local Time
* @name convertUTC Convert UTC Time to Local Time
* @param {string} strUTCString in Stringformat
* @return {string}
*/
function convertUTC(strUTCString)
{
    var dt = new Date(strUTCString);
    console.log(dt.toLocaleString());
    return dt.toLocaleString();
}
```

Gelieve te verwijzen [ naar jsdoc ](https://jsdoc.app/index.html) voor meer details bij het annoteren van functies javascript.
De bovenstaande code heeft twee functies:
**getCountyNamesList** - keert een serie van koord terug
**convertUTC** - zet UTC tijdstempel in lokale tijdzone om

Open js.txt, plak de volgende code en sla uw wijzigingen op.

```javascript
#base=js
functions.js
```

De regel #base=js geeft aan in welke map de JavaScript-bestanden zich bevinden.
De onderstaande regels geven de locatie van het JavaScript-bestand aan ten opzichte van de basislocatie.

Als u problemen in het creëren van de douanefuncties hebt, voel vrij om [ dit pakket ](assets/custom-functions.zip) in uw AEM instantie te downloaden en te installeren.

## De aangepaste functies gebruiken

De volgende video begeleidt u door de stappen betrokken bij het gebruiken van douanefunctie in de regelredacteur van een adaptief vorm
>[!VIDEO](https://video.tv.adobe.com/v/340305?quality=12&learn=on)
