---
title: Tags integreren in Adobe Experience Platform
description: Leer hoe u AEM as a Cloud Service kunt integreren met tags in Adobe Experience Platform. Dankzij de integratie kunt u de Adobe Web SDK implementeren en aangepaste JavaScript voor gegevensverzameling en personalisatie in uw AEM-pagina's injecteren.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture, Content Management
role: Developer, Architect, Leader, Data Architect, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18719
thumbnail: null
source-git-commit: 70665c019f63df1e736292ad24c47624a3a80d49
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 0%

---


# Tags integreren in Adobe Experience Platform

Leer hoe u AEM as a Cloud Service (AEMCS) kunt integreren met tags in Adobe Experience Platform. Met de integratie Tags (ook wel Launch genoemd) kunt u de Adobe Web SDK implementeren en aangepaste JavaScript voor gegevensverzameling en personalisatie in uw AEM-pagina&#39;s injecteren.

Dankzij de integratie kan uw marketing- of ontwikkelingsteam JavaScript voor personalisatie en gegevensverzameling beheren en implementeren, zonder dat u de AEM-code opnieuw hoeft te implementeren.

## Stappen op hoog niveau

Het integratieproces omvat vier belangrijke stappen die de verbinding tussen AEM en Tags tot stand brengen:

1. **creeer, vorm, en publiceer een bezit van Markeringen in Adobe Experience Platform**
2. **verifieer een configuratie van Adobe IMS voor Markeringen in AEM**
3. **creeer een configuratie van Markeringen in AEM**
4. **pas de configuratie van Markeringen op uw pagina&#39;s van AEM toe**

## Een eigenschap voor tags maken, configureren en publiceren in Adobe Experience Platform

Begin door een bezit van Markeringen in Adobe Experience Platform te creëren. Met deze eigenschap kunt u de implementatie van de Adobe Web SDK en eventuele aangepaste JavaScript beheren die vereist zijn voor personalisatie en gegevensverzameling.

