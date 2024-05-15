---
title: Machtigingen met metagegevens in AEM Assets
description: Machtigingen met metagegevens zijn een functie waarmee toegang wordt beperkt op basis van de eigenschappen van metagegevens van elementen in plaats van de mapstructuur.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-13757
doc-type: Tutorial
last-substantial-update: 2024-05-03T00:00:00Z
exl-id: 57478aa1-c9ab-467c-9de0-54807ae21fb1
duration: 158
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '738'
ht-degree: 0%

---

# Machtigingen met metagegevens{#metadata-driven-permissions}

Machtigingen met metagegevens zijn een functie waarmee toegangsbeheerbeslissingen van AEM Assets Author kunnen worden gebaseerd op eigenschappen van metagegevens van elementen in plaats van op mapstructuur. Met dit vermogen, kunt u toegangsbeheerbeleid bepalen dat attributen zoals activa status, type, of om het even welk bezit van douanemetagegevens evalueert u bepaalt.

Laten we een voorbeeld zien. Creatieve gebruikers kunnen hun werk naar AEM Assets uploaden naar de map met betrekking tot de campagne. Dit is mogelijk een werk in uitvoering dat niet is goedgekeurd voor gebruik. We willen ervoor zorgen dat de marketeers alleen goedgekeurde middelen voor deze campagne zien. Met de eigenschap metadata kunnen we aangeven dat een element is goedgekeurd en kan worden gebruikt door de marketeers.

## Hoe het werkt

Als u Metagegevens-gestuurde machtigingen inschakelt, moet u bepalen welke metagegevenseigenschappen van elementen toegangsbeperkingen bepalen, zoals &quot;status&quot; of &quot;merk&quot;. Deze eigenschappen kunnen dan worden gebruikt om toegangsbeheeringangen tot stand te brengen die specificeren welke gebruikersgroepen toegang tot activa met specifieke bezitswaarden hebben.

## Vereisten

Toegang tot een AEM as a Cloud Service omgeving die is bijgewerkt naar de nieuwste versie is vereist voor het instellen van machtigingen voor metagegevens.

## OSGi-configuratie {#configure-permissionable-properties}

Om Metadata-Driven Toestemmingen uit te voeren moet een ontwikkelaar een configuratie OSGi opstellen om as a Cloud Service te AEM, die specifieke eigenschappen van activameta-gegevens aan macht meta-gegeven toestemmingen toelaat.

1. Bepaal welke eigenschappen van elementmetagegevens worden gebruikt voor toegangsbeheer. De eigenschapsnamen zijn de JCR-eigenschapnamen op het element `jcr:content/metadata` resource. In ons geval wordt het een eigenschap met de naam `status`.
1. Een OSGi-configuratie maken `com.adobe.cq.dam.assetmetadatarestrictionprovider.impl.DefaultRestrictionProviderConfiguration.cfg.json` in uw AEM Maven-project.
1. Plak de volgende JSON in het gemaakte bestand:

   ```json
   {
     "restrictionPropertyNames":[
       "status",
       "brand"
     ],
     "enabled":true
   }
   ```

1. Vervang de eigenschapsnamen door de vereiste waarden.

## Rechten voor basiselementen opnieuw instellen

Alvorens op beperking-gebaseerde Ingangen van het Toegangsbeheer toe te voegen, zou een nieuwe top-level ingang moeten worden toegevoegd om gelezen toegang tot alle groepen eerst te ontkennen die aan toestemmingsevaluatie voor Activa (b.v. &quot;contribuanten&quot;of gelijkaardig) onderworpen zijn:

1. Ga naar de __Gereedschappen → Beveiliging → Rechten__ scherm
1. Selecteer de __Medewerkers__ groep (of een andere aangepaste groep waartoe alle gebruikersgroepen behoren)
1. Klikken __ACE toevoegen__ rechtsboven in het scherm
1. Selecteren `/content/dam` for __Pad__
1. Enter `jcr:read` for __Rechten__
1. Selecteren `Deny` for __Machtigingstype__
1. Onder Beperkingen selecteert u `rep:ntNames` en betreden `dam:Asset` als de __Restrictiewaarde__
1. Klikken __Opslaan__

