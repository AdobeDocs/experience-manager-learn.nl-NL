---
title: Beveiligde inhoud in AEM headless
description: Leer hoe u inhoud in AEM headless beveiligt.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer, Architect
level: Intermediate
jira: KT-15233
last-substantial-update: 2024-05-01T00:00:00Z
exl-id: c4b093d4-39b8-4f0b-b759-ecfbb6e9e54f
duration: 254
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1151'
ht-degree: 0%

---

# Inhoud in AEM zonder kop beschermen

De integriteit en beveiliging van uw gegevens garanderen wanneer u AEM inhoud zonder kop vanuit AEM Publish bedient, is van cruciaal belang wanneer u gevoelige inhoud moet bedienen. Zo kunt u de inhoud die wordt aangeboden door AEM GraphQL API-eindpunten zonder koppen beveiligen.

De leidraad in deze zelfstudie waar strenge eisen gelden voor inhoud die uitsluitend beschikbaar moet zijn voor specifieke gebruikers of gebruikersgroepen. Er moet een onderscheid worden gemaakt tussen gepersonaliseerde marketinginhoud en particuliere inhoud, zoals PII of persoonlijke financiële gegevens, om verwarring en onbedoelde resultaten te voorkomen. Deze zelfstudie behandelt het beschermen van persoonlijke inhoud.

Bij het bespreken van marketinginhoud hebben we het over inhoud die is toegesneden op individuele gebruikers of groepen, en die niet bestemd is voor algemene consumptie. Het is echter van essentieel belang om te begrijpen dat deze inhoud weliswaar op bepaalde gebruikers is gericht, maar dat de blootstelling buiten de beoogde context (bijvoorbeeld door het manipuleren van HTTP-aanvragen) geen risico voor de beveiliging, de wet of de reputatie oplevert.

Benadrukt wordt dat alle inhoud die in dit artikel wordt besproken, als privé wordt beschouwd en alleen door aangewezen gebruikers of groepen kan worden bekeken. Marketing-inhoud vereist vaak geen bescherming, maar de levering ervan aan specifieke gebruikers kan door de toepassing worden beheerd en in cache worden geplaatst voor prestaties.

Deze Hoe kan ik-onderwerpen bestrijken niet:

- Het beveiligen van de eindpunten direct, maar concentreert zich in plaats daarvan op het beveiligen van de inhoud die zij leveren.
- Verificatie voor AEM Publish of het verkrijgen van aanmeldingstokens. Verificatiemethoden en het doorgeven van referenties zijn afhankelijk van individuele gevallen en implementaties.

## Gebruikersgroepen

Eerst, moeten wij a [ gebruikersgroep ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/accessing/aem-users-groups-and-permissions) bepalen die de gebruikers bevat die toegang tot de beschermde inhoud zouden moeten hebben.

![ AEM Zwaarteloze beschermde de gebruikersgroep van de inhoudsgebruiker ](./assets/protected-content/user-groups.png){align="center"}

Gebruikersgroepen wijzen toegang toe aan inhoud zonder kop, waaronder Content Fragments of andere middelen waarnaar wordt verwezen.

1. Login aan AEM Auteur als a **gebruikersbeheerder**.
1. Navigeer aan **Hulpmiddelen** > **Veiligheid** > **Groepen**.
1. Selecteer **creeer** in de hoogste juiste hoek.
1. In het **lusje van Details**, specificeer **identiteitskaart van de Groep** en **Naam van de Groep**.
   - Identiteitskaart van de Groep en de Naam van de Groep kunnen om het even wat zijn, maar in dit voorbeeld gebruikt de naam **AEM de gebruikers van de Zwaartepunt API**.
1. Selecteer **sparen &amp; Sluiten**.
1. Selecteer de pas gecreëerde groep, dan kiezen **activeer** van de actiebar.

Als u verschillende toegangsniveaus nodig hebt, maakt u meerdere gebruikersgroepen die aan verschillende inhoud kunnen worden gekoppeld.

### Gebruikers toevoegen aan gebruikersgroepen

Als u AEM GraphQL API-verzoeken zonder koppen toegang wilt verlenen tot beveiligde inhoud, kunt u de aanvraag zonder koppen koppelen aan een gebruiker die tot een specifieke gebruikersgroep behoort. Hier volgen twee veelvoorkomende benaderingen:

1. **AEM as a Cloud Service [ technische rekeningen ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials):**
   - Maak een technische rekening in de AEM as a Cloud Service Developer Console.
   - Meld u eenmaal aan bij AEM auteur met de technische account.
   - Voeg de technische rekening aan de gebruikersgroep via **Hulpmiddelen > Veiligheid > Groepen > AEM Koploze API gebruikers > Leden** toe.
   - **activeer** zowel de technische rekeningsgebruiker als de gebruikersgroep op AEM Publish.
   - Deze methode is vereist dat de headless cliënt de Credentials van de Dienst niet aan de gebruiker blootstelt, aangezien zij geloofsbrieven voor een specifieke gebruiker zijn, en niet zouden moeten worden gedeeld.

   ![AEM Technical Account group management ](./assets/protected-content/group-membership.png){align="center"}