1. Ga naar [&#x200B; Adobe Experience Platform &#x200B;](https://experience.adobe.com/platform), teken binnen met uw Adobe ID, en navigeer aan **Markeringen** van het linkermenu.\
   ![&#x200B; de Markeringen van Adobe Experience Platform &#x200B;](../assets/setup/aep-tags.png)

2. Klik **Nieuw Bezit** om een nieuw bezit van Markeringen tot stand te brengen.\
   ![&#x200B; creeer Nieuw bezit van Markeringen &#x200B;](../assets/setup/aep-create-tags-property.png)

3. In **creeer Bezit** dialoog, ga het volgende in:
   - **Naam van het Bezit**: Een naam voor uw bezit van Markeringen
   - **Type van Bezit**: Selecteer **Web**
   - **Domein**: Het domein waar u het bezit opstelt (bijvoorbeeld, `.adobeaemcloud.com`)

   Klik **sparen**.

   ![&#x200B; het Bezit van de Markeringen van Adobe &#x200B;](../assets/setup/adobe-tags-property.png)

4. Open de nieuwe eigenschap. De **uitbreiding van de Kern** zou reeds moeten worden omvat. Later, gaat u de **uitbreiding van SDK van het 0&rbrace; Web toevoegen &lbrace;wanneer vestiging de het gebruiksgeval van de Experimentatie, aangezien het extra configuratie zoals** identiteitskaart van DataStream **vereist.**\
   ![&#x200B; de Uitbreiding van de Kern van de Markeringen van Adobe &#x200B;](../assets/setup/adobe-tags-core-extension.png)

5. Publiceer het bezit van Markeringen door naar **het Publiceren Stroom** te gaan en **te klikken voeg Bibliotheek** toe om een plaatsingsbibliotheek tot stand te brengen.
   ![&#x200B; Adobe Codes die Stroom publiceren &#x200B;](../assets/setup/adobe-tags-publishing-flow.png)

6. In **creeer Bibliotheek** dialoog, verstrek:
   - **Naam**: Een naam voor uw bibliotheek
   - **Milieu**: Selecteer **Ontwikkeling**
   - **Veranderingen van het Middel**: Kies **Alle Gewijzigde Middelen** toevoegen

   Klik **sparen &amp; bouwen aan Ontwikkeling**.

   ![&#x200B; de Markeringen van Adobe creëren Bibliotheek &#x200B;](../assets/setup/adobe-tags-create-library.png)

7. Om de bibliotheek aan productie te publiceren, klik **goedkeuren en publiceren aan Productie**. Zodra het publiceren is voltooid, is het bezit klaar voor gebruik in AEM.\
   ![&#x200B; de Markeringen van Adobe goedkeuren &amp; publiceren &#x200B;](../assets/setup/adobe-tags-approve-publish.png)

## Een Adobe IMS-configuratie voor tags in AEM controleren

Wanneer een AEMCS-omgeving wordt ingericht, bevat deze automatisch een Adobe IMS-configuratie voor tags, samen met een bijbehorend Adobe Developer Console-project. Deze configuratie zorgt voor veilige API-communicatie tussen AEM en Tags.

1. In AEM, navigeer aan **Hulpmiddelen** > **Veiligheid** > **de Configuraties van Adobe IMS**.\
   ![&#x200B; de Configuraties van Adobe IMS &#x200B;](../assets/setup/aem-ims-configurations.png)

2. Bepaal de plaats van de **1&rbrace; configuratie van de Lancering van Adobe.** Indien beschikbaar, selecteer het en klik **Gezondheid van de Controle** om de verbinding te verifiëren. Je zou een succesreactie moeten zien.\
   ![&#x200B; Controle van de Gezondheid van de Configuratie van Adobe IMS &#x200B;](../assets/setup/aem-ims-configuration-health-check.png)

## Een tagconfiguratie maken in AEM

Maak een configuratie Tags in AEM om de benodigde eigenschappen en instellingen voor uw sitepagina&#39;s op te geven.

1. In AEM, ga naar **Hulpmiddelen** > **de Diensten van de Wolk** > **de Configuraties van de Lancering van Adobe**.\
   ![&#x200B; de Configuraties van de Lancering van Adobe &#x200B;](../assets/setup/aem-launch-configurations.png)

2. Selecteer de wortelomslag van uw plaats (bijvoorbeeld, Plaats WKND) en klik **creeer**.\
   ![&#x200B; creeer de Configuratie van de Lancering van Adobe &#x200B;](../assets/setup/aem-create-launch-configuration.png)

3. Voer in het dialoogvenster het volgende in:
   - **Titel**: Bijvoorbeeld, &quot;de Markeringen van Adobe&quot;
   - **IMS Configuratie**: Selecteer de geverifieerde **configuratie IMS van de Lancering van Adobe**
   - **Bedrijf**: Selecteer het bedrijf verbonden aan uw bezit van Markeringen
   - **Bezit**: Kies het bezit van Markeringen vroeger gecreeerd

   Klik op **Next**.

   ![&#x200B; de Details van de Configuratie van de Lancering van Adobe &#x200B;](../assets/setup/aem-launch-configuration-details.png)

4. Voor demonstratiedoeleinden, houd de standaardwaarden voor **het Opvoeren** en **de milieu&#39;s van de Productie**. Klik **creëren**.\
   ![&#x200B; de Details van de Configuratie van de Lancering van Adobe &#x200B;](../assets/setup/aem-launch-configuration-create.png)

5. Selecteer de onlangs gecreeerde configuratie en klik **publiceren** om het ter beschikking te stellen aan uw plaatspagina&#39;s.\
   ![&#x200B; de Configuratie van de Lancering van Adobe publiceert &#x200B;](../assets/setup/aem-launch-configuration-publish.png)

## De configuratie van tags toepassen op uw AEM-site

Pas de configuratie van Markeringen toe om het Web SDK en de verpersoonlijkingslogica in uw plaatspagina&#39;s te injecteren.

1. In AEM, ga naar **Plaatsen**, selecteer uw omslag van de wortelplaats (bijvoorbeeld, Plaats WKND), en klik **Eigenschappen**.\
   ![&#x200B; de Eigenschappen van de Plaats van AEM &#x200B;](../assets/setup/aem-site-properties.png)

2. In de **dialoog van de Eigenschappen van de Plaats**, open het **Geavanceerde** lusje. Onder **Configuraties**, zorg ervoor `/conf/wknd` voor **de Configuratie van de Wolk** wordt geselecteerd.\
   ![&#x200B; Geavanceerde Eigenschappen van de Plaats AEM &#x200B;](../assets/setup/aem-site-advanced-properties.png)

## De integratie controleren

Om te bevestigen dat de configuratie van Markeringen correct werkt, kunt u:

1. Controleer de weergavebron van een AEM-publicatiepagina of inspecteer deze met de browsergereedschappen
2. Gebruik [&#x200B; Adobe Experience Platform Debugger &#x200B;](https://chromewebstore.google.com/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) om de injectie van SDK en van JavaScript van het Web te bevestigen

![&#x200B; Adobe Experience Platform Debugger &#x200B;](../assets/setup/aep-debugger.png)

## Aanvullende bronnen

- [&#x200B; overzicht van Adobe Experience Platform Debugger &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-platform/debugger/home)
- [&#x200B; Overzicht van Markeringen &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-platform/tags/home)