![Toegang weigeren](./assets/metadata-driven-permissions/deny-access.png)

## Toegang tot elementen via metagegevens verlenen

De ingangen van de toegangscontrole kunnen nu worden toegevoegd om gelezen toegang tot gebruikersgroepen te verlenen die op [De waarden van de metagegevens van het geconfigureerde element](#configure-permissionable-properties).

1. Ga naar de __Gereedschappen → Beveiliging → Rechten__ scherm
1. Selecteer de gebruikersgroepen die toegang moeten hebben tot de elementen
1. Klikken __ACE toevoegen__ rechtsboven in het scherm
1. Selecteren `/content/dam` (of een submap) voor __Pad__
1. Enter `jcr:read` for __Rechten__
1. Selecteren `Allow` for __Machtigingstype__
1. Onder __Beperkingen__ selecteert u een van de [De gevormde namen van het de meta-gegevensbezit van Activa in de configuratie OSGi](#configure-permissionable-properties)
1. Voer de vereiste waarde voor de eigenschap voor metagegevens in het dialoogvenster __Restrictiewaarde__ field
1. Klik op de knop __+__ pictogram om de Beperking aan de Ingang van het Toegangsbeheer toe te voegen
1. Klikken __Opslaan__

![Toegang toestaan](./assets/metadata-driven-permissions/allow-access.png)

## Machtigingen met metagegevens in feite

De voorbeeldmap bevat een aantal elementen.

![Admin View](./assets/metadata-driven-permissions/admin-view.png)

Zodra u toestemmingen vormt en de eigenschappen van activa meta-gegevens dienovereenkomstig plaatst zullen de gebruikers (Gebruiker van de Marktspeler in ons geval) slechts goedgekeurde activa zien.

![Marketingweergave](./assets/metadata-driven-permissions/marketeer-view.png)

## Voordelen en overwegingen

De voordelen van metagegevensgestuurde machtigingen zijn onder meer:

- Fijne controle over toegang tot elementen op basis van specifieke kenmerken.
- Ontkoppeling van toegangsbeheerbeleid van omslagstructuur, die voor flexibelere middelenorganisatie toestaat.
- Mogelijkheid om complexe toegangsbeheerregels te definiëren die zijn gebaseerd op meerdere metagegevenseigenschappen.

>[!NOTE]
>
> Het is belangrijk om op te merken:
> 
> - Eigenschappen van metagegevens worden aan de hand van de beperkingen beoordeeld met __Tekengelijkheid__ (`=`) (andere gegevenstypen of operatoren worden nog niet ondersteund voor groter dan (`>`) of Date, eigenschappen)
> - Om veelvoudige waarden voor een beperkingsbezit toe te staan, kunnen de extra beperkingen aan de Ingang van het Toegangsbeheer worden toegevoegd door het zelfde bezit van de &quot;Uitgezochte Type&quot;drop-down te selecteren en een nieuwe Waarde van de Beperking in te gaan (b.v. `status=approved`, `status=wip`) en klikken op &quot;+&quot; om de beperking aan de vermelding toe te voegen
> ![Meerdere waarden toestaan](./assets/metadata-driven-permissions/allow-multiple-values.png)
> - __EN beperkingen__ worden ondersteund, via meerdere beperkingen in één toegangsbeheeritem met verschillende eigenschapsnamen (bijvoorbeeld `status=approved`, `brand=Adobe`) wordt beoordeeld als een AND-voorwaarde, d.w.z. de geselecteerde gebruikersgroep krijgt leestoegang tot elementen met `status=approved AND brand=Adobe`
> ![Meerdere beperkingen toestaan](./assets/metadata-driven-permissions/allow-multiple-restrictions.png)
> - __OR-beperkingen__ worden gesteund door een nieuw Toegang van het Toegangsbeheer met een meta-gegevensbezitsbeperking toe te voegen zal een OF voorwaarde voor de ingangen, bijvoorbeeld één enkele ingang met beperking vestigen `status=approved` en één item met `brand=Adobe` wordt beoordeeld als `status=approved OR brand=Adobe`
> ![Meerdere beperkingen toestaan](./assets/metadata-driven-permissions/allow-multiple-aces.png)
