---
title: Geolocatie-API's gebruiken in Adaptive Forms
description: Adresvelden op uw formulier invullen met de geolocatie-API's
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 50db6155-ee83-4ddb-9e3a-56e8709222db
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 0%

---

# Geolocatie-API&#39;s gebruiken in Adaptive Forms{#using-geolocation-api-s-in-adaptive-forms}

In dit artikel bekijken we het gebruik van de Google Geolocation API om velden van een adaptief formulier te vullen. Dit wordt vaak gebruikt wanneer u de huidige adresvelden op een formulier wilt vullen.

De volgende stappen zijn uitgevoerd voor het gebruik van de Geolocation API in Adaptive Forms.

1. [API-sleutel ophalen](https://developers.google.com/maps/documentation/javascript/get-api-key) vanuit Google om het Google Maps-platform te gebruiken. U kunt een proefsleutel ophalen die 1 jaar geldig is.

1. Het adaptief formulierfragment is gemaakt met velden voor het huidige adres

1. De API Geolocation is aangeroepen tijdens de klikgebeurtenis van het afbeeldingsobject van Adaptief formulier

1. De JSON-gegevens die door de API-aanroep zijn geretourneerd, zijn geparseerd en de waarden van de Adaptief-formuliervelden zijn dienovereenkomstig ingesteld.

```javascript
navigator.geolocation.getCurrentPosition(showPosition);
function showPosition(position) 
{
console.log(" I am inside the showPosition in fragment");
console.log("Latitude: " + position.coords.latitude + "Longitude " + position.coords.longitude);
var url = "https://maps.googleapis.com/maps/api/geocode/json?latlng="+position.coords.latitude+","+position.coords.longitude+"&key=<your_api_key>";
  console.log(url);
  
  $.getJSON(url,function (data, textStatus){
    
    var location=data.results[0].formatted_address;
    console.log(location);
    
    for(i=0;i<data.results[0].address_components.length;i++)
        {
          if(data.results[0].address_components[i].types[0] == "street_number")
            {
              streetNumber.value = data.results[0].address_components[i].long_name;
            }
          if(data.results[0].address_components[i].types[0] == "route")
            {
              streetName.value = data.results[0].address_components[i].long_name;
            }
            if(data.results[0].address_components[i].types[0] == "postal_code")
            {
              
              zipCode.value = data.results[0].address_components[i].long_name;
            }
            if(data.results[0].address_components[i].types[0] == "locality")
            {
              
              city.value = data.results[0].address_components[i].long_name;
            }
          if(data.results[0].address_components[i].types[0] == "administrative_area_level_1")
            {
              
              state.value = data.results[0].address_components[i].long_name;
            }
        }
    
  });
}
```

![Velden vullen met geolocatieapi](assets/capture-4.gif)

In regel 1 gebruiken we de HTML Geolocation-API om de huidige locatie op te halen. Zodra de huidige plaats wordt verkregen gaan wij de huidige plaats over om functie te tonenPosition.

In de functie showPosition gebruiken we de Google API om de adresgegevens voor de opgegeven breedte en lengte op te halen.

De JSON die door de API wordt geretourneerd, wordt vervolgens geparseerd om de velden Adaptief formulier in te stellen.

>[!NOTE]
>
>Voor testdoeleinden kunt u het HTTP-protocol met localhost gebruiken in de URL.
>
>Voor de productieserver, zult u SSL voor uw AEMServer moeten toelaten om dit vermogen te krijgen.
>
>Het voorbeeld dat aan dit artikel is gekoppeld, is getest met het Amerikaanse adres. Als u deze mogelijkheid wilt gebruiken op andere geografische locaties, moet u mogelijk de JSON-parsering afstellen.

Voer de volgende stappen uit om deze functie op uw server te plaatsen

* AEM Forms-server installeren en starten.

>!![NOTE] Deze mogelijkheid is getest op AEM Forms 6.3 en hoger
* [Google API-sleutel ophalen](https://developers.google.com/maps/documentation/javascript/get-api-key).
* [Importeer de aan dit artikel gerelateerde elementen in AEM.](assets/geolocationapi.zip)
* [Open het fragment Adaptief formulier in de bewerkingsmodus.](http://localhost:4502/editor.html/content/forms/af/currentaddressfragment.html)
* Open de regeleditor voor de component Afbeeldingskeuze.
* Vervang de &lt;your_api_key> met de Google API-sleutel.
* Sla uw wijzigingen op.
* [Een voorbeeld van het formulier bekijken](http://localhost:4502/content/dam/formsanddocuments/currentaddressfragment/jcr:content?wcmmode=disabled).
* Klik op het pictogram voor &quot;geolocatie&quot;.
* Uw formulier moet worden ingevuld met uw huidige locatie.
