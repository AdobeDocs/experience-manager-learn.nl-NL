---
title: Ontwikkelprojecten in AEM
description: Een zelfstudie over de ontwikkeling van AEM-projecten. In deze zelfstudie maken we een aangepaste projectsjabloon die kan worden gebruikt om nieuwe projecten te maken in AEM voor het beheer van workflows en taken voor het schrijven van inhoud.
version: Experience Manager 6.4, Experience Manager 6.5
feature: Projects, Workflow
doc-type: Tutorial
topic: Development
role: Developer
level: Beginner
exl-id: 9bfe3142-bfc1-4886-85ea-d1c6de903484
duration: 1417
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '4441'
ht-degree: 0%

---

# Ontwikkelprojecten in AEM

Dit is een zelfstudie over ontwikkeling die laat zien hoe u [!DNL AEM Projects] kunt ontwikkelen. In deze zelfstudie maken we een aangepaste projectsjabloon die kan worden gebruikt om projecten te maken in AEM voor het beheer van workflows en taken voor het schrijven van inhoud.

>[!VIDEO](https://video.tv.adobe.com/v/16904?quality=12&learn=on)

*Deze video geeft een korte manifestatie van het gebeëindigde werkschema dat in het hieronder leerprogramma wordt gecreeerd.*

## Inleiding {#introduction}

[[!DNL AEM Projects] ](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/authoring/projects/projects) is een eigenschap van AEM wordt ontworpen om het gemakkelijker te maken om alle werkschema&#39;s en taken te beheren en te groeperen verbonden aan inhoudsverwezenlijking als deel van een implementatie van AEM Sites of van Assets.

De Projecten van AEM komen met verscheidene [ OOTB projectmalplaatjes ](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/authoring/projects/projects). Wanneer het creëren van een project, kunnen de auteurs van deze beschikbare malplaatjes kiezen. Grote AEM-implementaties met unieke bedrijfsvereisten willen aangepaste projectsjablonen maken die zijn afgestemd op hun behoeften. Door een malplaatje van het douaneproject tot stand te brengen kunnen de ontwikkelaars het projectdashboard vormen, in douanewerkschema&#39;s, koppelen en extra bedrijfsrollen voor een project tot stand brengen. We zullen de structuur van een projectsjabloon bekijken en een voorbeeldsjabloon maken.

![ Kaart van het Project van de Douane ](./assets/develop-aem-projects/custom-project-card.png)

## Instellen

Deze zelfstudie doorloopt de code die nodig is om een aangepaste projectsjabloon te maken. U kunt het [ pakket in bijlage ](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip) downloaden en installeren aan een lokaal milieu om samen met het leerprogramma te volgen. U kunt tot het volledige Gemaakt project ook toegang hebben dat op [ wordt ontvangen GitHub ](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide).

* [Pakket met voltooide zelfstudies](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [ Volledige Bewaarplaats van de Code op GitHub ](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)

Dit leerprogramma veronderstelt sommige basiskennis van [ de ontwikkelingspraktijken van AEM ](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/introduction/the-basics) en wat vertrouwdheid met [ AEM Maven projectopstelling ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html). Alle vermelde code is bedoeld om als verwijzing worden gebruikt en zou slechts aan de instantie van AEM van de a [ lokale ontwikkeling ](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/deploying/deploying/deploy) moeten worden opgesteld.

## Structuur van een projectsjabloon

Projectsjablonen moeten onder broncontrole worden geplaatst en moeten onder de toepassingsmap onder /apps blijven. Ideaal gezien zouden zij in een subfolder met de noemende overeenkomst van **&#42;/projects/templates/** &lt;my-template> moeten worden geplaatst. Door na deze noemende overeenkomst te plaatsen zullen om het even welke nieuwe douanesjablonen automatisch beschikbaar worden aan auteurs wanneer het creëren van een project. De configuratie van beschikbare Malplaatjes van het Project wordt geplaatst bij: **/content/projects/jcr:content** knoop door **cq:allowedTemplates** bezit. Dit is standaard een reguliere expressie: **/(apps|libs)/.&#42;/projects/templates/.&#42;**

De wortelknoop van een Malplaatje van het Project zal a **jcr hebben:primaryType** van **cq:Malplaatje**. Onder de wortelknoop van er zijn drie knopen: **gadgets**, **rollen**, en **werkschema&#39;s**. Deze knopen zijn allen **niet:ongestructureerde**. Onder het hoofdknooppunt kan ook een bestand miniatuur.png staan dat wordt weergegeven wanneer u de sjabloon selecteert in de wizard Project maken.

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

De wortelknoop van het projectmalplaatje is van type **cq:Malplaatje**. Op deze knoop kunt u eigenschappen **jcr vormen:titel** en **jcr:beschrijving** die in de Create Tovenaar van het Project wordt getoond. Er is ook een bezit genoemd **tovenaar** dat aan een vorm richt die de Eigenschappen van het project zal bevolken. De standaardwaarde van: **/libs/cq/core/content/projects/wizard/steps/defaultproject.html** zou in de meeste gevallen fijn moeten werken, aangezien het de gebruiker toestaat om de basiseigenschappen van het Project te bevolken en groepsleden toe te voegen.

*&#42;Merk op de Create Tovenaar van het Project niet Sling POST servlet gebruikt. In plaats daarvan worden de waarden gepost aan een douane servlet:**com.adobe.cq.projects.impl.servlet.ProjectServlet**. Hiermee moet u rekening houden bij het toevoegen van aangepaste velden.*

Een voorbeeld van een douanetovenaar kan voor het Malplaatje van het Project van de Vertaling worden gevonden: **/libs/cq/core/content/projects/wizard/translationproject/default project**.

### Gadgets {#gadgets}

Er zijn geen extra eigenschappen op deze knoop maar de kinderen van de gadget knoopcontrole die de Tegels van het Project het dashboard van het Project bevolken wanneer een nieuw Project wordt gecreeerd. [ de Tegels van het Project ](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/authoring/projects/projects) (ook genoemd geworden gadgets of peul) zijn eenvoudige kaarten die de werkplaats van een Project bevolken. Een volledige lijst met voetbtegels vindt u onder: **/libs/cq/gui/components/projects/admin/pod. **Projecteigenaars kunnen altijd tegels toevoegen/verwijderen nadat een project is gemaakt.

### Rollen {#roles}

Er zijn drie [ standaardrollen ](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/authoring/projects/projects) voor elk project: **Waarnemers**, **Redacteurs**, en **Eigenaars**. Door kindknopen onder de rolknoop toe te voegen, kunt u extra zaken-specifieke Rollen van het Project voor het malplaatje toevoegen. U kunt deze rollen aan specifieke werkschema&#39;s dan verbinden verbonden aan het project.

### Workflows {#workflows}

Één de meest verleidelijke redenen voor het creëren van een Malplaatje van het douaneproject is dat het u de capaciteit geeft om de beschikbare werkschema&#39;s voor gebruik met het project te vormen. Dit kan OTB-workflows of aangepaste workflows zijn. Onder de **werkschema&#39;s** knoop moet er a **modellen** knoop (ook `nt:unstructured`) zijn en de kindknopen onder specificeren de beschikbare werkschemamodellen. Het bezit **modelId **richt aan het werkschemamodel onder /etc/workflow en de bezit **tovenaar** richt aan de dialoog die wanneer het beginnen van het werkschema wordt gebruikt. Een significant voordeel van Projecten is de capaciteit om een douanedialoog (tovenaar) toe te voegen om zaken-specifieke meta-gegevens bij het begin van het werkschema te vangen die verdere acties binnen het werkschema kunnen drijven.

```shell
<projects-template-root> (cq:Template)
    + workflows (nt:unstructured)
         + models (nt:unstructured)
              + <workflow-model> (nt:unstructured)
                   - modelId = points to the workflow model
                   - wizard = dialog used to start the workflow
```

## Een projectsjabloon maken {#creating-project-template}

Aangezien wij hoofdzakelijk knopen kopiëren/vormen, zullen wij CRXDE Lite gebruiken. In uw lokale instantie van AEM, open omhoog [ CRXDE Lite ](http://localhost:4502/crx/de/index.jsp).

1. Maak eerst een map onder `/apps/&lt;your-app-folder&gt;` genaamd `projects` . Maak een andere map onder de naam `templates` .

   ```shell
   /apps/aem-guides/projects-tasks/
                       + projects (nt:folder)
                                + templates (nt:folder)
   ```

1. Om dingen gemakkelijker te maken, zullen wij ons douanemalplaatje van het bestaande Eenvoudige malplaatje van het Project beginnen.

   1. Kopieer en kleef de knoop **/libs/cq/core/content/projects/templates/default** onder de *malplaatjes* omslag die in Stap 1 wordt gecreeerd.

   ```shell
   /apps/aem-guides/projects-tasks/
                + templates (nt:folder)
                     + default (cq:Template)
   ```

1. U zou nu een weg zoals **/apps/aem-gidsen/projecten-taken/projecten/malplaatjes/authoring-project** moeten hebben.

   1. Bewerk **jcr:titel** en **jcr:beschrijving** eigenschappen van de auteur-project knoop aan douanetitel en beschrijvingswaarden.

      1. Verlaat het **tovenaar** bezit richtend aan de eigenschappen van het standaardProject.

   ```shell
   /apps/aem-guides/projects-tasks/projects/
            + templates (nt:folder)
                 + authoring-project (cq:Template)
                      - jcr:title = "Authoring Project"
                      - jcr:description = "A project to manage approval and publish process for AEM Sites or Assets"
                      - wizard = "/libs/cq/core/content/projects/wizard/steps/defaultproject.html"
   ```

1. Voor dit projectmalplaatje willen wij gebruik maken van Taken.
   1. Voeg een nieuwe **niet toe:ongestructureerde** knoop onder authoring-project/gadgets geroepen **taken**.
   1. Voeg de eigenschappen van het Koord aan de takenknoop voor **cardWeight** toe = &quot;100&quot;, **jcr:title**= &quot;Taken&quot;, en **het sling:resourceType**= &quot;cq/gui/components/projects/admin/pod/taskpod&quot;.

   Nu zal de [ tegel van Taken ](https://experienceleague.adobe.com/en/docs) door gebrek tonen wanneer een nieuw project wordt gecreeerd.

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

   1. Onder het projectmalplaatje (creatie-project) knoop voegt een nieuwe **niet toe:ongestructureerde** knoop-geëtiketteerde **rollen**.
   1. Voeg een andere **niet toe:ongestructureerde** knoop geëtiketteerd goedkeurt als kind van de rolknoop.
   1. Voeg de eigenschappen van het Koord **jcr toe:titel** = &quot;**Approvers**&quot;, **roleclass** =&quot;**eigenaar**&quot;, **roleid**=&quot;**goedkeurt**&quot;.
      1. De naam van het knooppunt fiatteurs en jcr:title en roleid kunnen elke tekenreekswaarde zijn (zolang roleid uniek is).
      1. **roleclass** regeert de toestemmingen die voor die rol worden toegepast op de [ drie rollen OTB ](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/authoring/projects/projects) worden gebaseerd: **eigenaar**, **redacteur**, en **waarnemer**.
      1. In het algemeen als de douanerol meer van een beheerdersrol dan is kan de roleclass **eigenaar zijn;** als het een specifiekere auteursrol zoals Fotograaf of Designer dan **redacteursklasse** roleclass zou moeten volstaan. Het grote verschil tussen **eigenaar** en **redacteur** is dat de projecteigenaars de projecteigenschappen kunnen bijwerken en nieuwe gebruikers aan het project toevoegen.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
           + approvers (nt:unstructured)
                - jcr:title = "Approvers"
                - roleclass = "owner"
                - roleid = "approver"
   ```

1. Door het Eenvoudige malplaatje van het Project te kopiëren, zult u vier gevormde werkschema&#39;s OOTB krijgen. Elk knooppunt onder workflows/modellen verwijst naar een specifieke workflow en een wizard voor begindialoogvenster voor die workflow. Later in deze zelfstudie maken we een aangepaste workflow voor dit project. Verwijder voorlopig de knooppunten onder workflow(en):

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
            + models (nt:unstructured)
               - (remove ootb models)
   ```

1. Om het voor inhoudsauteurs gemakkelijk te maken om het Malplaatje van het Project te identificeren kunt u een douaneminiatuur toevoegen. De aanbevolen grootte is 319x319 pixels.
   1. In CRXDE Lite creeert een dossier als sibling van gadgets, rollen, en werkstroomknopen genoemd **thumbnail.png**.
   1. Sla het knooppunt op, navigeer naar het knooppunt `jcr:content` en dubbelklik op de eigenschap `jcr:data` (klik niet op &#39;view&#39;).
      1. Hiermee wordt u gevraagd een dialoogvenster voor het bewerken van `jcr:data` -bestanden te openen en kunt u een aangepaste miniatuur uploaden.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
       + thumbnail.png (nt:file)
   ```

Voltooide XML-representatie van het projectsjabloon:

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

Nu kunnen wij ons Malplaatje van het Project testen door een Project te creëren.

1. U zou het douanemalplaatje als één van de opties voor projectverwezenlijking moeten zien.

   ![ kies Malplaatje ](./assets/develop-aem-projects/choose-template.png)

1. Na het selecteren van het douanemalplaatje klik &quot;daarna&quot;en merk op dat wanneer het bevolken van de Leden van het Project, u hen als rol kunt toevoegen Approver.

   ![ goedkeuren ](./assets/develop-aem-projects/user-approver.png)

1. Klik op Maken om het maken van het project te voltooien op basis van de aangepaste sjabloon. U zult op het dashboard van het Project opmerken dat de Tegel van Taken en de andere tegels die onder gadgets worden gevormd automatisch verschijnen.

   ![ Taaktegel ](./assets/develop-aem-projects/tasks-tile.png)


## Waarom workflow?

Traditioneel hebben AEM-workflows die zich rond een goedkeuringsproces bevinden, gebruikgemaakt van workflowstappen voor deelnemers. AEM Inbox omvat details rond Taken en Werkschema en verbeterde integratie met de Projecten van AEM. Deze eigenschappen maken het gebruiken van de het processtappen van de Taak van Projecten creëren een aantrekkelijkere optie.

### Waarom taken?

Het gebruiken van een Stap van de Aanmaak van de Taak over de traditionele stappen van de Deelnemer biedt een paar voordelen aan:

* **Begin en Verleden Datum** - maakt het voor auteurs gemakkelijk om hun tijd te beheren, maakt de nieuwe eigenschap van de Kalender gebruik van deze data.
* **Prioriteit** - de ingebouwde prioriteiten van Laag, Normaal, en Hoog staan auteurs toe om aan het werk voorrang te geven
* **Verbonden Commentaren** - aangezien de auteurs aan een taak werken zij de capaciteit hebben om commentaren te verlaten die samenwerking verhogen
* **Zichtbaarheid** - de tegels en de meningen van de Taak met Projecten staan managers toe om te bekijken hoe de tijd wordt besteed
* **Integratie van het Project** - De taken zijn reeds geïntegreerd met de rollen en dashboards van het Project

Als de stappen van de Deelnemer, kunnen de Taken dynamisch worden toegewezen en worden verpletterd. Taakmetagegevens zoals Titel, Prioriteit kunnen ook dynamisch worden ingesteld op basis van eerdere acties, zoals in de volgende zelfstudie.

Terwijl de Taken sommige voordelen over de Stappen van de Deelnemer hebben zij extra overheadkosten dragen, en zijn niet zo nuttig buiten een Project. Bovendien moet al dynamisch gedrag van Taken worden gecodeerd gebruikend manuscripten ecma die zijn eigen beperkingen hebben.

## Voorschriften voor het gebruik van hoofdletters en kleine letters {#goals-tutorial}

![ diagram van het het procesproces van het Werkschema ](./assets/develop-aem-projects/workflow-process-diagram.png)

In het bovenstaande diagram worden de vereisten op hoog niveau voor onze workflow voor monstergoedkeuring beschreven.

De eerste stap bestaat uit het maken van een taak om het bewerken van een stuk inhoud te voltooien. De aanvrager van de workflow kan de eerste taak kiezen.

Zodra de eerste taak volledig is zal toegewezen drie opties hebben om het werkschema te verpletteren:

**Normaal ** - het normale verpletteren leidt tot een taak die aan de groep van de fiatteur van het Project wordt toegewezen om te herzien en goed te keuren. De prioriteit van de taak is Normaal en de vervaldatum is vijf dagen vanaf het tijdstip waarop deze wordt gecreëerd.

**kruis** - het snel verpletteren leidt ook tot een taak die aan de groep van de Fiatteur van het Project wordt toegewezen. Prioriteit van de taak is Hoog en de vervaldatum is slechts één dag.

**Omzeiling** - in dit steekproefwerkschema heeft de aanvankelijke deelnemer de optie om de goedkeuringsgroep te mijden. (ja dit zou het doel van een &quot;Goedkeuring&quot;werkschema kunnen verslaan maar het staat ons toe om extra verpletterende mogelijkheden te illustreren)

De Fiatteur-groep kan de inhoud goedkeuren of deze terugsturen naar de eerste toegewezen persoon voor herbewerking. Als u een nieuwe taak wilt terugsturen, maakt u deze opnieuw en geeft u het desbetreffende label &#39;Naar achteren sturen voor opnieuw samenstellen&#39; op.

In de laatste stap van de workflow wordt gebruikgemaakt van de processtap Pagina/element activeren en wordt de lading gerepliceerd.

## Het workflowmodel maken

1. Navigeer in het menu Start van AEM naar Extra -> Workflow -> Modellen. Klik op &#39;Maken&#39; in de rechterbovenhoek om een workflowmodel te maken.

   Geef het nieuwe model een titel: &quot;Workflow voor inhoudsgoedkeuring&quot; en een URL-naam: &quot;workflow voor inhoudsgoedkeuring&quot;.

   ![ Werkschema creeert dialoog ](./assets/develop-aem-projects/workflow-create-dialog.png)

   [ voor meer informatie met betrekking tot het creëren van werkschema&#39;s, lees hier ](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-models).

1. Aangepaste workflows kunt u het beste groeperen in een eigen map onder /etc/workflow/modellen. In CRXDE Lite, creeer a **&quot;niet:omslag&quot;** onder /etc/workflow/modellen genoemd **&quot;aem-gidsen&quot;**. Als u een submap toevoegt, zorgt u ervoor dat aangepaste workflows niet per ongeluk worden overschreven tijdens upgrades of Service Pack-installaties.

   &#42; Nota het is belangrijk om nooit de omslag of douanewerkschema&#39;s onder wortelsubfolders zoals /etc/workflow/models/dam of /etc/workflow/models/projecten te plaatsen aangezien volledige subfolder ook door verbeteringen of de dienstpakken kan worden beschreven.

   ![ Plaats van werkschemamodel in 6.3 ](./assets/develop-aem-projects/custom-workflow-subfolder.png)

   Locatie van het workflowmodel in 6.3

   >[!NOTE]
   >
   >Als u AEM 6.4+ gebruikt, is de locatie van Workflow gewijzigd. Zie [ hier voor meer details.](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-best-practices)

   Als u AEM 6.4+ gebruikt, wordt het workflowmodel gemaakt onder `/conf/global/settings/workflow/models` . Herhaal bovenstaande stappen met de map /conf en voeg een submap met de naam `aem-guides` toe en verplaats de map `content-approval-workflow` eronder.

   ![ Moderne plaats van de werkschemadefinitie ](./assets/develop-aem-projects/modern-workflow-definition-location.png)
Locatie van workflowmodel in 6.4+

1. De introductie in AEM 6.3 is de mogelijkheid werkstroomfasen toe te voegen aan een bepaalde werkstroom. De fasen worden aan de gebruiker weergegeven vanuit het Postvak In op het tabblad Workflowinfo. Het toont de gebruiker het huidige werkgebied in het werkschema evenals de stadia voorafgaand aan en na het.

   Als u de fasen wilt configureren, opent u het dialoogvenster Pagina-eigenschappen vanuit de Sidekick. Het vierde tabblad heet &quot;Stages&quot;. Voeg de volgende waarden toe om de drie fasen van deze workflow te configureren:

   1. Inhoud bewerken
   1. Goedkeuring
   1. Publiceren

   ![ configuratie van werkschemaframes ](./assets/develop-aem-projects/workflow-model-stage-properties.png)

   Configureer de werkstroomfasen in het dialoogvenster Pagina-eigenschappen.

   ![ bar van de werkschemavooruitgang ](./assets/develop-aem-projects/workflow-info-progress.png)

   De voortgangsbalk van de workflow zoals deze wordt weergegeven in het AEM Inbox.

   Naar keuze kunt u een **Beeld** aan de Eigenschappen van de Pagina uploaden die als duimnagel van het Werkschema wordt gebruikt wanneer de gebruikers het selecteren. Afbeeldingsafmetingen moeten 319x319 pixels zijn. Het toevoegen van a **Beschrijving** aan de Eigenschappen van de Pagina zal ook verschijnen wanneer een gebruiker het werkschema gaat selecteren.

1. Het workflowproces Projecttaak maken is ontworpen om een taak te maken als een stap in de workflow. Pas na het voltooien van de taak gaat de workflow verder. Een krachtig aspect van de stap van de Taak van het Project creëren is dat het werkschema meta-gegevens waarden kan lezen en die gebruiken om de taak dynamisch tot stand te brengen.

   Verwijder eerst de Stap van de Deelnemer die standaard wordt gemaakt. Van Sidekick in het componentenmenu breidt **uit &quot;Projecten&quot;** subrubriek en sleep + laat vallen **&quot;de Taak van het Project van het Project&quot;creëren** op het model.

   Dubbelklik op de stap Projecttaak maken om het workflowdialoogvenster te openen. Configureer de volgende eigenschappen:

   Dit tabblad wordt veel gebruikt voor alle stappen in het workflowproces en we stellen de Titel en Beschrijving in (deze zijn niet zichtbaar voor de eindgebruiker). Het belangrijke bezit dat wij zullen plaatsen is het Stadium van het Werkschema aan **&quot;geeft Inhoud&quot;** van het drop-down menu uit.

   ```shell
   Common Tab
   -----------------
       Title = "Start Task Creation"
       Description = "This the first task in the Workflow"
       Workflow Stage = "Edit Content"
   ```

   Het workflowproces Projecttaak maken is ontworpen om een taak te maken als een stap in de workflow. Met het tabblad Taak kunnen we alle waarden van de taak instellen. In ons geval willen we dat de ontvanger dynamisch is, dus laten we hem leeg. De overige eigenschapswaarden:

   ```shell
   Task Tab
   -----------------
       Name* = "Edit Content"
       Task Priority = "Medium"
       Description = "Edit the content and finalize for approval. Once finished submit for approval."
       Due In - Days = "2"
   ```

   Het verpletterende lusje is een facultatieve dialoog die beschikbare acties voor de gebruiker kan specificeren die de taak voltooit. Deze handelingen zijn alleen tekenreekswaarden en worden opgeslagen in de metagegevens van de workflow. Deze waarden kunnen door manuscripten en/of processtappen later in het werkschema worden gelezen om de werkschema dynamisch &quot;te leiden&quot;. Op basis van de workflowdoelstellingen die we gaan uitvoeren, voegt u drie acties toe aan dit tabblad:

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

1. In de vorige stap hebben we verwezen naar een Pre-Create Taakscript. Wij zullen dat manuscript nu tot stand brengen waarin wij de Ontvanger van de Taak zullen plaatsen die op de waarde van een waarde van werkschemagegevens &quot;**wordt gebaseerd toegewezen**&quot;. De **&quot;toegewezen&quot;** waarde wordt geplaatst wanneer het werkschema weg wordt geschopt. Wij zullen ook de werkschemameta-gegevens lezen om de prioriteit van de taak dynamisch te kiezen door &quot;**taskPriority&quot;te lezen** waarde van de meta-gegevens van het werkschema evenals **&quot;taskdueDate&quot; **aan dynamisch te plaatsen wanneer de eerste taak wordt vereist.

   Voor organisatorische doeleinden hebben we onder onze toepassingsmap een map gemaakt voor al onze projectgerelateerde scripts: **/apps/aem-guides/projects-tasks/projects/scripts** . Creeer een dossier onder deze omslag genoemd **&quot;start-task-config.ecma&quot;**. &#42; Nota zorgt ervoor de weg aan uw begin-taak-config.ecma- dossier de weg aanpast die op het Geavanceerde Lusje van Montages in Stap 4 wordt geplaatst.

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

1. Navigeer terug naar de workflow voor goedkeuring van inhoud. Sleep+Daling **OF Splitste** component (die in Sidekick onder de &quot;categorie van het Werkschema&quot;wordt gevonden) onder de **3} Stap van de Taak van het Begin {.** Selecteer in het dialoogvenster Algemeen het keuzerondje voor 3 vertakkingen. OR Splitst zal de waarde van werkschemameta-gegevens **&quot;lastTaskAction&quot;** lezen om de route van het werkschema te bepalen. Het **&quot;lastTaskAction&quot;** bezit wordt geplaatst aan één van de waarden van het Verpletterende Lusje dat in Stap 4 wordt gevormd. Voor elk van de lusjes van de Tak vult het **1} de tekstgebied van het Manuscript {met de volgende waarden uit:**

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

   &#42; Nota wij doen een directe gelijke van het Koord om de route te bepalen zodat is het belangrijk dat de waarden die in de manuscripten van de Tak worden geplaatst de waarden van de Route aanpassen die in Stap 4 worden geplaatst.

1. Sleep+Daling een andere &quot;**creeer de Taak van het Project**&quot;stap op het model aan uiterst links (Tak 1) onder OF spleet. Vul het dialoogvenster met de volgende eigenschappen in:

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

   Aangezien dit de Normale route van de Goedkeuring is wordt de prioriteit van de taak geplaatst aan Medium. Daarnaast geven we de groep fiatteurs vijf dagen om de taak te voltooien. De toegewezen persoon wordt leeg gelaten op het tabblad Taak aangezien wij dit dynamisch zullen toewijzen op het tabblad Geavanceerde instellingen. Wij geven de Approvers groep twee mogelijke routes wanneer het voltooien van deze taak: **&quot;keur goed en publiceer&quot;** als zij de inhoud goedkeuren en het kan worden gepubliceerd en **&quot;verzend terug voor Herziening&quot;** als er kwesties zijn die de originele redacteur moet verbeteren. De fiatteur kan commentaren verlaten die de originele redacteur zal zien of is het werkschema teruggekeerd aan hem/haar.

Eerder in deze zelfstudie hebben we een projectsjabloon gemaakt dat een rol van fiatteurs bevatte. Telkens als een nieuw Project van dit Malplaatje wordt gecreeerd wordt een project-specifieke Groep gecreeerd voor de rol Approvers. Enkel als een Stap van de Deelnemer kan een Taak slechts aan een Gebruiker of een Groep worden toegewezen. We willen deze taak toewijzen aan de projectgroep die overeenkomt met de groep fiatteurs. Alle werkschema&#39;s die van binnen een Project worden gelanceerd zullen meta-gegevens hebben die de Rollen van het Project aan de specifieke groep van het Project in kaart brengen.

Kopieer+plak de volgende code in het **1} de tekstgebied van het Manuscript {van de **Geavanceerde Montages **tab.** Deze code zal de werkschemameta-gegevens lezen en zal de taak aan de groep Approvers van het Project toewijzen. Als het niet de waarde van de fiatversgroep kan vinden zal het terug vallen om de taak aan de groep van Beheerders toe te wijzen.

```
var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");

task.setCurrentAssignee(projectApproverGrp);
```

1. Sleep+Daling een andere &quot;**creeer de Taak van het Project**&quot;stap op het model aan de middentak (Tak 2) onder OF spleet. Vul het dialoogvenster met de volgende eigenschappen in:

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

   Wij kunnen het zelfde manuscriptfragment zoals in Stap 7 opnieuw gebruiken om het **manuscript** tekst-gebied op het** Geavanceerde Montages **tab te bevolken. Kopiëren en de onderstaande code plakken:

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

   ![ werkschemamodel OF spleet ](./assets/develop-aem-projects/workflow-stage-after-orsplit.png)

   Het model van het Werkschema zou als dit moeten kijken nadat alle drie takken in OF verdeeld zijn gevormd.

1. Aangezien de Approvers groep de optie heeft om het werkschema terug naar de originele redacteur voor verdere revisies te verzenden zullen wij op de **Goto** stap vertrouwen om de laatste genomen actie te lezen en het werkschema aan het begin te leiden of het te laten verdergaan.

   Sleep en zet de component Goto Step (gevonden in de Sidekick onder Workflow) onder OF splitst op de plaats waar deze opnieuw wordt samengevoegd. Dubbelklik op de volgende eigenschappen en configureer deze in het dialoogvenster:

   ```
   Common Tab
   ----------------
       Title = "Goto Step"
       Description = "Based on the Approver groups action route the workflow to the beginning or continue and publish the payload."
   
   Process Tab
   ---------------
       The step to go to. = "Start Task Creation"
   ```

   Het laatste stuk dat wij zullen vormen is het Manuscript als deel van Goto processtap. De scriptwaarde kan worden ingesloten via het dialoogvenster of worden geconfigureerd om te wijzen naar een extern bestand. Goto Manuscript moet a **functiecontrole () bevatten** en waar terugkeren als het werkschema naar de gespecificeerde stap zou moeten gaan. Als er false worden geretourneerd, gaat de workflow verder.

   Als de goedkeurende groep **&quot;achteruit voor Herziening&quot;** actie (gevormd in Stap 7 en 8) kiest dan willen wij het werkschema aan de **&quot;Taakverwezenlijking van het Begin&quot;terugkeren** stap.

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

1. Om te publiceren zullen wij de nuttige lading gebruiken de weg **pagina/activa** stap van het Proces activeren. Deze processtap vereist weinig configuratie en zal de nuttige lading van het werkschema aan de replicatierij voor activering toevoegen. Wij zullen de stap onder de stap van de Goto toevoegen en op deze manier kan het slechts worden bereikt als de groep Approver de inhoud voor publicatie heeft goedgekeurd of de originele redacteur de route van de Goedkeuring van de Bypass koos.

   Sleep+Daling **activeer de stap van het het Proces van de Pagina/van Activa** (die in Sidekick onder Werkschema WCM) onder de Stap van het Goto in het model wordt gevonden.

   ![ volledig werkschemamodel ](assets/develop-aem-projects/workflow-model-final.png)

   Hoe moet het workflowmodel eruitzien nadat u de stap Ga naar hebt toegevoegd en de stap Pagina/element activeren hebt geactiveerd.

1. Als de Approver groep de inhoud voor herziening terugstuurt willen wij de originele redacteur het weten. Dit kunnen we bereiken door de eigenschappen voor het maken van taken dynamisch te wijzigen. Wij zullen van de lastActionTake bezitswaarde van **&quot;achteruit voor Herziening&quot;verzenden**. Als die waarde aanwezig is, zullen wij de titel en de beschrijving wijzigen om erop te wijzen dat deze taak het resultaat van inhoud is die voor herziening wordt teruggestuurd. Wij zullen ook de prioriteit aan **&quot;Hoog&quot;** bijwerken zodat het het eerste punt is de redacteur aan werkt. Tot slot zullen wij de taak vervaldatum aan één dag plaatsen vanaf wanneer het werkschema voor revisie werd teruggestuurd.

   Vervang het start `start-task-config.ecma` -script (gemaakt in Stap 5) door de volgende code:

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

## De wizard &quot;Startworkflow&quot; maken {#start-workflow-wizard}

Wanneer u een workflow uit een project verwijdert, moet u een wizard opgeven om de workflow te starten. Met de standaardwizard `/libs/cq/core/content/projects/workflowwizards/default_workflow` kan de gebruiker een workflowtitel, een startopmerking en een payload-pad invoeren voor de workflow die moet worden uitgevoerd. Er zijn ook verschillende andere voorbeelden te vinden onder: `/libs/cq/core/content/projects/workflowwizards`.

Het maken van een aangepaste wizard kan zeer krachtig zijn, omdat u essentiële informatie kunt verzamelen voordat de workflow start. De gegevens worden opgeslagen als onderdeel van de metagegevens van de werkstroom en werkstroomprocessen kunnen dit lezen en het gedrag dynamisch wijzigen op basis van de ingevoerde waarden. We maken een aangepaste wizard die de eerste taak dynamisch op basis van een beginwaarde van de wizard toewijst.

1. In CRXDE-Lite zullen wij een subomslag onder `/apps/aem-guides/projects-tasks/projects` omslag creëren genoemd &quot;tovenaars&quot;. Kopieer de standaardtovenaar van: `/libs/cq/core/content/projects/workflowwizards/default_workflow` onder de pas gecreëerde tovenaarsomslag en noem het aan **tevreden-goedkeuring-begin** anders. Het volledige pad moet nu zijn: `/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start` .

   De standaardwizard is een wizard met twee kolommen en de eerste kolom bevat Titel, Beschrijving en Miniatuur van het workflowmodel geselecteerd. De tweede kolom bevat velden voor de titel van de workflow, Opmerking starten en Pad loonbelasting. De tovenaar is een standaard vorm van de Aanraking UI en maakt gebruik van de standaard [ componenten van de Vorm van de Vorm UI van Granite ](https://experienceleague.adobe.com/en/docs) om de gebieden te bevolken.

   ![ tovenaar van het het werkschema van de inhoudsgoedkeuring ](./assets/develop-aem-projects/content-approval-start-wizard.png)

1. Wij zullen een extra gebied aan de tovenaar toevoegen dat wordt gebruikt om de ontvanger van de eerste taak in het werkschema (zie [ tot het Model van het Werkschema ](#create-workflow-model) leiden: Stap 5) te plaatsen.

   Beneath `../content-approval-start/jcr:content/items/column2/items` creeer een nieuwe knoop van type `nt:unstructured` genoemd **&quot;wijs&quot;** toe. Wij zullen de component van de Plukker van de Gebruiker van Projecten gebruiken (die uit de [ Component van de Plukker van de Gebruiker van de Graniet ](https://experienceleague.adobe.com/en/docs)) wordt gebaseerd. Met dit formulierveld kunt u eenvoudig de selectie van gebruikers en groepen beperken tot gebruikers die tot het huidige project behoren.

   Hieronder is de vertegenwoordiging van XML van **toewijzen** knoop:

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

1. Wij zullen ook een prioritair selectiegebied toevoegen dat de prioriteit van de eerste taak in het werkschema zal bepalen (zie [ tot het Model van het Werkschema ](#create-workflow-model) leiden: Stap 5).

   Onder `/content-approval-start/jcr:content/items/column2/items` creeer een nieuwe knoop van type `nt:unstructured` genoemd **prioriteit**. Wij zullen de [ Uitgezochte component van UI van de Graniet ](https://experienceleague.adobe.com/en/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions) gebruiken om het vormgebied te bevolken.

   Onder de **prioritaire** knoop zullen wij een **punten** knoop van **toevoegen niet:ongestructureerde**. Onder de **punten** knoop voeg 3 meer knopen toe om de selectieopties voor Hoog, Medium, en Laag te bevolken. Elke knoop is van type **niet:ongestructureerde** en zou a **tekst** en **waarde** bezit moeten hebben. Zowel de tekst als de waarde moeten dezelfde waarde hebben:

   1. Hoog
   1. Medium
   1. Laag

   Voor de knoop van Medium voeg een extra genoemd bezit Van Boole toe &quot;**geselecteerd&quot;** met een waarde die aan **waar** wordt geplaatst. Zo weet u zeker dat Medium de standaardwaarde in het selectieveld is.

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

1. De aanvrager van de workflow kan de vervaldatum van de eerste taak instellen. Wij zullen het [ granite UI DatePicker ](https://experienceleague.adobe.com/en/docs) vormgebied gebruiken om deze input te vangen. Wij zullen ook een verborgen gebied met a [ TypeHint ](https://sling.apache.org/documentation/bundles/manipulating-content-the-slingpostservlet-servlets-post.html#typehint) toevoegen om ervoor te zorgen dat de input als het typebezit van de Datum in JCR wordt opgeslagen.

   Voeg twee **niet toe:ongestructureerde** knopen met de volgende eigenschappen die hieronder in XML worden vertegenwoordigd:

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

1. U kunt de volledige code voor de dialoog van de begintovenaar [ hier ](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/projects-tasks-guide/ui.apps/src/main/content/jcr_root/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start/.content.xml) bekijken.

## De workflow en de projectsjabloon verbinden {#connecting-workflow-project}

Het laatste wat we moeten doen, is ervoor zorgen dat het workflowmodel beschikbaar is om van binnen een van de Projecten te worden afgevoerd. Om dit te doen, moeten wij het Malplaatje van het Project opnieuw bezoeken wij in Deel 1 van deze reeks creeerden.

De configuratie van het Werkschema is een gebied van een Malplaatje van het Project dat de beschikbare werkschema&#39;s specificeert die met dat project moeten worden gebruikt. De configuratie is ook verantwoordelijk voor het specificeren van de Tovenaar van het Werkschema van het Begin wanneer het schoppen van het werkschema (dat wij in de [ vorige stappen) ](#start-workflow-wizard) creeerden. De configuratie van het Werkschema van een Malplaatje van het Project is &quot;levend&quot;betekenend dat het bijwerken van de werkschemaconfiguratie nieuwe gecreeerde Projecten evenals bestaande Projecten zal uitvoeren die het malplaatje gebruiken.

1. Navigeer in CRXDE-Lite naar de ontwerpprojectsjabloon die eerder in `/apps/aem-guides/projects-tasks/projects/templates/authoring-project/workflows/models` is gemaakt.

   Onder de modelknoop voeg een nieuwe knoop genoemd **toe inhoudsgoedkeuring** met een knooptype van **niet:ongestructureerde**. Voeg de volgende eigenschappen toe aan het knooppunt:

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/etc/workflow/models/aem-guides/content-approval-workflow/jcr:content/model"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

   >[!NOTE]
   >
   >Als u AEM 6.4 gebruikt, is de locatie van Workflow gewijzigd. Wijs de eigenschap `modelId` onder `/var/workflow/models/aem-guides/content-approval-workflow` toe aan de locatie van het workflowmodel van de runtime.
   >
   >
   >Zie [ hier voor meer details over de verandering in plaats van werkschema.](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-best-practices)

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/var/workflow/models/aem-guides/content-approval-workflow"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

1. Zodra het werkschema van de Goedkeuring van de Inhoud aan het Malplaatje van het Project is toegevoegd zou het beschikbaar moeten zijn om van de Tegel van het Werkschema van het project af te schoppen. Ga door en start en speel rond met de verschillende routines die we hebben gecreëerd.

## Ondersteunende materialen

* [Voltooid lespakket downloaden](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [ Volledige Bewaarplaats van de Code op GitHub ](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)
* [ de Documentatie van de Projecten van AEM ](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/authoring/projects/projects)
