---
title: Inleiding tot ontwerpen en publiceren | Snel site maken AEM
description: Gebruik de pagina-editor in Adobe Experience Manager AEM om de inhoud van de website bij te werken. Leer hoe Componenten worden gebruikt om ontwerpen te vergemakkelijken. Begrijp het verschil tussen AEM auteur- en Publish-omgevingen en leer hoe u wijzigingen op de livesite publiceert.
version: Cloud Service
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
jira: KT-7497
thumbnail: KT-7497.jpg
doc-type: Tutorial
exl-id: 17ca57d1-2b9a-409c-b083-398d38cd6a19
recommendations: noDisplay, noCatalog
duration: 263
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1285'
ht-degree: 0%

---

# Inleiding tot ontwerpen en publiceren {#author-content-publish}

Het is belangrijk om te begrijpen hoe een gebruiker inhoud voor de website zal bijwerken. In dit hoofdstuk zullen wij de persoon van de Auteur van de Inhoud van a **goedkeuren** en sommige redactionele updates aan de plaats maken die in het vorige hoofdstuk wordt geproduceerd. Aan het einde van het hoofdstuk publiceren we de wijzigingen om te begrijpen hoe de livesite wordt bijgewerkt.

## Vereisten {#prerequisites}

Dit is een meerdelig leerprogramma en men veronderstelt dat de stappen die in [ worden geschetst een plaats ](./create-site.md) hoofdstuk zijn voltooid.

## Doelstelling {#objective}

1. Begrijp de concepten **Pagina&#39;s** en **Componenten** in AEM Sites.
1. Leer hoe u de inhoud van de website kunt bijwerken.
1. Leer hoe u wijzigingen op de livesite publiceert.

## Een nieuwe pagina maken {#create-page}

Een website wordt doorgaans opgedeeld in pagina&#39;s en vormt zo een ervaring van meerdere pagina&#39;s. AEM de inhoud van structuren op dezelfde manier. Maak vervolgens een nieuwe pagina voor de site.

1. Login aan de AEM **Dienst van de Auteur** die in het vorige hoofdstuk wordt gebruikt.
1. Van het AEM scherm van het Begin klik **Plaatsen** > **Plaats WKND** > **Engels** > **Artikel**
1. In de hogere rechterhoek klikt **** > **Pagina** creëren.

   ![ creeer Pagina ](assets/author-content-publish/create-page-button.png)

   Dit zal omhoog de **creëren tovenaar van de Pagina** brengen.

1. Kies het **malplaatje van de Pagina van het Artikel** en klik **daarna**.

   Pagina&#39;s in AEM worden gemaakt op basis van een paginasjabloon. De Malplaatjes van de pagina worden onderzocht in detail in het [ hoofdstuk van de Malplaatjes van de Pagina ](page-templates.md).

1. Onder **Eigenschappen** ga a **Titel** van &quot;de Wereld van Hello in.
1. Plaats de **Naam** om `hello-world` te zijn en **te klikken creeert**.

   ![ Aanvankelijke eigenschappen van de Pagina ](assets/author-content-publish/initial-page-properties.png)

1. In de dialoog pop-up klik **Open** om de pas gecreëerde pagina te openen.

## Auteur een component {#author-component}

AEM Componenten kunnen worden beschouwd als kleine modulaire bouwstenen van een webpagina. Door UI in logische brokken of Componenten te breken, maakt het het veel gemakkelijker te beheren. Om componenten opnieuw te gebruiken, moeten de componenten configureerbaar zijn. Dit gebeurt via het dialoogvenster van de auteur.

AEM verstrekt een reeks [ Componenten van de Kern ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) die productie klaar om zijn te gebruiken. De **Componenten van de Kern** waaier van basiselementen zoals [ Tekst ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) en [ Beeld ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) aan complexere elementen UI zoals a [ Carrousel ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html).

Vervolgens maakt u enkele componenten met de AEM Pagina-editor.

1. Navigeer aan de **Wereld van Hello** pagina die in de vorige oefening wordt gecreeerd.
1. Zorg ervoor dat u op **uitgeeft** wijze bent en op de linkerkant-spoor klikt het **pictogram van Componenten**.

   ![ de redacteur van de pagina zijspoor ](assets/author-content-publish/page-editor-siderail.png)

   Hiermee wordt de componentbibliotheek geopend en worden de beschikbare componenten weergegeven die op de pagina kunnen worden gebruikt.

