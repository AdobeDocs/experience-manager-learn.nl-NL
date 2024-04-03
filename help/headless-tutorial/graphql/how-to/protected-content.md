---
title: Beveiligde inhoud in AEM headless
description: Leer hoe u inhoud in AEM headless beveiligt.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer, Architect
level: Intermediate
jira: KT-15233
last-substantial-update: 2024-04-01T00:00:00Z
source-git-commit: c498783aceaf3bb389baaeaeefbe9d8d0125a82e
workflow-type: tm+mt
source-wordcount: '992'
ht-degree: 0%

---


# Inhoud in AEM zonder kop beschermen

Het is van cruciaal belang dat u de integriteit en beveiliging van uw gegevens waarborgt wanneer u AEM inhoud zonder koppen vanuit AEM Publiceren aanbiedt. Zo kunt u de inhoud die wordt aangeboden door AEM GraphQL API-eindpunten zonder koppen beveiligen.

Deze Hoe kan ik-onderwerpen bestrijken niet:

- Het beveiligen van de eindpunten direct, maar concentreert zich in plaats daarvan op het beveiligen van de inhoud die zij leveren.
- Verificatie voor AEM Publiceren of het verkrijgen van aanmeldingstokens. Verificatiemethoden en het doorgeven van referenties zijn afhankelijk van individuele gevallen en implementaties.

## Gebruikersgroepen

Ten eerste moeten we een [gebruikersgroep](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/accessing/aem-users-groups-and-permissions) met de gebruikers die toegang moeten hebben tot de beveiligde inhoud.

![Gebruikersgroep voor inhoud zonder hoofd AEM](./assets/protected-content/user-groups.png){align="center"}

Gebruikersgroepen wijzen toegang toe aan inhoud zonder kop, waaronder Content Fragments of andere middelen waarnaar wordt verwezen.

1. Aanmelden bij AEM auteur als een **gebruikersbeheerder**.
1. Navigeren naar **Gereedschappen** > **Beveiliging** > **Groepen**.
1. Selecteren **Maken** in de rechterbovenhoek.
1. In de **Details** tabblad, geeft u de **Groep-id** en **Groepsnaam**.
   - De Groep ID en de Naam van de Groep kunnen om het even wat zijn, maar in dit voorbeeld gebruikt de naam **Gebruikers van de API voor AEM zonder hoofd**.
1. Selecteren **Opslaan en sluiten**.
1. Selecteer de nieuwe groep en kies **Activeren** in de actiebalk.

Als u verschillende toegangsniveaus nodig hebt, maakt u meerdere gebruikersgroepen die aan verschillende inhoud kunnen worden gekoppeld.

### Gebruikers toevoegen aan gebruikersgroepen

Als u AEM GraphQL API-verzoeken zonder koppen toegang wilt verlenen tot beveiligde inhoud, kunt u de aanvraag zonder koppen koppelen aan een gebruiker die tot een specifieke gebruikersgroep behoort. Hier volgen twee veelvoorkomende benaderingen:

1. **AEM as a Cloud Service [technische rekeningen](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials):**
   - Maak een technisch account in de AEM as a Cloud Service ontwikkelaarsconsole.
   - Meld u eenmaal aan bij AEM auteur met de technische account.
   - De technische account toevoegen aan de gebruikersgroep via **Gereedschappen > Beveiliging > Groepen > AEM gebruikers van de headless API > Leden**.
   - **Activeren** zowel de gebruiker van de technische account als de gebruikersgroep voor AEM publiceren.
   - Deze methode is vereist dat de headless cliënt de Credentials van de Dienst niet aan de gebruiker blootstelt, aangezien zij geloofsbrieven voor een specifieke gebruiker zijn, en niet zouden moeten worden gedeeld.

   ![Beheer van technische accountgroepen AEM](./assets/protected-content/group-membership.png){align="center"}

2. **Benoemde gebruikers:**
   - Verifieer genoemde gebruikers en voeg hen direct aan de gebruikersgroep toe bij AEM publiceren.
   - Deze methode vereist de headless cliënt om gebruikersgeloofsbrieven met AEM te verifiëren Publish, een AEM login of toegangstoken te verkrijgen, en dit teken voor verdere verzoeken aan AEM te gebruiken. De details over hoe dit te bereiken zijn niet in deze manier van doen en zijn afhankelijk van de uitvoering.

## Inhoudsfragmenten beschermen

Het beschermen van inhoudsfragmenten is van essentieel belang voor de beveiliging van de inhoud zonder kop en wordt bereikt door de inhoud te koppelen aan een gesloten gebruikersgroep (CUG). Wanneer een gebruiker een aanvraag indient voor de AEM GraphQL API zonder koppen, wordt de geretourneerde inhoud gefilterd op basis van de CUG&#39;s van de gebruiker.

