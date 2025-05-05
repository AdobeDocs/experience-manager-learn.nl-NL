---
title: AEM Forms integreren met SendGrid
description: Gebruik van AEM Forms het op de cloud gebaseerde e-mailleveringsplatform van SengGrid.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-13605
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-07-14T00:00:00Z
exl-id: 62b73f4b-69d8-4ede-9d57-3d6472d25d5a
duration: 118
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 0%

---

# AEM Forms integreren met SendGrid

Welkom bij deze technische handleiding, waar we het verzenden van e-mails met dynamische SendGrid-sjablonen vanuit AEM Forms zullen onderzoeken. Met deze handleiding krijgt u een duidelijk inzicht in hoe u dynamische sjablonen kunt gebruiken om uw e-mailinhoud op effectieve wijze aan te passen.

Met dynamische sjablonen kunt u e-mailsjablonen maken die verschillende inhoud kunnen weergeven voor ontvangers op basis van de gegevens die in het adaptieve formulier zijn vastgelegd. Door personalisatievariabelen te gebruiken, kunt u gerichte en aangepaste e-mailervaringen leveren die met uw publiek passen.

Bovendien zullen wij in het gebruik van het dossier van Swagger delven, dat u toelaat om uw e-mails verder aan te passen door de naam en het e-mailadres van de klant te omvatten, evenals het selecteren van het aangewezen dynamische e-mailmalplaatje.

Volg de stapsgewijze instructies in dit document om de kracht van dynamische SendGrid-sjablonen en AEM Forms te benutten en uw e-mailcommunicatie naar een hoger niveau van betrokkenheid en relevantie te tillen. Laten we beginnen!

## Vereisten

Voordat u verdergaat met het verzenden van e-mails met gebruik van dynamische SendGrid-sjablonen uit AEM Forms, moet u controleren of aan de volgende voorwaarden is voldaan:

1. **SendGrid Rekening**: Teken omhoog voor een rekening SendGrid in [ https://sendgrid.com ](https://sendgrid.com) om tot hun e-mailleveringsdiensten toegang te hebben. U hebt de accountgegevens nodig om SendGrid te kunnen integreren met AEM Forms.
1. **Familiariteit met het Creëren van Gegevensbronnen**: Heb een werkende kennis van het creëren van gegevensbronnen in AEM Forms. Indien nodig, verwijs naar de documentatie bij [ het creëren van gegevensbronnen ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=nl-NL) voor gedetailleerde instructies.
1. **Familiariteit met het Model van de Gegevens van de Vorm**: Begrijp het concept het Model van de Gegevens van de Vorm in AEM Forms. Indien vereist, herzie de documentatie over [ het creëren van modellen van vormgegevens ](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=nl-NL) om u het noodzakelijke begrip te verzekeren.

Door aan deze voorwaarden te voldoen, beschikt u over de essentiële kennis en bronnen om e-mails effectief te verzenden met gebruik van dynamische SendGrid-sjablonen uit AEM Forms.

## Voorbeeldelementen

De voorbeeldmiddelen die bij dit artikel worden geleverd, zijn onder meer:

* **[het dossier van de Tagger](assets/SendGridWithDynamicTemplate.yaml)**: Dit dossier staat u toe om e-mail te verzenden gebruikend een dynamisch e-mailmalplaatje. Het biedt de vereiste specificaties en configuraties voor integratie met SendGrid en AEM Forms voor naadloze e-maillevering.

U kunt het aangeboden Swagger-bestand gebruiken als referentie of beginpunt voor het implementeren van e-mailfunctionaliteit met dynamische sjablonen.

## Testinstructies

Voer de volgende stappen uit om de in deze handleiding beschreven functionaliteit te testen:

1. Download het [ die&rbrace; kwikdossier ](assets/SendGridWithDynamicTemplate.yaml) in de activa omslag wordt verstrekt.
2. Maak een herstelbare gegevensbron met behulp van het gedownloade wagerbestand en uw SendGrid-referenties.
3. Maak een formuliergegevensmodel op basis van de herstelde gegevensbron.
4. Roep de `mail/send` POST-bewerking van het formuliergegevensmodel aan volgens uw vereisten. U kunt bijvoorbeeld de e-mail activeren door erop te klikken of deze opnemen in de AEM Forms-workflow.

De steekproeflading voor de dienst is als volgt. Vervang de waarden van de plaatsaanduiding door uw eigen gegevens:

```json
{
    "sendgridpayload": {
        "from": {
            "email": "gs@xyz.com"
        },
        "personalizations": [{
            "to": [{
                "email": "johndoe@xyz.com"
            }],
            "dynamic_template_data": {
                "customerName": "John Doe"
            }
        }],
        "template_id": "d-72aau292a3bd60b5300c"
    }
}
```

Zorg ervoor dat de `template_id` overeenkomt met de id van uw dynamische e-mailsjabloon SendGrid en dat de e-mailadressen geldig zijn en door SendGrid worden geverifieerd. Met de waarden in de sectie `personalizations` kunt u de e-mail aanpassen met de gegevens die door de gebruiker zijn ingevoerd in het adaptieve formulier.

Door deze stappen te volgen en de geleverde lading aan te passen, kunt u de integratie van dynamische malplaatjes SendGrid met AEM Forms effectief testen.