1. De rol neer en **Drag+Drop** a **Tekst (v2)** component op het belangrijkste editable gebied van de pagina.

   ![ belemmering + de tekstcomponent van de Daling ](assets/author-content-publish/drag-drop-text-cmp.png)

1. Klik de **component van de Tekst** om te benadrukken en dan het **moersleutel** pictogram ![ pictogram van de Sleutel ](assets/author-content-publish/wrench-icon.png) te klikken om de dialoog van de Component te openen. Voer tekst in en sla de wijzigingen op in het dialoogvenster.

   ![ Rich Text Component ](assets/author-content-publish/rich-text-populated-component.png)

   De **component van de Tekst** zou de rijke tekst op de pagina nu moeten tonen.

1. Herhaal de bovengenoemde stappen, behalve sleep een geval van het **Beeld (v2)** component op de pagina. Open de {**dialoog van de component 1} van het Beeld.**

1. In het linkerspoor, schakelaar aan de **Vinder van Activa** door het **pictogram van Assets** pictogram ![ activa ](assets/author-content-publish/asset-icon.png) te klikken.
1. **Drag+Drop** een beeld in de dialoog van de Component en klik **Gedaan** om de veranderingen te bewaren.

   ![ voeg activa aan dialoog toe ](assets/author-content-publish/add-asset-dialog.png)

1. Merk op dat er componenten op de pagina zijn, als de **Titel**, **Navigatie**, **Onderzoek** die vast zijn. Deze gebieden zijn geconfigureerd als onderdeel van het paginasjabloon en kunnen niet worden gewijzigd op een afzonderlijke pagina. Dit wordt meer onderzocht in het volgende hoofdstuk.

Voel u vrij om te experimenteren met enkele andere componenten. De documentatie over elke [ Component van de Kern kan hier ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) worden gevonden. Een gedetailleerde videoreeks over [ het auteursrecht van de Pagina kan hier ](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html) worden gevonden.

## Publish-updates {#publish-updates}

AEM de milieu&#39;s worden verdeeld tussen de Dienst van de Auteur **en de Dienst van a** Publish **.** In dit hoofdstuk hebben wij verscheidene wijzigingen aan de plaats op de **Dienst van de Auteur** aangebracht. Opdat plaatsbezoekers om de veranderingen te bekijken moeten wij hen aan de **Dienst van Publish** publiceren.

![ Hoog niveaudiagram ](assets/author-content-publish/author-publish-high-level-flow.png)

*Hoogtepunt stroom van inhoud van Auteur aan Publish*

**1.** Inhoudsauteurs werken de site-inhoud bij. De updates kunnen worden voorvertoond, gecontroleerd en goedgekeurd om live te worden gezet.

**2.** Inhoud wordt gepubliceerd. Publicatie kan op aanvraag worden uitgevoerd of voor een toekomstige datum worden gepland.

**3.** De wijzigingen worden door bezoekers van de site weerspiegeld in de Publish-service.

### Publish de wijzigingen

Laten we nu de wijzigingen publiceren.

1. Van het AEM scherm van het Begin navigeert aan **Plaatsen** en selecteert de **Plaats WKND**.
1. Klik **leiden Publicatie** in de menubar.

   ![ beheer publicatie ](assets/author-content-publish/click-manage-publiciation.png)

   Aangezien dit een gloednieuwe site is, willen we alle pagina&#39;s publiceren en kunnen we met de wizard Publicatie beheren precies bepalen wat er moet worden gepubliceerd.

1. Onder **Opties** verlaat de standaardmontages aan **Publish** en plant het voor **nu**. Klik op **Next**.
1. Onder **Reikwijdte**, selecteer de **Plaats WKND** en klik **omvat de Montages van Kinderen**. In de dialoog, controle **omvat kinderen**. Schakel de overige selectievakjes uit om ervoor te zorgen dat de hele site wordt gepubliceerd.

   ![ Update publiceert werkingsgebied ](assets/author-content-publish/update-scope-publish.png)

