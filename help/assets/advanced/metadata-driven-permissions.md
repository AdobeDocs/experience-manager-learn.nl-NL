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

Toegang tot een AEM as a Cloud Service-omgeving die is bijgewerkt naar de nieuwste versie is vereist voor het instellen van machtigingen voor metagegevens.

## OSGi-configuratie {#configure-permissionable-properties}

Om Metadata-Driven Toestemmingen uit te voeren moet een ontwikkelaar een configuratie OSGi aan AEM as a Cloud Service opstellen, die specifieke eigenschappen van activameta-gegevens aan macht meta-gegeven toestemmingen toelaat.

1. Bepaal welke eigenschappen van elementmetagegevens worden gebruikt voor toegangsbeheer. De eigenschapsnamen zijn de JCR-eigenschapnamen op de `jcr:content/metadata` -bron van het element. In ons geval wordt het een eigenschap met de naam `status` .
1. Creeer een configuratie OSGi `com.adobe.cq.dam.assetmetadatarestrictionprovider.impl.DefaultRestrictionProviderConfiguration.cfg.json` in uw AEM Maven project.
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

Alvorens op beperking-gebaseerde Ingangen van het Toegangsbeheer toe te voegen, zou een nieuwe top-level ingang moeten worden toegevoegd om gelezen toegang tot alle groepen eerst te ontkennen die aan toestemmingsevaluatie voor Assets (b.v. &quot;contribuanten&quot;of gelijkaardig) onderworpen zijn:

1. Navigeer aan __Hulpmiddelen → Veiligheid → Het scherm van Toestemmingen__
1. Selecteer de __Medewerkers__ groep (of andere douanegroep dat alle gebruikersgroepen tot behoren)
1. Klik __ACE__ in de hogere juiste hoek van het scherm toevoegen
1. Selecteer `/content/dam` voor __Weg__
1. Ga `jcr:read` voor __Bevoegdheden__ in
1. Selecteer `Deny` voor __Type van Toestemming__
1. Onder Beperkingen, selecteer `rep:ntNames` en ga `dam:Asset` als __Waarde van de Beperking__ in
1. Klik __sparen__

![ ontken Toegang ](./assets/metadata-driven-permissions/deny-access.png)

## Toegang tot elementen via metagegevens verlenen

De ingangen van de toegangscontrole kunnen nu worden toegevoegd om gelezen toegang tot gebruikersgroepen te verlenen die op de [ worden gebaseerd gevormde waarden van het bezit van meta-gegevens van Activa ](#configure-permissionable-properties).

1. Navigeer aan __Hulpmiddelen → Veiligheid → Het scherm van Toestemmingen__
1. Selecteer de gebruikersgroepen die toegang moeten hebben tot de elementen
1. Klik __ACE__ in de hogere juiste hoek van het scherm toevoegen
1. Selecteer `/content/dam` (of een subfolder) voor __Weg__
1. Ga `jcr:read` voor __Bevoegdheden__ in
1. Selecteer `Allow` voor __Type van Toestemming__
1. Onder __Beperkingen__, selecteer één van de [ gevormde namen van het de meta-gegevensbezit van Activa in de configuratie OSGi ](#configure-permissionable-properties)
1. Ga de vereiste waarde van het meta-gegevensbezit op het __gebied van de Waarde van de Beperking__ in
1. Klik op het pictogram __+__ om de beperking toe te voegen aan het toegangbeheeritem
1. Klik __sparen__

![ staat Toegang ](./assets/metadata-driven-permissions/allow-access.png) toe

## Machtigingen met metagegevens in feite

De voorbeeldmap bevat een aantal elementen.

![ Admin Mening ](./assets/metadata-driven-permissions/admin-view.png)

Zodra u toestemmingen vormt en de eigenschappen van activa meta-gegevens dienovereenkomstig plaatst zullen de gebruikers (Gebruiker van de Marktspeler in ons geval) slechts goedgekeurde activa zien.

![ Mening van de Marktspeler ](./assets/metadata-driven-permissions/marketeer-view.png)

## Voordelen en overwegingen

De voordelen van metagegevensgestuurde machtigingen zijn onder meer:

- Fijne controle over toegang tot elementen op basis van specifieke kenmerken.
- Ontkoppeling van toegangsbeheerbeleid van omslagstructuur, die voor flexibelere middelenorganisatie toestaat.
- Mogelijkheid om complexe toegangsbeheerregels te definiëren die zijn gebaseerd op meerdere metagegevenseigenschappen.

>[!NOTE]
>
> Het is belangrijk om op te merken:
> 
> - De eigenschappen van meta-gegevens worden geëvalueerd tegen de beperkingen gebruikend __gelijkheid van het Koord__ (`=`) (andere gegevenstypes of exploitanten worden nog niet gesteund, voor groter dan (`>`) of de eigenschappen van de Datum)
> - Om veelvoudige waarden voor een beperkingsbezit toe te staan, kunnen de extra beperkingen aan de ingang van het Toegangsbeheer worden toegevoegd door het zelfde bezit van de &quot;Uitgezochte Type&quot;drop-down te selecteren en een nieuwe Waarde van de Beperking (b.v. `status=approved`, `status=wip`) in te gaan en &quot;+&quot;te klikken om de beperking aan de ingang toe te voegen
> ![Meerdere waarden toestaan ](./assets/metadata-driven-permissions/allow-multiple-values.png)
> - __EN de beperkingen__ worden gesteund, via veelvoudige beperkingen in één enkel Ingang van het Toegangsbeheer met verschillende bezitsnamen (b.v. `status=approved`, `brand=Adobe`) zullen als EN voorwaarde worden geëvalueerd, d.w.z. de geselecteerde gebruikersgroep zal lees toegang tot activa met `status=approved AND brand=Adobe` worden verleend
> ![Meerdere beperkingen toestaan ](./assets/metadata-driven-permissions/allow-multiple-restrictions.png)
> - __OF de beperkingen__ worden gesteund door een nieuw Toegang van het Toegangsbeheer met een beperking van het meta-gegevensbezit toe te voegen zullen een OF voorwaarde voor de ingangen, b.v. één enkel punt met beperking `status=approved` vestigen en één enkel punt met `brand=Adobe` zal als `status=approved OR brand=Adobe` worden geëvalueerd
> ![Meerdere beperkingen toestaan ](./assets/metadata-driven-permissions/allow-multiple-aces.png)
