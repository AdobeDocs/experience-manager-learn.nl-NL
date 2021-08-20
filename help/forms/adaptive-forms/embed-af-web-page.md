---
title: Aangepaste Forms/HTML5-formulieren insluiten in webpagina
description: Configuratiestappen die nodig zijn om adaptieve Forms- of HTML5-formulieren in te sluiten in een niet-AEM webpagina.
feature: Adaptieve Forms
type: Tutorial
version: 6.4,6.5
topic: Ontwikkeling
role: Developer
level: Beginner
kt: 8390
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---


# Adaptief formulier of HTML5-formulier insluiten in webpagina

Het ingesloten adaptieve formulier is volledig functioneel en gebruikers kunnen het formulier invullen en verzenden zonder de pagina te verlaten. Hierdoor kan de gebruiker in de context van andere elementen op de webpagina blijven en tegelijkertijd met het formulier communiceren.

In de volgende video worden de stappen beschreven die nodig zijn om een adaptief of HTML5-formulier in te sluiten in een webpagina.
Raadpleeg de [documentatie](https://experienceleague.adobe.com/docs/experience-manager-64/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html?lang=en) voor de beste voorwaarden, aanbevolen procedures, enz.
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=9&learn=on)

U kunt de voorbeeldbestanden die worden gebruikt in de video [hier downloaden](assets/embedding-af-web-page.zip)

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