2. **Benoemde gebruikers:**
   - Verifieer genoemde gebruikers en voeg hen aan de gebruikersgroep op AEM Publish direct toe.
   - Deze methode vereist de headless cliënt om gebruikersgeloofsbrieven met AEM Publish voor authentiek te verklaren, een AEM login of toegangstoken te verkrijgen, en dit teken voor verdere verzoeken aan AEM te gebruiken. De details over hoe dit te bereiken zijn niet in deze manier van doen en zijn afhankelijk van de uitvoering.

## Inhoudsfragmenten beschermen

Het beschermen van inhoudsfragmenten is van essentieel belang voor de beveiliging van de inhoud zonder kop en wordt bereikt door de inhoud te koppelen aan een gesloten gebruikersgroep (CUG). Wanneer een gebruiker een aanvraag indient voor de AEM GraphQL API zonder koppen, wordt de geretourneerde inhoud gefilterd op basis van de CUG&#39;s van de gebruiker.

![ AEM Zwaarloze KUGs ](./assets/protected-content/cugs.png){align="center"}

Volg deze stappen om dit door [ gesloten Gebruikersgroepen (CUGs) ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/assets/advanced/closed-user-groups) te bereiken.

1. Login aan AEM Auteur als a **gebruiker DAM**.
2. Navigeer aan **Assets > Dossiers** en selecteer de **omslag** die de te beschermen Fragmenten van de Inhoud bevat. CUGs wordt toegepast hiërarchisch en effect subfolders tenzij met andere CUG met voeten getreden.
   - Zorg ervoor dat gebruikers die tot andere kanalen behoren die de inhoud van de mappen gebruiken, zijn opgenomen in deze gebruikersgroep. U kunt ook de gebruikersgroepen die aan die kanalen zijn gekoppeld, opnemen in de lijst met CUG&#39;s. Als dat niet het geval is, is de inhoud niet toegankelijk voor deze kanalen.
3. Selecteer de omslag en kies **Eigenschappen** van de toolbar.
4. Selecteer de **Toestemmingen** tabel.
5. Het type in de **Naam van de Groep** en selecteert **voegt** knoop toe om nieuwe CUG toe te voegen.
6. **sparen** om CUG toe te passen.
7. **selecteer** de activaomslag en selecteer **Publish** om de omslag met toegepaste CUGs naar AEM Publish te verzenden, waar het als toestemming zal worden geëvalueerd.

Voer dezelfde stappen uit voor alle mappen met inhoudsfragmenten die moeten worden beveiligd, waarbij u de juiste CUG&#39;s toepast op elke map.

Nu, wanneer een HTTP- verzoek aan het AEM Punt van GraphQL API van de Zwaartepunt wordt gemaakt, slechts zullen de Fragments van de Inhoud die door gespecificeerde CUGs van de verzoekende gebruiker toegankelijk zijn in het resultaat worden omvat. Als de gebruiker geen toegang heeft tot een inhoudsfragment, is het resultaat leeg, maar wordt nog steeds een 200 HTTP-statuscode geretourneerd.

### Inhoud waarnaar wordt verwezen beschermen

Inhoudsfragmenten verwijzen vaak naar andere AEM, zoals afbeeldingen. Om deze inhoud waarnaar wordt verwezen te beveiligen, past u CUG&#39;s toe op de elementenmappen waarin de middelen waarnaar wordt verwezen, zijn opgeslagen. Merk op dat de referenced activa algemeen gevraagd worden gebruikend methodes verschillend van die van AEM Headless GraphQL APIs. Bijgevolg kan de manier waarop toegangstokens worden doorgegeven aan aanvragen naar deze genoemde elementen, verschillen.

Afhankelijk van de inhoudsarchitectuur kan het nodig zijn CUG&#39;s toe te passen op meerdere mappen om ervoor te zorgen dat alle inhoud waarnaar wordt verwezen, wordt beveiligd.

## Het in cache plaatsen van beveiligde inhoud voorkomen

AEM as a Cloud Service [ plaatst de reacties van HTTP door gebrek ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/publish) voor prestatiesverbetering in het voorgeheugen. Dit kan echter problemen veroorzaken met het bedienen van beveiligde inhoud. Om caching van dergelijke inhoud te verhinderen, [ verwijder geheim voorgeheugenkopballen voor specifieke eindpunten ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/publish#how-to-customize-cache-rules-1) in de configuratie Apache van de AEM instantie van Publish.

Voeg de volgende regel toe aan het Apache-configuratiebestand van uw Dispatcher-project om cachekoppen voor specifieke eindpunten te verwijderen:

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

Deze gids richt zich niet het beveiligen van de [ AEM Koploze eindpunten van GraphQL API ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/graphql-endpoint) zelf, maar eerder op het beveiligen van de inhoud die door hen wordt gediend. Alle gebruikers, inclusief anonieme gebruikers, hebben toegang tot de eindpunten die beveiligde inhoud bevatten. Alleen de inhoud die toegankelijk is voor de gesloten gebruikersgroepen van de gebruiker, wordt geretourneerd. Als er geen inhoud toegankelijk is, heeft de AEM Headless API-respons nog steeds een 200 HTTP-antwoordstatuscode, maar zijn de resultaten leeg. Doorgaans is het beveiligen van de inhoud voldoende, omdat de eindpunten zelf vertrouwelijke gegevens niet intrinsiek toegankelijk maken. Als u de eindpunten moet beveiligen, pas ACLs op hen op AEM Publish toe via [ het Schipen van de Initialisatie van de Bewaarplaats van de Bewaarplaats (opnieuw richt) manuscripten ](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
