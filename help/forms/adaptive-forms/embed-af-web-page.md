---
title: Aangepaste Forms/HTML5-formulieren insluiten in webpagina
description: Configuratiestappen die nodig zijn om adaptieve Forms- of HTML5-formulieren in te sluiten in een niet-AEM webpagina.
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 8390
exl-id: 068e38df-9c71-4f55-b6d6-e1486c29d0a9
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# Adaptief formulier of HTML5-formulier insluiten in webpagina

Het ingesloten adaptieve formulier is volledig functioneel en gebruikers kunnen het formulier invullen en verzenden zonder de pagina te verlaten. Hierdoor kan de gebruiker in de context van andere elementen op de webpagina blijven en tegelijkertijd met het formulier communiceren.

In de volgende video worden de stappen beschreven die nodig zijn om een adaptief of HTML5-formulier in te sluiten in een webpagina.
Raadpleeg de [documentatie](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html) voor optimale voorwaarden, beste praktijken enz.
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=12&learn=on)

U kunt de voorbeeldbestanden downloaden die in de video worden gebruikt [van hier](assets/embedding-af-web-page.zip)

Hier volgt de code die wordt gebruikt om het adaptieve formulier op te halen en het formulier in te sluiten in de container die wordt aangeduid met de klassenaam **right**

```javascript
$(document).ready(function(){
  
    var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
    console.log(formName);
    $(".right").append(data);
      
    });
  
});
```
