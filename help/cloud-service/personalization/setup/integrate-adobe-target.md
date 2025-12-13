---
title: Adobe Target integreren
description: Leer hoe u AEM as a Cloud Service met Adobe Target kunt integreren om persoonlijke inhoud (Experience Fragments) als aanbiedingen te beheren en te activeren.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture
role: Developer, Leader, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18718
thumbnail: null
exl-id: 86767e52-47ce-442c-a620-bc9e7ac2eaf3
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 0%

---

# Adobe Target integreren

Leer hoe u AEM as a Cloud Service (AEMCS) kunt integreren met Adobe Target om persoonlijke inhoud, zoals Experience Fragments, te activeren zoals aanbiedingen in Adobe Target.

Dankzij deze integratie kan uw marketingteam persoonlijke inhoud centraal in AEM maken en beheren. Deze inhoud kan vervolgens naadloos worden geactiveerd als aanbiedingen in Adobe Target.

>[!IMPORTANT]
>
>De integratiestap is optioneel als uw team ervoor kiest om aanbiedingen volledig binnen Adobe Target te beheren, zonder AEM als gecentraliseerde opslagplaats voor inhoud te gebruiken.

## Stappen op hoog niveau

Het integratieproces omvat vier belangrijke stappen die de verbinding tussen AEM en Adobe Target tot stand brengen:

1. **creeer en vorm een project van Adobe Developer Console**
2. **creeer een configuratie van Adobe IMS voor Doel in AEM**
3. **creeer een configuratie van erfenisAdobe Target in AEM**
4. **pas de configuratie van Adobe Target op de Fragmenten van de Ervaring toe**

## Een Adobe Developer Console-project maken en configureren

Om AEM in staat te stellen veilig met Adobe Target te communiceren, moet u een Adobe Developer Console-project configureren met behulp van OAuth server-to-server verificatie. U kunt een bestaand project gebruiken of een nieuw project tot stand brengen.

