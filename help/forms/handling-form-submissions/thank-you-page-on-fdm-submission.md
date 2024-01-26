---
title: Indienings-id weergeven bij het verzenden van het formulier
description: De reactie van een verzonden formuliergegevensmodel weergeven op de pagina Hartelijk dank
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13900
last-substantial-update: 2023-09-09T00:00:00Z
exl-id: 18648914-91cc-470d-8f27-30b750eb2f32
duration: 85
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# De pagina Hartelijk dank aanpassen

Wanneer u een adaptief formulier verzendt naar een REST-eindpunt, wilt u een bevestigingsbericht weergeven waarmee de gebruiker weet dat het verzenden van het formulier is geslaagd. De reactie van de POST bevat informatie over de verzending, zoals de verzendings-id, en een goed ontworpen bevestigingsbericht bevat de verzendings-id die bijdraagt tot een betere gebruikerservaring. Dit antwoord kan worden weergegeven op de pagina &quot;Bedankt&quot; die is geconfigureerd met het aangepaste formulier.

De volgende het schermschot toont een vorm wordt voorgelegd gebruikend het Model van de Gegevens van de Vorm verzendt actie met een dank u gevormde pagina

![dank-u-pagina](./assets/thank-you-page-fdm-submit.png)

De POST van een formuliergegevensmodel retourneert altijd een JSON-object in de reactie. Deze JSON is beschikbaar in de URL van de pagina Danku als een queryparameter _fdmSubmitResult_. U kunt deze queryparameter parseren en de JSON-elementen weergeven in de pagina &quot;Bedankt&quot;.
De volgende voorbeeldcode parseert de JSON-reactie om de waarde van het nummerveld te extraheren. Het juiste XML-bestand wordt vervolgens samengesteld en doorgegeven in de slingRequest om het formulier in te vullen. Deze code wordt doorgaans geschreven in de JSP van de paginacomponent die is gekoppeld aan de sjabloon Adaptief formulier.

```java
if(request.getParameter("fdmSubmitResult")!=null)
{
    String fdmSubmitResult =  request.getParameter("fdmSubmitResult");
    String status = request.getParameter("status");
    com.google.gson.JsonObject jsonObject = com.google.gson.JsonParser.parseString(fdmSubmitResult).getAsJsonObject();
    String caseNumber = jsonObject.get("result").getAsJsonObject().get("number").getAsString();
    slingRequest.setAttribute("data","<afData><afUnboundData><data><caseNumber>"+caseNumber+"</caseNumber><status>"+status+"</status></data></afUnboundData></afData>");
}
```

U wordt aangeraden de pagina Hartelijk dank te baseren op een nieuwe adaptieve formuliersjabloon waarmee u de aangepaste code kunt schrijven om de reactie uit de queryparameters te extraheren.

## De oplossing testen

Maak een adaptief formulier en configureer dit om het formulier te verzenden met de verzendactie van het formuliergegevensmodel.
[Het voorbeeldadaptieve formuliersjabloon implementeren](assets/thank-you-page-template.zip)
Een formulier voor bedankt maken op basis van deze sjabloon Koppel deze dank aan u pagina met uw hoofdformulier De JSP-code wijzigen in het dialoogvenster [createXml.jsp](http://localhost:4502/apps/thank-you-page-template/component/page/thankyoupage/createxml.jsp) om het XML-bestand te maken dat nodig is om het aangepaste formulier vooraf in te vullen.
Geef een voorbeeld van het aangepaste formulier weer en verzend het.
De pagina Hartelijk dank moet worden weergegeven en vooraf worden gevuld met gegevens die zijn opgegeven in de XML