1. Klik de **Gepubliceerde knoop van Verwijzingen**. Controleer in het dialoogvenster of alles is gecontroleerd. Dit zal het **StandaardMalplaatje van de Plaats** en verscheidene configuraties omvatten die door het Malplaatje van de Plaats worden geproduceerd. Klik **Gedaan** om bij te werken.

   ![ verwijzingen van Publish ](assets/author-content-publish/publish-references.png)

1. Tot slot controleer de doos naast **Plaats WKND** en klik **daarna** in de hogere rechterhoek.
1. In de **stap van de Werkstromen**, ga de titel van het a **Werkschema** in. Dit kan om het even welke tekst zijn en als deel van een controletraject later nuttig. Ga &quot;Aanvankelijke publiceren&quot;in en klik **Publish**.

![ De stap eerste van het Werkschema publiceert ](assets/author-content-publish/workflow-step-publish.png)

## Gepubliceerde inhoud weergeven {#publish}

Navigeer vervolgens naar de Publish-service om de wijzigingen weer te geven.

1. U kunt de URL van de Publish Service eenvoudig ophalen door de Auteur-URL te kopiëren en het woord `author` te vervangen door `publish` . Bijvoorbeeld:

   * **Auteur URL** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **Publish URL** - `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. Voeg `/content/wknd.html` toe aan de Publish-URL, zodat de uiteindelijke URL er als volgt uitziet: `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html` .

   >[!NOTE]
   >
   > Verandering `wknd.html` om de naam van uw plaats aan te passen, als u een unieke naam tijdens [ plaatsverwezenlijking ](create-site.md) verstrekte.

1. Als u naar de Publish-URL navigeert, wordt de site weergegeven, zonder enige AEM ontwerpfunctionaliteit.

   ![ Gepubliceerde plaats ](assets/author-content-publish/publish-url-update.png)

1. Gebruikend het **menu van de Navigatie** klikt **Artikel** > **Wereld van Hello** om aan de vroeger gemaakte pagina van de Wereld van Hello te navigeren.
1. Keer terug naar de **AEM Dienst van de Auteur** en maak sommige extra inhoudsveranderingen in de Redacteur van de Pagina.
1. Publish deze veranderingen direct van binnen de paginaredacteur door het **pictogram van de Eigenschappen van de Pagina** te klikken > **Publish Pagina**

   ![ publiceer direct ](assets/author-content-publish/page-editor-publish.png)

1. Terugkeer aan de **AEM Dienst van Publish** om de veranderingen te bekijken. Waarschijnlijk zult u **** niet onmiddellijk de updates zien. Dit is omdat de **AEM Dienst van Publish** [ caching via een Apache Webserver en CDN ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html) omvat. Standaard wordt HTML-inhoud gedurende ~5 minuten in cache geplaatst.

1. Als u de cache wilt overslaan voor test- en foutopsporingsdoeleinden, voegt u gewoon een queryparameter toe, zoals `?nocache=true` . De URL zou er als `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true` uitzien. Meer details over de caching strategie en beschikbare configuraties [ kunnen hier ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html) worden gevonden.

1. U kunt ook de URL naar de Publish Service in Cloud Manager vinden. Navigeer aan het **Programma van Cloud Manager** > **Milieu** > **Milieu**.

   ![ de Dienst van Publish van de Mening ](assets/author-content-publish/view-environment-segments.png)

   Onder **Segmenten van het Milieu** kunt u verbindingen aan de **Auteur** en **Publish** diensten vinden.

## Gefeliciteerd! {#congratulations}

U hebt zojuist wijzigingen in uw AEM-site gemaakt en gepubliceerd.

### Volgende stappen {#next-steps}

In een real-world implementatie die een plaats met modellen en ontwerpen UI plant gaat typisch aan verwezenlijking van de Plaats vooraf. Leer hoe de Kits van UI van Adobe XD kunnen worden gebruikt om uw implementatie van Adobe Experience Manager Sites in [ te ontwerpen UI die met Adobe XD ](./ui-planning-adobe-xd.md) plant te versnellen.

Wilt u doorgaan met het verkennen van AEM Sites-mogelijkheden? Voel vrij om recht binnen aan het hoofdstuk op [ Malplaatjes van de Pagina ](./page-templates.md) te springen om het verband tussen een Malplaatje van de Pagina en een Pagina te begrijpen.


