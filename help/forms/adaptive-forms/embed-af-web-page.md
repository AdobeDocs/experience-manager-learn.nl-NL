---
title: Aangepaste Forms/HTML5-formulieren insluiten in webpagina
description: Configuratiestappen die nodig zijn om adaptieve Forms- of HTML5-formulieren in te sluiten in een niet-AEM-webpagina.
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-8390
exl-id: 068e38df-9c71-4f55-b6d6-e1486c29d0a9
last-substantial-update: 2020-06-09T00:00:00Z
duration: 398
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Adaptief formulier of HTML5-formulier insluiten in webpagina

Het ingesloten adaptieve formulier is volledig functioneel en gebruikers kunnen het formulier invullen en verzenden zonder de pagina te verlaten. Hierdoor kan de gebruiker in de context van andere elementen op de webpagina blijven en tegelijkertijd met het formulier communiceren.

In de volgende video worden de stappen beschreven die nodig zijn om een adaptief of HTML5-formulier in te sluiten in een webpagina.
Gelieve te verwijzen naar de [ documentatie ](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html?lang=nl-NL) voor beste eerste vereisten, beste praktijken etc.
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=12&learn=on)

U kunt de steekproefdossiers downloaden die in de video [ van hier worden gebruikt ](assets/embedding-af-web-page.zip)

Het volgende is de code wordt gebruikt om de adaptieve vorm te halen en de vorm in de container in te bedden die door de klassennaam **wordt geïdentificeerd recht**

```javascript
$(document).ready(function(){
  
    var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
    console.log(formName);
    $(".right").append(data);
      
    });
  
});
```
