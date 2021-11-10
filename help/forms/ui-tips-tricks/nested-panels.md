---
title: Navigeren door geneste deelvensters
description: Navigeren door geneste deelvensters
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9335
source-git-commit: 20cae7a327131927f831ae9c49fb5eebfb00f5c4
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 0%

---

# Navigatietabels met meerdere deelvensters

Wanneer uw formulier navigatietabels links heeft en een van de tabbladen meerdere deelvensters bevat, kunt u de titel van de onderliggende deelvensters verbergen en toch tussen de tabbladen en de onderliggende deelvensters van deze tabbladen navigeren

[Deze functie is actief in dit formulier](https://forms.enablementadobe.com/content/forms/af/testnav1.html)




## Adaptief formulier maken

Maak een adaptief formulier met de volgende structuur. Het hoofddeelvenster bevat onderliggende deelvensters die links als tabbladen worden weergegeven. Sommige van deze &quot;**tabs**&quot; hebt extra onderliggende deelvensters. Het tabblad Familie heeft bijvoorbeeld twee onderliggende deelvensters: Echtgenoot en Onderliggende.

Er wordt ook een werkbalk toegevoegd onder de FormContainer met de knoppen Vorige en Volgende

![werkbalkafstand](assets/multiple-panels.png)



Standaard worden in dit formulier alle deelvensters aan de linkerkant weergegeven en wordt vervolgens van de ene tab naar de andere gegaan wanneer u op de volgende knop klikt.

Als u dit standaardgedrag wilt wijzigen, moet u het volgende doen:

>[!VIDEO](https://video.tv.adobe.com/v/338369?quality=9&learn=on)


Voeg de volgende code toe de klikgebeurtenis van **Volgende** knop met de code-editor

```javascript
window.guideBridge.setFocus(null, 'nextItemDeep', true);
```

Voeg de volgende code toe de klikgebeurtenis van **Vorige** knop met de code-editor

```javascript
window.guideBridge.setFocus(null, 'prevItemDeep', true);
```

Met de bovenstaande code kunt u gemakkelijk navigeren tussen de tabbladen en de onderliggende deelvensters van elk tabblad.

## De titel van onderliggende deelvensters verbergen

Gebruik de stijleditor om de titel van de onderliggende tabbladen te verbergen.

>[!VIDEO](https://video.tv.adobe.com/v/338370?quality=9&learn=on)

>[!NOTE]
>
>De in dit artikel beschreven mogelijkheid werkt niet op het laatste tabblad. Als het tabblad Adres bijvoorbeeld onderliggende deelvensters bevat, werkt deze functionaliteit niet.