1. Ga naar [ Adobe Developer Console ](https://developer.adobe.com/console) en teken binnen met uw Adobe ID.

2. Maak een nieuw project of selecteer een bestaand project.\
   ![ Project van Adobe Developer Console ](../assets/setup/adc-project.png)

3. Klik **toevoegen API**. In **voeg API** dialoog toe, filter door **Experience Cloud**, selecteer **Adobe Target**, en klik **daarna**.\
   ![ voeg API aan Project toe ](../assets/setup/adc-add-api.png)

4. In **vorm API** dialoog, selecteer de **Server-aan-Server** authentificatiemethode en klik **daarna**.\
   ![ vorm API ](../assets/setup/adc-configure-api.png)

5. In de **Uitgezochte Profiles van het Product** stap, selecteer **Standaard Workspace** en klik **sparen Gevormde API**.\
   ![ Uitgezochte Profielen van het Product ](../assets/setup/adc-select-product-profiles.png)

6. In de linkernavigatie, uitgezochte **OAuth Server-aan-Server** en herzie de configuratiedetails. Let op de client-id en het clientgeheim. U hebt deze waarden nodig om de IMS-integratie in AEM te configureren.
   ![ Server-aan-Server Details ](../assets/setup/adc-oauth-server-to-server.png)

## Een Adobe IMS-configuratie voor doel maken in AEM

In AEM maakt u een Adobe IMS-configuratie voor Target met behulp van de referenties van de Adobe Developer Console. Met deze configuratie kan AEM verifiëren met de Adobe Target API&#39;s.

1. In AEM, navigeer aan **Hulpmiddelen** > **Veiligheid** en selecteer **Configuraties van Adobe IMS**.\
   ![ de Configuraties van Adobe IMS ](../assets/setup/aem-ims-configurations.png)

2. Klik **creëren**.\
   ![ creeer de Configuratie van Adobe IMS ](../assets/setup/aem-create-ims-configuration.png)

3. Op de **pagina van de Configuratie van de Rekening van Adobe IMS Technische**, ga het volgende in:
   - **Oplossing van de Wolk**: Adobe Target
   - **Titel**: Een etiket voor de configuratie, zoals &quot;Adobe Target&quot;
   - **de Server van de Vergunning**: `https://ims-na1.adobelogin.com`
   - **identiteitskaart van de Cliënt**: Van Adobe Developer Console
   - **Geheim van de Cliënt**: Van Adobe Developer Console
   - **Reikwijdte**: Van Adobe Developer Console
   - **Org identiteitskaart**: Van Adobe Developer Console

   Dan klik **creëren**.

   ![ de Details van de Configuratie van Adobe IMS ](../assets/setup/aem-ims-configuration-details.png)

4. Selecteer de configuratie en klik **Gezondheid van de Controle** om de verbinding te verifiëren. Een succesbericht bevestigt dat AEM verbinding kan maken met Adobe Target.\
   ![ Controle van de Gezondheid van de Configuratie van Adobe IMS ](../assets/setup/aem-ims-configuration-health-check.png)

## Een verouderde Adobe Target-configuratie maken in AEM

Als u Experience Fragments wilt exporteren als aanbiedingen aan Adobe Target, maakt u een oudere Adobe Target-configuratie in AEM.

1. In AEM, navigeer aan **Hulpmiddelen** > **de Diensten van de Wolk** en selecteer **de Diensten van de Wolk van de Oudere wolk**.\
   ![ de Verouderde Diensten van de Wolk ](../assets/setup/aem-legacy-cloud-services.png)

2. In de **Adobe Target** sectie, klik **nu** vormen.\
   ![ vorm Adobe Target Verouderd ](../assets/setup/aem-configure-adobe-target-legacy.png)

3. In **creeer de dialoog van de Configuratie**, ga een naam zoals &quot;Verouderde Adobe Target&quot;in en klik **creeer**.\
   ![ creeer Adobe Target Verouderde Configuratie ](../assets/setup/aem-create-adobe-target-legacy-configuration.png)

4. Voor de **pagina van de Configuratie van de Verouderde Adobe Target**, verstrek het volgende:
   - **Authentificatie**: IMS
   - **Code van de Cliënt**: Uw de cliëntcode van Adobe Target (die in Adobe Target wordt gevonden onder **Beleid** > **Implementatie**)
   - **Configuratie IMS**: De configuratie IMS u vroeger creeerde

   Klik **verbinden met Adobe Target** om de verbinding te bevestigen.

   ![ Adobe Target Verouderde Configuratie ](../assets/setup/aem-target-legacy-configuration.png)

## Adobe Target-configuratie toepassen op fragmenten met ervaring

Koppel de Adobe Target-configuratie aan uw Experience Fragments zodat ze kunnen worden geëxporteerd en gebruikt als aanbiedingen in Target.

1. In AEM, ga naar **Fragmenten van de Ervaring**.\
   ![Ervaringsfragmenten](../assets/setup/aem-experience-fragments.png)

2. Selecteer de wortelomslag die uw Fragmenten van de Ervaring (bijvoorbeeld, `WKND Site Fragments`) bevat en **Eigenschappen** klikt.\
   ![ Eigenschappen van de Fragmenten van de Ervaring ](../assets/setup/aem-experience-fragments-properties.png)

3. Voor de **pagina van Eigenschappen**, open de **Diensten van de Wolk** tabel. In de **sectie van de Configuraties van Cloud Service**, selecteer uw configuratie van Adobe Target.\
   ![ de Diensten van de Wolk van Fragmenten van de Ervaring](../assets/setup/aem-experience-fragments-cloud-services.png)

4. In de **Adobe Target** sectie die verschijnt, voltooi het volgende:
   - **het Formaat van de Uitvoer van Adobe Target**: HTML
   - **Adobe Target Workspace**: Selecteer de werkruimte aan gebruik (bijvoorbeeld, &quot;Standaard Workspace&quot;)
   - **ExternalAlizer Domains**: Ga de domeinen voor het produceren van externe URLs in

   ![ Fragmenten van de Ervaring de Configuratie van Adobe Target ](../assets/setup/aem-experience-fragments-adobe-target-configuration.png)

5. Klik **sparen &amp; dicht** om de configuratie toe te passen.

## De integratie controleren

Om te bevestigen dat de integratie correct werkt, test de uitvoerfunctionaliteit:

1. Maak in AEM een nieuw Ervingspatroon of open een bestaand fragment. Klik **Uitvoer aan Adobe Target** van de toolbar.\
   ![ de Fragment van de Ervaring van de Uitvoer aan Adobe Target ](../assets/setup/aem-export-experience-fragment-to-adobe-target.png)

2. In Adobe Target, ga naar de **sectie van Aanbiedingen** en verifieer dat het Fragment van de Ervaring als aanbieding verschijnt.\
   ![ Aanbiedingen van Adobe Target ](../assets/setup/adobe-target-xf-as-offer.png)

## Aanvullende bronnen

- [ overzicht van doel API ](https://experienceleague.adobe.com/en/docs/target-dev/developer/api/target-api-overview)
- [ Aanbieding van het Doel ](https://experienceleague.adobe.com/en/docs/target/using/experiences/offers/manage-content)
- [ Adobe Developer Console ](https://developer.adobe.com/developer-console/docs/guides/)
- [ Fragmenten van de Ervaring in AEM ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use)