![AEM CUG&#39;s zonder koppen](./assets/protected-content/cugs.png){align="center"}

Voer de volgende stappen uit om dit te bereiken [Gesloten gebruikersgroepen (CUG&#39;s)](https://experienceleague.adobe.com/en/docs/experience-manager-learn/assets/advanced/closed-user-groups).

1. Aanmelden bij AEM auteur als een **DAM-gebruiker**.
2. Navigeren naar **Middelen > Bestanden** en selecteert u de **map** met de te beschermen inhoudsfragmenten. CUGs wordt toegepast hiërarchisch en effect subfolders tenzij met andere CUG met voeten getreden.
   - Zorg ervoor dat gebruikers die tot andere kanalen behoren die de inhoud van de mappen gebruiken, zijn opgenomen in deze gebruikersgroep. U kunt ook de gebruikersgroepen die aan die kanalen zijn gekoppeld, opnemen in de lijst met CUG&#39;s. Als dat niet het geval is, is de inhoud niet toegankelijk voor deze kanalen.
3. Selecteer de map en kies **Eigenschappen** op de werkbalk.
4. Selecteer de **Machtigingen** tab.
5. Typ in het dialoogvenster **Groepsnaam** en selecteert u de **Toevoegen** om de nieuwe CUG toe te voegen.
6. **Opslaan** om de CUG toe te passen.
7. **Selecteren** de elementenmap en selecteer **Publiceren** om de map met de toegepaste CUG&#39;s naar AEM Publish te verzenden, waar deze als een machtiging wordt geëvalueerd.

Voer dezelfde stappen uit voor alle mappen met inhoudsfragmenten die moeten worden beveiligd, waarbij u de juiste CUG&#39;s toepast op elke map.

Nu, wanneer een HTTP- verzoek aan het AEM Punt van GraphQL API van de Zwaartepunt wordt gemaakt, slechts zullen de Fragments van de Inhoud die door gespecificeerde CUGs van de verzoekende gebruiker toegankelijk zijn in het resultaat worden omvat. Als de gebruiker geen toegang heeft tot een inhoudsfragment, is het resultaat leeg, maar wordt nog steeds een 200 HTTP-statuscode geretourneerd.

### Inhoud waarnaar wordt verwezen beschermen

Inhoudsfragmenten verwijzen vaak naar andere AEM, zoals afbeeldingen. Om deze inhoud waarnaar wordt verwezen te beveiligen, past u CUG&#39;s toe op de elementenmappen waarin de middelen waarnaar wordt verwezen, zijn opgeslagen. Merk op dat de referenced activa algemeen gevraagd worden gebruikend methodes verschillend van die van AEM Headless GraphQL APIs. Bijgevolg kan de manier waarop toegangstokens worden doorgegeven aan aanvragen naar deze genoemde elementen, verschillen.

Afhankelijk van de inhoudsarchitectuur kan het nodig zijn CUG&#39;s toe te passen op meerdere mappen om ervoor te zorgen dat alle inhoud waarnaar wordt verwezen, wordt beveiligd.

## Het in cache plaatsen van beveiligde inhoud voorkomen

AEM as a Cloud Service [plaatst standaard HTTP-reacties in cache](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/publish) voor prestatieverbetering. Dit kan echter problemen veroorzaken met het bedienen van beveiligde inhoud. Om te voorkomen dat dergelijke inhoud in cache wordt geplaatst, [cachekoppen voor specifieke eindpunten verwijderen](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/publish#how-to-customize-cache-rules-1) in de Apache-configuratie van de AEM Publish-instantie.

Voeg de volgende regel aan het Apache configuratiedossier van uw project van de Verzender toe om geheim voorgeheugenkopballen voor specifieke eindpunten te verwijderen:

```xml
# dispatcher/src/conf.d/available_vhosts/example.vhost

<VirtualHost *:80>
    ...
    # Replace `example` with the name of your GraphQL endpoint's configuration name.
    <LocationMatch "^/graphql/execute.json/example/.*$">
        # Remove cache headers for protected endpoints so they are not cached
        Header unset Cache-Control
        Header unset Surrogate-Control
        Header set Age 0
    </LocationMatch>
    ...
</VirtualHost>
```

Merk op dat dit een prestatiesboete zal veroorzaken aangezien de inhoud niet door de verzender of CDN in het voorgeheugen zal worden opgenomen. Dit is een compromis tussen prestaties en veiligheid.

## Beveiliging van eindpunten van de GraphQL API zonder koppen

In deze handleiding wordt het beveiligen van de [AEM GraphQL API-eindpunten zonder koppen](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/graphql-endpoint) zelf, maar vooral om de inhoud te beveiligen die hen ten goede komt. Alle gebruikers, inclusief anonieme gebruikers, hebben toegang tot de eindpunten die beveiligde inhoud bevatten. Alleen de inhoud die toegankelijk is voor de gesloten gebruikersgroepen van de gebruiker, wordt geretourneerd. Als er geen inhoud toegankelijk is, heeft de AEM Headless API-respons nog steeds een 200 HTTP-antwoordstatuscode, maar zijn de resultaten leeg. Doorgaans is het beveiligen van de inhoud voldoende, omdat de eindpunten zelf vertrouwelijke gegevens niet intrinsiek toegankelijk maken. Als u de eindpunten moet beveiligen, pas ACLs op hen op AEM toe publiceren via [Scripts voor initialisatie van opslagplaats (repoint-it)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).

