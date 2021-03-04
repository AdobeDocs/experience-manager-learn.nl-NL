---
title: Projecten ontwikkelen in AEM
description: Een zelfstudie over de ontwikkeling van AEM projecten.  In deze zelfstudie maken we een aangepaste projectsjabloon die kan worden gebruikt om nieuwe projecten te maken binnen AEM voor het beheer van workflows en taken voor het schrijven van inhoud.
version: 6.3, 6.4, 6.5
feature: '"Projecten, workflow"'
topics: collaboration, development, governance
activity: develop
audience: developer, implementer, administrator
doc-type: tutorial
topic: Ontwikkeling
role: Developer
level: Begin
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '4654'
ht-degree: 0%

---


# Projecten ontwikkelen in AEM

Dit is een zelfstudie over ontwikkeling die laat zien hoe u [!DNL AEM Projects] kunt ontwikkelen.  In deze zelfstudie maken we een aangepaste projectsjabloon die kan worden gebruikt om nieuwe projecten te maken binnen AEM voor het beheer van workflows en taken voor het schrijven van inhoud.

>[!VIDEO](https://video.tv.adobe.com/v/16904/?quality=12&learn=on)

*Deze video geeft een korte demo van de voltooide workflow die in de onderstaande zelfstudie is gemaakt.*

## Inleiding {#introduction}

[[!DNL AEM Projects]](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html) is een functie van AEM die is ontworpen om het eenvoudiger te maken om alle workflows en taken die aan het maken van inhoud zijn gekoppeld, te beheren en te groeperen als onderdeel van een AEM Sites- of middelenimplementatie.

AEM Projecten worden geleverd met verschillende [OOTB-projectsjablonen](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#ProjectTemplates). Wanneer het creëren van een nieuw project, kunnen de auteurs van deze beschikbare malplaatjes kiezen. De grote AEM implementaties met unieke bedrijfsvereisten zullen de malplaatjes van het douaneProject willen tot stand brengen, die aan hun behoeften worden aangepast. Door een malplaatje van het douaneproject tot stand te brengen kunnen de ontwikkelaars het projectdashboard vormen, in douanewerkschema&#39;s, koppelen en extra bedrijfsrollen voor een project tot stand brengen. We zullen de structuur van een projectsjabloon bekijken en een voorbeeldsjabloon maken.

![Aangepaste projectkaart](./assets/develop-aem-projects/custom-project-card.png)

## Instellen

Deze zelfstudie doorloopt de code die nodig is om een aangepaste projectsjabloon te maken. U kunt het [bijgevoegde pakket](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip) downloaden en installeren naar een lokale omgeving om deze bij de zelfstudie te volgen. U kunt tot het volledige Gemaakt project ook toegang hebben dat op [GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide) wordt ontvangen.

* [Pakket met voltooide zelfstudies](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [Volledige gegevensopslagruimte voor code op GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)

Deze zelfstudie gaat uit van enige basiskennis van [AEM ontwikkelingspraktijken](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/the-basics.html) en enige vertrouwdheid met [AEM Maven project setup](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ht-projects-maven.html). Alle vermelde code is bedoeld om als verwijzing te worden gebruikt en zou slechts aan een [lokale AEM instantie](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingStarted) moeten worden opgesteld.

## Structuur van een projectsjabloon

Projectsjablonen moeten onder broncontrole worden geplaatst en moeten onder de toepassingsmap onder /apps blijven. In het ideale geval moeten ze in een submap worden geplaatst met de naamgevingsconventie van ***/projects/templates/**&lt;my-template>. Door na deze noemende overeenkomst te plaatsen zullen om het even welke nieuwe douanesjablonen automatisch beschikbaar worden aan auteurs wanneer het creëren van een project. De configuratie van beschikbare projectsjablonen is ingesteld op: **/content/projects/jcr:content** node by the **cq:allowedTemplates** property. Dit is standaard een reguliere expressie: **/(apps|libs)/.*/projects/templates/.***

De wortelknoop van een Malplaatje van het Project zal **jcr:primaryType** van **cq:Template** hebben. Onder het basisknooppunt van er zijn 3 knooppunten: **gadgets**, **rollen** en **workflows**. Deze knooppunten zijn allemaal **nt:unStructured**. Onder het hoofdknooppunt kan ook een bestand miniatuur.png staan dat wordt weergegeven wanneer u de sjabloon selecteert in de wizard Project maken.

De volledige nodestructuur:

```shell
/apps/<my-app>
    + projects (nt:folder)
         + templates (nt:folder)
              + <project-template-root> (cq:Template)
                   + gadgets (nt:unstructured)
                   + roles (nt:unstructured)
                   + workflows (nt:unstructured)
```

### Hoofdmap projectsjabloon

De wortelknoop van het projectmalplaatje zal van type **cq zijn:Template**. Op deze knoop kunt u eigenschappen **jcr:title** en **jcr:description** vormen die in de Create Tovenaar van het Project zullen worden getoond. Er is ook een bezit genoemd **tovenaar** die aan een vorm richt die de Eigenschappen van het project zal bevolken. De standaardwaarde van: **/libs/cq/core/content/projects/wizard/steps/defaultproject.html** zou in de meeste gevallen fijn moeten werken, aangezien het de gebruiker toestaat om de basiseigenschappen van het Project te bevolken en groepsleden toe te voegen.

**Let op: de wizard Project maken gebruikt de servlet Sling POST niet. In plaats daarvan worden waarden naar een aangepaste servlet gepost:**com.adobe.cq.projects.impl.servlet.ProjectServlet**. Hiermee moet u rekening houden bij het toevoegen van aangepaste velden.*

Een voorbeeld van een douanetovenaar kan voor het Malplaatje van het Project van de Vertaling worden gevonden: **/libs/cq/core/content/projects/wizard/translationproject/default project**.

### Gadgets {#gadgets}

Er zijn geen extra eigenschappen op deze knoop maar de kinderen van de gadget knoopcontrole die de Tegels van het Project het dashboard van het Project bevolken wanneer een nieuw Project wordt gecreeerd. [De projecttegels](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#ProjectTiles)  (ook wel gadgets of pods genoemd) zijn eenvoudige kaarten die de werkplek van een project vullen. Een volledige lijst met de bovenste tegels vindt u onder: **/libs/cq/gui/components/projects/admin/pod. **Projecteigenaars kunnen altijd tegels toevoegen/verwijderen nadat een project is gemaakt.

### Rollen {#roles}

Er zijn 3 [standaardrollen](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#UserRolesinaProject) voor elk project: **Waarnemers**, **Editors** en **Eigenaars**. Door kindknopen onder de rolknoop toe te voegen kunt u extra bedrijfsspecifieke Rollen van het Project voor het malplaatje toevoegen. U kunt deze rollen aan specifieke werkschema&#39;s dan verbinden verbonden aan het project.

### Workflows {#workflows}

Één de meest verleidelijke redenen voor het creëren van een Malplaatje van het douaneproject is dat het u de capaciteit geeft om de beschikbare werkschema&#39;s voor gebruik met het project te vormen. Dit kan OTB-workflows of aangepaste workflows zijn. Onder de **workflows** knoop moet er een **modelknoop** (ook `nt:unstructured`) zijn en de kindknopen onder specificeren de beschikbare werkschemamodellen. De eigenschap **modelId **wijst naar het workflowmodel onder /etc/workflow en de eigenschap **wizard** verwijst naar het dialoogvenster dat wordt gebruikt bij het starten van de workflow. Een groot voordeel van Projecten is de capaciteit om een douanedialoog (tovenaar) toe te voegen om bedrijfsspecifieke meta-gegevens bij het begin van het werkschema te vangen die verdere acties binnen het werkschema kunnen drijven.

```shell
<projects-template-root> (cq:Template)
    + workflows (nt:unstructured)
         + models (nt:unstructured)
              + <workflow-model> (nt:unstructured)
                   - modelId = points to the workflow model
                   - wizard = dialog used to start the workflow
```

## Een projectsjabloon maken {#creating-project-template}

Aangezien wij hoofdzakelijk het kopiëren/het vormen knopen zullen zijn zullen wij CRXDE Lite gebruiken. Open in uw lokale AEM [CRXDE Lite](http://localhost:4502/crx/de/index.jsp).

1. Begin door een nieuwe omslag te creëren onder `/apps/&lt;your-app-folder&gt;` genoemd `projects`. Maak een andere map onder de naam `templates`.

   ```shell
   /apps/aem-guides/projects-tasks/
                       + projects (nt:folder)
                                + templates (nt:folder)
   ```

1. Om dingen gemakkelijker te maken zullen wij onze douanemalplaatje van het bestaande Eenvoudige malplaatje van het Project beginnen.

   1. Kopieer en plak het knooppunt **/libs/cq/core/content/projects/templates/default** onder de map *templates* die in Stap 1 is gemaakt.

   ```shell
   /apps/aem-guides/projects-tasks/
                + templates (nt:folder)
                     + default (cq:Template)
   ```

1. U zou nu een weg zoals **/apps/aem-guides/projects-tasks/projects/templates/authoring-project** moeten hebben.

   1. Bewerk de eigenschappen **jcr:title** en **jcr:description** van het schrijver-projectknooppunt naar aangepaste titel- en beschrijvingswaarden.

      1. Verlaat **tovenaar** bezit wijzend aan de standaardeigenschappen van het Project.

   ```shell
   /apps/aem-guides/projects-tasks/projects/
            + templates (nt:folder)
                 + authoring-project (cq:Template)
                      - jcr:title = "Authoring Project"
                      - jcr:description = "A project to manage approval and publish process for AEM Sites or Assets"
                      - wizard = "/libs/cq/core/content/projects/wizard/steps/defaultproject.html"
   ```

1. Voor dit projectmalplaatje willen wij gebruik maken van Taken.
   1. Voeg een nieuw **nt:unStructured** knooppunt onder authoring-project/gadgets genoemd **tasks** toe.
   1. Voeg tekenreekseigenschappen toe aan het taakknooppunt voor **cardWeight** = &quot;100&quot;, **jcr:title**=&quot;Tasks&quot; en **sling:resourceType**=&quot;cq/gui/components/projects/admin/pod/taskpod&quot;.

   Nu zal [de tegel van Taken](https://docs.adobe.com/docs/en/aem/6-3/author/projects.html#Tasks) door gebrek verschijnen wanneer een nieuw project wordt gecreeerd.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
            + team (nt:unstructured)
            + asset (nt:unstructured)
            + work (nt:unstructured)
            + experiences (nt:unstructured)
            + projectinfo (nt:unstructured)
            ..
            + tasks (nt:unstructured)
                 - cardWeight = "100"
                 - jcr:title = "Tasks"
                 - sling:resourceType = "cq/gui/components/projects/admin/pod/taskpod"
   ```

1. We voegen een aangepaste rol fiatteur toe aan ons projectsjabloon.

   1. Onder het projectmalplaatje (creatie-project) voegt de knoop een nieuwe **nt:unStructured** knoop geëtiketteerd **rollen** toe.
   1. Voeg een andere **nt:unStructured** knoop geëtiketteerd goedkeuraars als kind van de rolknoop toe.
   1. Tekenreekseigenschappen **jcr:title** = &quot;**Approvers**&quot;, **roleclass** =&quot;**owner**&quot;, **roleid**=&quot;**fiatvers**&quot;.
      1. De naam van het knooppunt fiatteurs en jcr:title en roleid kunnen elke tekenreekswaarde zijn (zolang roleid uniek is).
      1. **De** verzamelingslassis bepaalt de toestemmingen die voor die rol worden toegepast die op de  [3 rollen] OOTB (https://docs.adobe.com/docs/en/aem/6-3/author/projects.html#User Roles in een Project) worden gebaseerd:  **eigenaar**,  **redacteur**, en  **waarnemer**.
      1. In het algemeen als de douanerol meer van een beheersrol is dan kan de klasse **eigenaar zijn;** als het een specifiekere auteursrol zoals Fotograaf of Ontwerper is dan **redacteur** roleclass zou moeten voldoende zijn. Het grote verschil tussen **owner** en **editor** is dat de projecteigenaars de projecteigenschappen kunnen bijwerken en nieuwe gebruikers aan het project kunnen toevoegen.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
           + approvers (nt:unstructured)
                - jcr:title = "Approvers"
                - roleclass = "owner"
                - roleid = "approver"
   ```

1. Door het Eenvoudige malplaatje van het Project te kopiëren zult u 4 gevormde werkschema&#39;s OOTB krijgen. Elk knooppunt onder workflows/modellen verwijst naar een specifieke workflow en een wizard voor begindialoogvenster voor die workflow. Later in deze zelfstudie maken we een aangepaste workflow voor dit project. Verwijder voorlopig de knooppunten onder workflow(en):

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
            + models (nt:unstructured)
               - (remove ootb models)
   ```

1. Om het voor inhoudsauteurs gemakkelijk te maken om het Malplaatje van het Project te identificeren kunt u een douaneminiatuur toevoegen. De aanbevolen grootte is 319x319 pixels.
   1. In CRXDE Lite maakt u een nieuw bestand op hetzelfde niveau als gadgets, rollen en workflowknooppunten met de naam **thumbnail.png**.
   1. Sla het knooppunt op, navigeer naar het knooppunt `jcr:content` en dubbelklik op de eigenschap `jcr:data` (klik niet op &#39;view&#39;).
      1. Hiermee wordt u gevraagd een dialoogvenster `jcr:data` te bewerken en kunt u een aangepaste miniatuur uploaden.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
       + thumbnail.png (nt:file)
   ```

Voltooide vertegenwoordiging van XML van het Malplaatje van het Project:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
    jcr:description="A project to manage approval and publish process for AEM Sites or Assets"
    jcr:primaryType="cq:Template"
    jcr:title="Authoring Project"
    ranking="{Long}1"
    wizard="/libs/cq/core/content/projects/wizard/steps/defaultproject.html">
    <jcr:content
        jcr:primaryType="nt:unstructured"
        detailsHref="/projects/details.html"/>
    <gadgets jcr:primaryType="nt:unstructured">
        <team
            jcr:primaryType="nt:unstructured"
            jcr:title="Team"
            sling:resourceType="cq/gui/components/projects/admin/pod/teampod"
            cardWeight="60"/>
        <tasks
            jcr:primaryType="nt:unstructured"
            jcr:title="Tasks"
            sling:resourceType="cq/gui/components/projects/admin/pod/taskpod"
            cardWeight="100"/>
        <work
            jcr:primaryType="nt:unstructured"
            jcr:title="Workflows"
            sling:resourceType="cq/gui/components/projects/admin/pod/workpod"
            cardWeight="80"/>
        <experiences
            jcr:primaryType="nt:unstructured"
            jcr:title="Experiences"
            sling:resourceType="cq/gui/components/projects/admin/pod/channelpod"
            cardWeight="90"/>
        <projectinfo
            jcr:primaryType="nt:unstructured"
            jcr:title="Project Info"
            sling:resourceType="cq/gui/components/projects/admin/pod/projectinfopod"
            cardWeight="100"/>
    </gadgets>
    <roles jcr:primaryType="nt:unstructured">
        <approvers
            jcr:primaryType="nt:unstructured"
            jcr:title="Approvers"
            roleclass="owner"
            roleid="approvers"/>
    </roles>
    <workflows
        jcr:primaryType="nt:unstructured"
        tags="[]">
        <models jcr:primaryType="nt:unstructured">
        </models>
    </workflows>
</jcr:root>
```

## Het testen van het malplaatje van het douaneproject

Nu kunnen wij ons Malplaatje van het Project testen door een nieuw Project te creëren.

1. U zou het douanemalplaatje als één van de opties voor projectverwezenlijking moeten zien.

   ![Sjabloon kiezen](./assets/develop-aem-projects/choose-template.png)

1. Na het selecteren van het douanemalplaatje klik &quot;daarna&quot;en merk op dat wanneer het bevolken van de Leden van het Project u hen als rol kunt toevoegen Approver.

   ![Goedkeuren](./assets/develop-aem-projects/user-approver.png)

1. Klik op Maken om het maken van het project te voltooien op basis van de aangepaste sjabloon. U zult op het dashboard van het Project opmerken dat de Tegel van Taken en de andere tegels die onder gadgets worden gevormd automatisch verschijnen.

   ![Taaktegel](./assets/develop-aem-projects/tasks-tile.png)


## Waarom workflow?

Traditioneel AEM workflows die rond een goedkeuringsproces worden gecentreerd, hebben workflowstappen van deelnemers gebruikt. AEM Inbox omvat detail rond Taken en Werkschema en verbeterde integratie met AEM Projecten. Deze eigenschappen maken het gebruiken van de het processtappen van de Taak van Projecten creëren een aantrekkelijkere optie.

### Waarom taken?

Het gebruiken van een Stap van de Aanmaak van de Taak over de traditionele stappen van de Deelnemer biedt een paar voordelen aan:

* **Begin- en vervaldatum**  - maakt het voor auteurs gemakkelijk om hun tijd te beheren, maakt de nieuwe functie van de Kalender gebruik van deze data.
* **Prioriteit**  - ingebouwde prioriteiten van Laag, Normaal, en Hoog staan auteurs toe om het werk voorrang te geven
* **Verbonden Commentaren**  - aangezien de auteurs aan een taak werken hebben zij de capaciteit om commentaren te verlaten die samenwerking verhogen
* **Zichtbaarheid**  - De tegels van de Taak en de meningen met Projecten staan managers toe om te bekijken hoe de tijd wordt besteed
* **Projectintegratie**  - Taken zijn al geïntegreerd met projectrollen en dashboards

Als de stappen van de Deelnemer, kunnen de Taken dynamisch worden toegewezen en worden verpletterd. Taakmetagegevens zoals Titel, Prioriteit kunnen ook dynamisch worden ingesteld op basis van eerdere acties, zoals in de volgende zelfstudie.

Terwijl de Taken sommige voordelen over de Stappen van de Deelnemer hebben zij extra overheadkosten dragen, en zijn niet zo nuttig buiten een Project. Bovendien moet al dynamisch gedrag van Taken worden gecodeerd gebruikend manuscripten ecma die zijn eigen beperkingen hebben.

## Voorbeelden van vereisten voor het gebruik van hoofdletters en kleine letters {#goals-tutorial}

![Workflowprocesdiagram](./assets/develop-aem-projects/workflow-process-diagram.png)

In het bovenstaande diagram worden de vereisten op hoog niveau voor onze workflow voor monstergoedkeuring beschreven.

De eerste stap bestaat uit het maken van een taak om het bewerken van een stuk inhoud te voltooien. De aanvrager van de workflow kan de eerste taak kiezen.

Zodra de eerste taak volledig is zal toegewezen drie opties hebben om het werkschema te verpletteren:

**Normaal ** - het normale verpletteren leidt tot een taak die aan de groep van de fiatteur van het Project wordt toegewezen om te herzien en goed te keuren. De prioriteit van de taak is Normaal en de vervaldatum is vijf dagen vanaf het tijdstip waarop deze wordt gemaakt.

**Het ruw** -ruitenverpletteren leidt ook tot een taak die aan de groep van de Fiatteur van het Project wordt toegewezen. De prioriteit van de taak is Hoog en de vervaldatum is slechts 1 dag.

**Bypass**  - in deze voorbeeldwerkstroom heeft de eerste deelnemer de optie om de goedkeuringsgroep te omzeilen. (ja dit zou het doel van een &quot;Goedkeuring&quot;werkschema kunnen verslaan maar het staat ons toe om extra verpletterende mogelijkheden te illustreren)

De Approver Groep kan de inhoud goedkeuren of het terugsturen naar de aanvankelijke toegewezen voor herwerk. Als een nieuwe taak wordt teruggestuurd voor een nieuwe taak, wordt er een nieuwe taak gemaakt met het label &#39;Terugsturen voor opnieuw werken&#39;.

In de laatste stap van de workflow wordt gebruikgemaakt van de processtap Pagina/element activeren en wordt de lading gerepliceerd.

## Het workflowmodel maken

1. Navigeer in het menu AEM Start naar Extra -> Workflow -> Modellen. Klik op &#39;Maken&#39; in de rechterbovenhoek om een nieuw workflowmodel te maken.

   Geef het nieuwe model een titel: &quot;Workflow voor goedkeuring van inhoud&quot; en een URL-naam: &quot;content-approval-workflow&quot;.

   ![Het dialoogvenster Workflow maken](./assets/develop-aem-projects/workflow-create-dialog.png)

   Lees hier voor meer informatie over [het maken van workflows](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-models.html).

1. Aangepaste workflows kunt u het beste groeperen in een eigen map onder /etc/workflow/modellen. Maak in CRXDE Lite een nieuwe **&#39;not:folder&#39;** onder /etc/workflow/models genaamd **&quot;aem-guides&quot;**. Door een submap toe te voegen, zorgt u ervoor dat aangepaste workflows niet per ongeluk worden overschreven tijdens upgrades of Service Pack-installaties.

   *Houd er rekening mee dat het belangrijk is om de map of aangepaste workflows nooit onder submappen van het pad te plaatsen, zoals /etc/workflow/models/dam of /etc/workflow/models/projects, omdat de volledige submap ook kan worden overschreven door upgrades of servicepacks.

   ![Locatie van workflowmodel in 6.3](./assets/develop-aem-projects/custom-workflow-subfolder.png)

   Locatie van workflowmodel in 6.3

   >[!NOTE]
   >
   >Als AEM 6.4+ wordt gebruikt, is de locatie van de workflow gewijzigd. Zie [hier voor meer informatie.](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-best-practices.html#LocationsWorkflowModels)

   Als AEM 6.4+ wordt gebruikt, wordt het workflowmodel gemaakt onder `/conf/global/settings/workflow/models`. Herhaal de bovenstaande stappen met de map /conf en voeg een submap met de naam `aem-guides` toe en verplaats `content-approval-workflow` eronder.

   ![Moderne workflowdefinitie ](./assets/develop-aem-projects/modern-workflow-definition-location.png)
locationLocation van workflowmodel in 6.4+

1. De introductie in AEM 6.3 is de mogelijkheid werkstroomfasen toe te voegen aan een bepaalde werkstroom. De fasen worden aan de gebruiker weergegeven vanuit het Postvak In op het tabblad Workflowinfo. Het toont de gebruiker het huidige werkgebied in het werkschema evenals de stadia voorafgaand aan en na het.

   Als u de fasen wilt configureren, opent u het dialoogvenster Pagina-eigenschappen vanuit de SideKick. Het vierde tabblad heet &quot;Stages&quot;. Voeg de volgende waarden toe om de drie fasen van deze workflow te configureren:

   1. Inhoud bewerken
   1. Goedkeuring
   1. Publicatie

   ![workflowconfiguratie](./assets/develop-aem-projects/workflow-model-stage-properties.png)

   Configureer de werkstroomfasen in het dialoogvenster Pagina-eigenschappen.

   ![werkstroomvoortgangsbalk](./assets/develop-aem-projects/workflow-info-progress.png)

   De werkstroomvoortgangsbalk zoals deze wordt weergegeven in het AEM Inbox.

   U kunt desgewenst een **Afbeelding** uploaden naar de Pagina-eigenschappen die worden gebruikt als de workflowminiatuur wanneer gebruikers deze selecteren. Afbeeldingsafmetingen moeten 319x319 pixels zijn. Het toevoegen van een **Beschrijving** aan de Eigenschappen van de Pagina zal ook verschijnen wanneer een gebruiker gaat om het werkschema te selecteren.

1. Het workflowproces Projecttaak maken is ontworpen om een taak te maken als een stap in de workflow. Pas na het voltooien van de taak gaat de workflow verder. Een krachtig aspect van de stap van de Taak van het Project creëren is dat het werkschema meta-gegevens waarden kan lezen en die gebruiken om de taak dynamisch tot stand te brengen.

   Verwijder eerst de Stap van de Deelnemer die standaard wordt gemaakt. Van Sidetrap in het componentenmenu breidt **&quot;Projecten&quot;** subkop uit en sleept **&quot;creëren de Taak van het Project&quot;** op het model.

   Dubbelklik op de stap Projecttaak maken om het workflowdialoogvenster te openen. Configureer de volgende eigenschappen:

   Dit tabblad wordt veel gebruikt voor alle stappen in het workflowproces en we stellen de Titel en Beschrijving in (deze zijn niet zichtbaar voor de eindgebruiker). Het belangrijke bezit dat wij zullen plaatsen is het Stadium van het Werkschema aan **&quot;geeft Inhoud&quot;** van het drop-down menu uit.

   ```shell
   Common Tab
   -----------------
       Title = "Start Task Creation"
       Description = "This the first task in the Workflow"
       Workflow Stage = "Edit Content"
   ```

   Het workflowproces Projecttaak maken is ontworpen om een taak te maken als een stap in de workflow. Met het tabblad Taak kunnen we alle waarden van de taak instellen. In ons geval willen we dat de ontvanger dynamisch is, dus laten we hem leeg. De rest van de eigenschapswaarden:

   ```shell
   Task Tab
   -----------------
       Name* = "Edit Content"
       Task Priority = "Medium"
       Description = "Edit the content and finalize for approval. Once finished submit for approval."
       Due In - Days = "2"
   ```

   Het verpletterende lusje is een facultatieve dialoog die beschikbare acties voor de gebruiker kan specificeren die de taak voltooit. Deze handelingen zijn alleen tekenreekswaarden en worden opgeslagen in de metagegevens van de workflow. Deze waarden kunnen door manuscripten en/of processtappen later in het werkschema worden gelezen om de werkschema dynamisch &quot;te leiden&quot;. Op basis van de [workflowdoelstellingen](#goals-tutorial) voegen we drie acties toe aan dit tabblad:

   ```shell
   Routing Tab
   -----------------
       Actions =
           "Normal Approval"
           "Rush Approval"
           "Bypass Approval"
   ```

   Dit lusje staat ons toe om een pre-Create Manuscript van de Taak te vormen waar wij kunnen programmatically diverse waarden van de Taak beslissen alvorens het wordt gecreeerd. U kunt het script verwijzen naar een extern bestand of een kort script rechtstreeks in het dialoogvenster insluiten. In ons geval wijzen we het script voor het maken van taken naar een extern bestand. In Stap 5 zullen wij dat manuscript tot stand brengen.

   ```shell
   Advanced Settings Tab
   -----------------
      Pre-Create Task Script = "/apps/aem-guides/projects/scripts/start-task-config.ecma"
   ```

1. In de vorige stap hebben we verwezen naar een Pre-Create Taakscript. Wij zullen dat manuscript nu tot stand brengen waarin wij de Ontvanger van de Taak zullen plaatsen die op de waarde van een werkschemawaarde &quot;**assignee**&quot;wordt gebaseerd. De **&quot;assignee&quot;** waarde zal worden geplaatst wanneer het werkschema weg wordt gezet. We zullen ook de metagegevens van de workflow lezen om dynamisch de prioriteit van de taak te kiezen door de waarde &quot;**taskPriority&quot;** van de metagegevens van de workflow en de waarde **&quot;taskdueDate&quot; **te lezen om dynamisch in te stellen wanneer de eerste taak moet worden uitgevoerd.

   Voor organisatorische doeleinden hebben we onder onze toepassingsmap een map gemaakt voor al onze projectgerelateerde scripts: **/apps/aem-guides/projects-tasks/projects/scripts**. Maak een nieuw bestand onder deze map met de naam **&quot;start-task-config.ecma&quot;**. *Opmerking zorgt ervoor dat het pad naar het bestand start-task-config.ecma overeenkomt met het pad dat is ingesteld op het tabblad Geavanceerde instellingen in stap 4.

   Voeg het volgende toe als de inhoud van het bestand:

   ```
   // start-task-config.ecma
   // Populate the task using values stored as workflow metadata originally posted by the start workflow wizard
   
   // set the assignee based on start workflow wizard
   var assignee = workflowData.getMetaDataMap().get("assignee", Packages.java.lang.String);
   task.setCurrentAssignee(assignee);
   
   //Set the due date for the initial task based on start workflow wizard
   var dueDate = workflowData.getMetaDataMap().get("taskDueDate", Packages.java.util.Date);
   if (dueDate != null) {
       task.setProperty("taskDueDate", dueDate);
   }
   
   //Set the priority based on start workflow wizard
   var taskPriority = workflowData.getMetaDataMap().get("taskPriority", "Medium");
   task.setProperty("taskPriority", taskPriority);
   ```

1. Ga terug naar de workflow voor goedkeuring van inhoud. Sleep de **OR Split** component (gevonden in de Sidetrap onder de categorie &#39;Workflow&#39;) onder de **Start Taak** Stap. Selecteer in het dialoogvenster Algemeen het keuzerondje voor 3 vertakkingen. OR Split zal de waarde **&quot;lastTaskAction&quot;** van werkschemameta-gegevens lezen om de route van het werkschema te bepalen. Het **&quot;lastTaskAction&quot;** bezit zal aan één van de waarden van het Verpletterende Lusje worden geplaatst dat in Stap 4 wordt gevormd. Vul voor elk tabblad Vertakking het tekstgebied **Script** met de volgende waarden in:

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Normal Approval") {
       return true;
   }
   
   return false;
   }
   ```

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Rush Approval") {
       return true;
   }
   
   return false;
   }
   ```

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Bypass Approval") {
       return true;
   }
   
   return false;
   }
   ```

   *Note wij doen een directe gelijke van het Koord om de route te bepalen zodat is het belangrijk dat de waarden die in de manuscripten van de Tak worden geplaatst de waarden van de Route aanpassen die in Stap 4 worden geplaatst.

1. Sleep een andere stap &quot;**Projecttaak maken**&quot; naar het model helemaal links (vertakking 1) onder de splitsing OR. Vul het dialoogvenster met de volgende eigenschappen in:

   ```
   Common Tab
   -----------------
       Title = "Approval Task Creation"
       Description = "Create a an approval task for Project Approvers. Priority is Medium."
       Workflow Stage = "Approval"
   
   Task Tab
   ------------
       Name* = "Approve Content for Publish"
       Task Priority = "Medium"
       Description = "Approve this content for publication."
       Days = "5"
   
   Routing Tab - Actions
   ----------------------------
       "Approve and Publish"
       "Send Back for Revision"
   ```

   Aangezien dit de Normale route van de Goedkeuring is, wordt de prioriteit van de taak geplaatst aan Middel. Daarnaast geven we de groep fiatteurs vijf dagen om de taak te voltooien. De toegewezen persoon wordt leeg gelaten op het tabblad Taak aangezien wij dit dynamisch zullen toewijzen op het tabblad Geavanceerde instellingen. We geven de groep fiatteurs twee mogelijke routes bij het uitvoeren van deze taak: **&quot;goedkeuren en publiceren&quot;** als zij de inhoud goedkeuren en het kan worden gepubliceerd en **&quot;verzend terug voor Herziening&quot;** als er kwesties zijn die de originele redacteur moet verbeteren. De fiatteur kan commentaren verlaten die de originele redacteur zal zien of is het werkschema teruggekeerd aan hem/haar.

Eerder in deze zelfstudie hebben we een projectsjabloon gemaakt dat een rol van fiatteurs bevatte. Telkens als een nieuw Project van dit Malplaatje wordt gecreeerd zal een project-specifieke Groep voor de rol worden gecreeerd Approvers. Enkel als een Stap van de Deelnemer kan een Taak slechts aan een Gebruiker of een Groep worden toegewezen. We willen deze taak toewijzen aan de projectgroep die overeenkomt met de groep fiatteurs. Alle werkschema&#39;s die van binnen een Project worden gelanceerd zullen meta-gegevens hebben die de Rollen van het Project aan de specifieke groep van het Project in kaart brengen.

Kopieer en plak de volgende code in het tekstgebied **Script** van de **Geavanceerde Montages **tabel. Deze code zal de werkschemameta-gegevens lezen en zal de taak aan de groep Approvers van het Project toewijzen. Als het niet de waarde van de fiatversgroep kan vinden zal het terug vallen om de taak aan de groep van Beheerders toe te wijzen.

```
var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");

task.setCurrentAssignee(projectApproverGrp);
```

1. Sleep een andere stap &quot;**Projecttaak maken**&quot; naar het model naar de middelste vertakking (vertakking 2) onder OF. Vul het dialoogvenster met de volgende eigenschappen in:

   ```
   Common Tab
   -----------------
       Title = "Rush Approval Task Creation"
       Description = "Create a an approval task for Project Approvers. Priority is High."
       Workflow Stage = "Approval"
   
   Task Tab
   ------------
       Name* = "Rush Approve Content for Publish"
       Task Priority = "High"
       Description = "Rush approve this content for publication."
       Days = "1"
   
   Routing Tab - Actions
   ----------------------------
       "Approve and Publish"
       "Send Back for Revision"
   ```

   Aangezien dit de route van de Goedkeuring van Rusland is wordt de prioriteit van de taak geplaatst aan Hoog. Daarnaast geven we de groep fiatteurs slechts één dag om de taak te voltooien. De toegewezen persoon wordt leeg gelaten op het tabblad Taak aangezien wij dit dynamisch zullen toewijzen op het tabblad Geavanceerde instellingen.

   We kunnen hetzelfde scriptfragment opnieuw gebruiken als in Stap 7 om het tekstgebied **Script** op de tab* Geavanceerde instellingen **te vullen. Kopiëren en de onderstaande code plakken:

   ```
   var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");
   
   task.setCurrentAssignee(projectApproverGrp);
   ```

1. Sleep een component** Geen bewerking** naar de vertakking uiterst rechts (vertakking 3). De component Geen bewerking voert geen handeling uit en wordt onmiddellijk uitgevoerd, omdat de oorspronkelijke editor de goedkeuringsstap wil omzeilen. Technisch gezien konden wij deze Tak zonder enige werkschemastappen verlaten, maar als beste praktijken zullen wij een Geen stap van de Verrichting toevoegen. Dit maakt het aan andere ontwikkelaars duidelijk wat het doel van Tak 3 is.

   Dubbelklik op de workflowstap en configureer de titel en beschrijving:

   ```
   Common Tab
   -----------------
       Title = "Bypass Approval"
       Description = "Placeholder step to indicate that the original editor decided to bypass the approver group."
   ```

   ![workflowmodel OR split](./assets/develop-aem-projects/workflow-stage-after-orsplit.png)

   Het model van het Werkschema zou als dit moeten kijken nadat alle drie takken in OF verdeeld zijn gevormd.

1. Aangezien de groep Approvers de optie heeft om het werkschema terug naar de originele redacteur voor verdere herzieningen te verzenden zullen wij op **Goto** stap vertrouwen om de laatste uitgevoerde actie te lezen en het werkschema aan het begin te leiden of het te laten verdergaan.

   Sleep en zet de component Goto Step (gevonden in de Sidetrap onder Workflow) onder OF splitst op de plaats waar deze opnieuw wordt samengevoegd. Dubbelklik op de volgende eigenschappen en configureer deze in het dialoogvenster:

   ```
   Common Tab
   ----------------
       Title = "Goto Step"
       Description = "Based on the Approver groups action route the workflow to the beginning or continue and publish the payload."
   
   Process Tab
   ---------------
       The step to go to. = "Start Task Creation"
   ```

   Het laatste stuk dat wij zullen vormen is het Manuscript als deel van de stap van het proces van Goto. De scriptwaarde kan worden ingesloten via het dialoogvenster of worden geconfigureerd om naar een extern bestand te wijzen. Het Goto-script moet een **function check()** bevatten en true retourneren als de workflow naar de opgegeven stap moet gaan. Als er false worden geretourneerd, gaat de workflow verder.

   Als de goedkeurende groep **&quot;achteruit voor Herziening&quot;kiest** actie (gevormd in Stap 7 en 8) dan willen wij de werkschema aan **&quot;de Aanmaak van de Taak van het Begin&quot;** stap terugkeren.

   Voeg op het tabblad Proces het volgende fragment toe aan het tekstgebied Script:

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Send Back for Revision") {
       return true;
   }
   
   return false;
   }
   ```

1. Voor het publiceren van de lading gebruiken wij de weg **pagina/element** stap van het Proces activeren. Deze processtap vereist weinig configuratie en zal de nuttige lading van het werkschema aan de replicatierij voor activering toevoegen. Wij zullen de stap onder de stap van de Goto toevoegen en op deze manier kan het slechts worden bereikt als de groep Approver de inhoud voor publicatie heeft goedgekeurd of de originele redacteur de route van de Goedkeuring van de Bypass koos.

   Sleep **Activeer Pagina/element** de stap van het Proces (die in Sidetrap onder Werkschema WCM wordt gevonden) onder Goto Stap in het model.

   ![workflowmodel voltooid](assets/develop-aem-projects/workflow-model-final.png)

   Hoe moet het workflowmodel eruitzien nadat u de stap Ga naar hebt toegevoegd en de stap Pagina/element activeren hebt geactiveerd.

1. Als de Approver groep de inhoud voor herziening terugstuurt willen wij de originele redacteur het weten. Dit kunnen we bereiken door de eigenschappen voor het maken van taken dynamisch te wijzigen. Wij zullen van de lastActionTake bezitswaarde van **&quot;terugsturen voor Herziening&quot;**. Als die waarde aanwezig is, zullen wij de titel en de beschrijving wijzigen om erop te wijzen dat deze taak het resultaat van inhoud is die voor herziening wordt teruggestuurd. Wij zullen ook de prioriteit aan **&quot;Hoog&quot;** bijwerken zodat het het eerste punt is de redacteur aan werkt. Tot slot zullen wij de taak vervaldatum aan één dag plaatsen vanaf wanneer het werkschema voor revisie werd teruggestuurd.

   Vervang het begin `start-task-config.ecma` manuscript (gecreeerd in Stap 5) met het volgende:

   ```
   // start-task-config.ecma
   // Populate the task using values stored as workflow metadata originally posted by the start workflow wizard
   
   // set the assignee based on start workflow wizard
   var assignee = workflowData.getMetaDataMap().get("assignee", Packages.java.lang.String);
   task.setCurrentAssignee(assignee);
   
   //Set the due date for the initial task based on start workflow wizard
   var dueDate = workflowData.getMetaDataMap().get("taskDueDate", Packages.java.util.Date);
   if (dueDate != null) {
       task.setProperty("taskDueDate", dueDate);
   }
   
   //Set the priority based on start workflow wizard
   var taskPriority = workflowData.getMetaDataMap().get("taskPriority", "Medium");
   task.setProperty("taskPriority", taskPriority);
   
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   //change the title and priority if the approver group sent back the content
   if(lastAction == "Send Back for Revision") {
     var taskName = "Review and Revise Content";
   
     //since the content was rejected we will set the priority to High for the revison task
     task.setProperty("taskPriority", "High"); 
   
     //set the Task name (displayed as the task title in the Inbox) 
     task.setProperty("name", taskName);
     task.setProperty("nameHierarchy", taskName);
   
     //set the due date of this task 1 day from current date
     var calDueDate = Packages.java.util.Calendar.getInstance();
     calDueDate.add(Packages.java.util.Calendar.DATE, 1);
     task.setProperty("taskDueDate", calDueDate.getTime());
   
   }
   ```

## De wizard &quot;Start workflow&quot; maken {#start-workflow-wizard}

Wanneer u een workflow uit een project verwijdert, moet u een wizard opgeven om de workflow te starten. De standaardwizard: `/libs/cq/core/content/projects/workflowwizards/default_workflow` staat de gebruiker toe om een Titel van het Werkschema, een begincommentaar, en een nuttige ladingspad voor het werkschema in te gaan om te lopen. Er zijn ook verschillende andere voorbeelden te vinden onder: `/libs/cq/core/content/projects/workflowwizards`.

Het maken van een aangepaste wizard kan zeer krachtig zijn, omdat u essentiële informatie kunt verzamelen voordat de workflow start. De gegevens worden opgeslagen als onderdeel van de metagegevens van de werkstroom en werkstroomprocessen kunnen dit lezen en het gedrag dynamisch wijzigen op basis van de ingevoerde waarden. We maken een aangepaste wizard die de eerste taak dynamisch op basis van een beginwaarde van de wizard toewijst.

1. In CRXDE-Lite zullen wij een subomslag onder `/apps/aem-guides/projects-tasks/projects` omslag creëren genoemd &quot;tovenaars&quot;. Kopieer de standaardwizard van: `/libs/cq/core/content/projects/workflowwizards/default_workflow` onder de nieuwe wizards-map en wijzig de naam ervan in **content-approval-start**. Het volledige pad moet nu zijn: `/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start`.

   De standaardwizard is een wizard met twee kolommen en de eerste kolom bevat Titel, Beschrijving en Miniatuur van het workflowmodel geselecteerd. De tweede kolom bevat velden voor de titel van de workflow, Opmerking starten en Pad loonbelasting. De wizard is een standaardaanraakinterface-formulier en maakt gebruik van de standaardformuliercomponenten [Granite UI](https://docs.adobe.com/docs/en/aem/6-5/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/index.html) om de velden te vullen.

   ![wizard voor inhoudsgoedkeuring](./assets/develop-aem-projects/content-approval-start-wizard.png)

1. Wij zullen een extra gebied aan de tovenaar toevoegen die zal worden gebruikt om de toegewezen van de eerste taak in het werkschema (zie [Create het Model van het Werkschema](#create-workflow-model) te plaatsen: Stap 5).

   Onder `../content-approval-start/jcr:content/items/column2/items` maak een nieuw knooppunt van het type `nt:unstructured` met de naam **&quot;assign&quot;**. Wij zullen de component van de Plukker van de Gebruiker van Projecten gebruiken (die van [de Component van de Plukker van de Gebruiker van Granite ](https://docs.adobe.com/docs/en/aem/6-5/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/userpicker/index.html)) wordt gebaseerd. Met dit formulierveld kunt u eenvoudig de selectie van gebruikers en groepen beperken tot gebruikers die tot het huidige project behoren.

   Hieronder ziet u de XML-representatie van het knooppunt **assign**:

   ```xml
   <assign
       granite:class="js-cq-project-user-picker"
       jcr:primaryType="nt:unstructured"
       sling:resourceType="cq/gui/components/projects/admin/userpicker"
       fieldLabel="Assign To"
       hideServiceUsers="{Boolean}true"
       impersonatesOnly="{Boolean}true"
       showOnlyProjectMembers="{Boolean}true"
       name="assignee"
       projectPath="${param.project}"
       required="{Boolean}true"/>
   ```

1. Wij zullen ook een prioritair selectiegebied toevoegen dat de prioriteit van de eerste taak in het werkschema zal bepalen (zie [het Model van het Werkschema creëren](#create-workflow-model): Stap 5).

   Onder `/content-approval-start/jcr:content/items/column2/items` maak een nieuw knooppunt van het type `nt:unstructured` met de naam **priority**. Met de [Selectie van graniet-interface](https://docs.adobe.com/docs/en/aem/6-2/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/select/index.html) wordt het formulierveld gevuld.

   Onder de **priority**-node voegen we een **items**-knooppunt van **nt:unStructured** toe. Onder de **items** knoop voeg 3 extra knopen toe om de selectieopties voor Hoog, Normaal, en Laag te bevolken. Elk knooppunt is van het type **nt:unStructured** en moet een **text** en **value** bezit hebben. Zowel de tekst als de waarde moeten dezelfde waarde hebben:

   1. Hoog
   1. Normaal
   1. Laag

   Voeg voor het middelste knooppunt een extra Booleaanse eigenschap met de naam &quot;**selected&quot;** toe met een waarde ingesteld op **true**. Zo weet u zeker dat Medium de standaardwaarde in het selectieveld is.

   Hieronder ziet u een XML-representatie van de nodestructuur en -eigenschappen:

   ```xml
   <priority
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/select"
       fieldLabel="Task Priority"
       name="taskPriority">
           <items jcr:primaryType="nt:unstructured">
               <high
                   jcr:primaryType="nt:unstructured"
                   text="High"
                   value="High"/>
               <medium
                   jcr:primaryType="nt:unstructured"
                   selected="{Boolean}true"
                   text="Medium"
                   value="Medium"/>
               <low
                   jcr:primaryType="nt:unstructured"
                   text="Low"
                   value="Low"/>
               </items>
   </priority>
   ```

1. De aanvrager van de workflow kan de vervaldatum van de eerste taak instellen. We gebruiken het formulierveld [Granite UI DatePicker](https://docs.adobe.com/docs/en/aem/6-5/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/datepicker/index.html) om deze invoer vast te leggen. We voegen ook een verborgen veld met een [TypeHint](https://sling.apache.org/documentation/bundles/manipulating-content-the-slingpostservlet-servlets-post.html#typehint) toe om ervoor te zorgen dat de invoer wordt opgeslagen als een eigenschap van het type Date in het JCR.

   Voeg twee **nt:ongestructureerde** knopen met de volgende eigenschappen toe die hieronder in XML worden vertegenwoordigd:

   ```xml
   <duedate
       granite:rel="project-duedate"
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/datepicker"
       displayedFormat="YYYY-MM-DD HH:mm"
       fieldLabel="Due Date"
       minDate="today"
       name="taskDueDate"
       type="datetime"/>
   <duedatetypehint
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/hidden"
       name="taskDueDate@TypeHint"
       type="datetime"
       value="Calendar"/>
   ```

1. U kunt de volledige code voor de dialoog van de begintovenaar [hier](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/projects-tasks-guide/ui.apps/src/main/content/jcr_root/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start/.content.xml) bekijken.

## De workflow en de projectsjabloon {#connecting-workflow-project} verbinden

Het laatste wat we moeten doen, is ervoor zorgen dat het workflowmodel beschikbaar is om van binnen een van de Projecten te worden afgevoerd. Om dit te doen, moeten wij het Malplaatje van het Project opnieuw bezoeken wij in Deel 1 van deze reeks creeerden.

De configuratie van het Werkschema is een gebied van een Malplaatje van het Project dat de beschikbare werkschema&#39;s specificeert die met dat project moeten worden gebruikt. De configuratie is ook verantwoordelijk voor het specificeren van de Tovenaar van het Werkschema van het Begin wanneer het schoppen van het werkschema (dat wij in [vorige stappen) ](#start-workflow-wizard) creeerden. De configuratie van het Werkschema van een Malplaatje van het Project is &quot;levend&quot;betekenend dat het bijwerken van de werkschemaconfiguratie nieuwe gecreeerde Projecten evenals bestaande Projecten zal uitvoeren die het malplaatje gebruiken.

1. In CRXDE-Lite navigeer aan het auteursprojectmalplaatje vroeger bij `/apps/aem-guides/projects-tasks/projects/templates/authoring-project/workflows/models` werd gecreeerd.

   Onder de modelknoop voeg een nieuwe knoop genoemd **contentApproval** met een knooptype van **nt:unStructured** toe. Voeg de volgende eigenschappen toe aan het knooppunt:

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/etc/workflow/models/aem-guides/content-approval-workflow/jcr:content/model"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

   >[!NOTE]
   >
   >Als AEM 6.4 wordt gebruikt, is de locatie van de workflow gewijzigd. Wijs de eigenschap `modelId` onder `/var/workflow/models/aem-guides/content-approval-workflow` toe aan de locatie van het workflowmodel van de runtime
   >
   >
   >Zie [hier voor meer informatie over de wijziging in de locatie van de workflow.](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-best-practices.html#LocationsWorkflowModels)

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/var/workflow/models/aem-guides/content-approval-workflow"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

1. Zodra het werkschema van de Goedkeuring van de Inhoud aan het Malplaatje van het Project is toegevoegd zou het beschikbaar moeten zijn om uit de Tegel van het Werkschema van het project te schoppen. Ga door en start en speel rond met de verschillende routines die we hebben gecreëerd.

## Ondersteunende materialen

* [Voltooid lespakket downloaden](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [Volledige gegevensopslagruimte voor code op GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)
* [Documentatie AEM projecten](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html)
