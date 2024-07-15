---
title: Extensies voor inhoudsfragmenten AEM
description: Leer hoe u AEM as a Cloud Service Content Fragment-extensies kunt maken en implementeren
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
jira: KT-11603
thumbnail: KT-11603.png
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 9164423b-a609-4bc5-9777-112d229ae748
duration: 126
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '773'
ht-degree: 0%

---

# Uitbreidbaarheid AEM inhoudfragmenten

AEM de UI van de Fragmenten van de Inhoud is een krachtige verlengbare UI voor het beheren van het creëren van, het beheren van, en het uitgeven van de Fragmenten van de Inhoud. Er zijn verschillende extensiepunten beschikbaar waarmee u de interface naar wens kunt aanpassen. Verschillende extensiepunten zijn beschikbaar op basis van de interface die u uitbreidt.

## Extensiepunten van de console van inhoudsfragmenten

De Content Fragment Console in AEM (Adobe Experience Manager) is een gebruikersinterface die een gecentraliseerde locatie biedt voor het beheren en ordenen van inhoudsfragmenten. Het biedt een uitgebreide reeks hulpmiddelen en eigenschappen aan om inhoudsfragmenten tot stand te brengen, uit te geven, te publiceren en te volgen, die gebruikers machtigen om gestructureerde inhoud over diverse kanalen en touchpoints efficiënt te beheren.

![ de Console van Fragmenten van de Inhoud ](./assets/overview/cfc.png)

[ AEM de Console van de Fragmenten van de Inhoud ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html) is verlengbare UI voor het lijst van en het beheren van inhoudsfragmenten. [ AEM de uitbreidingen van de Console van het Fragment van de Inhoud worden gecreeerd ](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation) gebruikend het `@adobe/aem-cf-admin-ui-ext-tpl` malplaatje van App Builder.

De volgende extensiepunten voor de Content Fragments Console zijn beschikbaar:

<div class="columns is-multiline">
      <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Action bar">
        <div class="card" style="height: 100%">
          <div class="card-image">
            <figure class="image is-16by9">
              <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" title="Actiebalk" tabindex="-1" target="_blank" rel="referrer">
                <img class="is-bordered-r-small" src="./assets/overview/cfc-action-bar.png" alt="Actiebalk">
              </a>
            </figure>
          </div>
          <div class="card-content is-padded-small">
            <div class="content">
              <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" title="Actiebalk" target="_blank" rel="referrer">Actiebalk</a></p>
              <p class="is-size-6">Pas handelingen aan wanneer een of meer inhoudsfragmenten zijn geselecteerd.</p>
              <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Mening de documenten </span>
              </a>
            </div>
          </div>
        </div>
      </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Grid columns">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" title="Rasterkolommen" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfc-grid-columns.png" alt="Rasterkolommen">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" title="Rasterkolommen" target="_blank" rel="referrer">Rasterkolommen</a></p>
          <p class="is-size-6">Pas de gegevens aan die in de lijst Inhoudsfragmenten worden weergegeven.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Mening de documenten </span>
          </a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" title="Menu Koptekst" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfc-header-menu.png" alt="Menu Koptekst">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" title="Menu Koptekst" target="_blank" rel="referrer">Menu Koptekst</a></p>
          <p class="is-size-6">Pas acties aan voor wanneer er geen inhoudsfragmenten zijn geselecteerd.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Mening de documenten </span>
          </a>
        </div>
      </div>
    </div>
  </div>  
</div>

## Extensiepunten van de Inhoudsfragmenteditor

De Content Fragment Editor in AEM (Adobe Experience Manager) is een gebruikersinterfacecomponent waarmee gebruikers inhoudsfragmenten kunnen maken, bewerken en beheren. Het biedt een visueel intuïtieve en gebruiksvriendelijke omgeving voor het werken met gestructureerde inhoud, waarmee gebruikers inhoudselementen kunnen definiëren en indelen, sjablonen kunnen toepassen, variaties kunnen beheren en een voorvertoning kunnen weergeven van de inhoud die op verschillende kanalen wordt weergegeven. De Inhoudsfragmenteditor stroomlijnt het proces van het maken van herbruikbare en modulaire inhoud die eenvoudig kan worden gedistribueerd en gepubliceerd over meerdere digitale ervaringen.

![ de Redacteur van Fragmenten van de Inhoud ](./assets/overview/cfe.png)

AEM de Inhoudsfragmenteditor is de uitbreidbare UI voor het bewerken van inhoudsfragmenten. [ AEM de uitbreidingen van de Redacteur van het Fragment van de Inhoud worden gecreeerd ](https://developer.adobe.com/uix/docs/services/aem-cf-editor/code-generation/) gebruikend het `@adobe/aem-cf-editor-ui-ext-tpl` malplaatje van App Builder.

De volgende extensiepunten van de Content Fragments Editor zijn beschikbaar:

<div class="columns is-multiline">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
      <div class="card" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu" title="Menu Koptekst" tabindex="-1" target="_blank" rel="referrer">
              <img class="is-bordered-r-small" src="./assets/overview/cfe-header-menu.png" alt="Menu Koptekst">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu/" title="Menu Koptekst" target="_blank" rel="referrer">Menu Koptekst</a></p>
            <p class="is-size-6">Pas handelingen aan in het koptekstmenu van de Content Fragment Editor.</p>
            <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
              <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Mening de documenten </span>
            </a>
          </div>
        </div>
      </div>
    </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor toolbar">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" title="Rich Text Editor, werkbalk" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-toolbar.png" alt="Rich Text Editor, werkbalk">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" title="Rich Text Editor, werkbalk"  target="_blank" rel="referrer">Rich Text Editor, werkbalk</a></p>
          <p class="is-size-6">Voeg een aangepaste knop toe aan de Rich Text Editor (RTE) van de Content Fragment Editor.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Mening de documenten </span>
          </a>
        </div>
      </div>
    </div>
  </div>

<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor widgets">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" title="Widgets van Rich Text Editor" tabindex="-1"  target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-widgets.png" alt="Widgets van Rich Text Editor">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" title="Widgets van Rich Text Editor" target="_blank" rel="referrer">Widgets van Rich Text Editor</a></p>
          <p class="is-size-6">Pas acties in RTE aan die aan aanslagen worden gebonden.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Mening de documenten </span>
          </a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor badges">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/" title="Badges Rich Text Editor" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-badges.png" alt="Badges Rich Text Editor">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/ " title="Badges Rich Text Editor" target="_blank" rel="referrer">Badges Rich Text Editor</a></p>
          <p class="is-size-6">Pas niet-bewerkbare gestileerde blokken aan in RTE.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Mening de documenten </span>
          </a>
        </div>
      </div>
    </div>
  </div>
</div>

## Voorbeelden van extensies

Welkom bij een verzameling AEM voorbeelden van UI-uitbreidingscodes! Deze bron is ontworpen om u praktische demonstraties en inzichten te bieden bij het uitbreiden van de Adobe Experience Manager-gebruikersinterface (AEM). Of u nu een ontwikkelaar bent die de functionaliteit van AEM wil verbeteren, deze codevoorbeelden dienen als waardevolle referentie.

<div class="columns is-multiline">
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/console-bulk-property-update.md" title="Bulkeigenschap bijwerken" tabindex="-1">
            <img class="is-bordered-r-small" src="./assets/../examples/assets/bulk-property-update/card.png" alt="Bulkeigenschap bijwerken">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/console-bulk-property-update.md" title="Bulkeigenschap bijwerken">Bulk Content Fragment property update</a></p>
          <p class="is-size-6">Een uitbreiding van de actiebalk van de inhoudsfragmentconsole met modale en Adobe I/O Runtime-actie.</p>
          <a href="./examples/console-bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Mening het voorbeeld </span>
          </a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="OpenAI-based image generation and upload to AEM extension">
        <div class="card" style="height: 100%">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./examples/console-image-generation-and-image-upload.md" title="Op OpenAI gebaseerde afbeelding genereren en uploaden naar AEM extensie" tabindex="-1">
                        <img class="is-bordered-r-small" src="./examples/assets/digital-image-generation/card.png" alt="Op OpenAI gebaseerde afbeelding genereren en uploaden naar AEM extensie">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/console-image-generation-and-image-upload.md" title="Op OpenAI gebaseerde afbeelding genereren en uploaden naar AEM extensie">OpenAPI-afbeelding genereren</a></p>
                    <p class="is-size-6">Onderzoek een uitbreiding van de voorbeeldactie die een beeld gebruikend OpenAI produceert, uploadt het aan AEM en werkt beeldbezit op het geselecteerde Inhoudsfragment bij.</p>
                    <a href="./examples/console-image-generation-and-image-upload.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Mening het voorbeeld </span>
                    </a>
                </div>
            </div>
        </div>
    </div>    
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Custom columns">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/custom-grid-columns.md" title="Aangepaste kolommen" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/custom-grid-columns/card.png" alt="Aangepaste kolommen">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/custom-grid-columns.md" title="Aangepaste kolommen">Aangepaste kolommen</a></p>
          <p class="is-size-6">Voeg een aangepaste kolom toe aan de Content Fragment Console.</p>
          <a href="./examples/custom-grid-columns.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Mening het voorbeeld </span>
          </a>
        </div>
      </div>
    </div>
  </div>    
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Export to XML">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-export-to-xml.md" title="Exporteren naar XML" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/export-to-xml/card.png" alt="Exporteren naar XML">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-export-to-xml.md" title="Exporteren naar XML">Exporteren naar XML</a></p>
          <p class="is-size-6">Exporteer een inhoudsfragment als XML uit de Inhoudsfragmenteditor.</p>
          <a href="./examples/editor-export-to-xml.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Mening het voorbeeld </span>
          </a>
        </div>
      </div>
    </div>
  </div>    
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor toolbar button">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-toolbar.md" title="Rich Text Editor, werkbalkknop" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-toolbar-card.png" alt="Rich Text Editor, werkbalkknop">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-toolbar.md" title="Rich Text Editor, werkbalkknop">Rich Text Editor, werkbalkknop</a></p>
          <p class="is-size-6">Voeg aangepaste werkbalkknoppen toe aan RTE-velden in de Inhoudsfragmenteditor.</p>
          <a href="./examples/editor-rte-toolbar.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Mening het voorbeeld </span>
          </a>
        </div>
      </div>
    </div>
  </div>   
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor Widget">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-widget.md" title="Widget Rich Text Editor" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-widget-card.png" alt="Widget Rich Text Editor">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-toolbar.md" title="Widget Rich Text Editor">Widget Rich Text Editor</a></p>
          <p class="is-size-6">Voeg widgets toe aan de Rich Text Editor in de Content Fragment Editor.</p>
          <a href="./examples/editor-rte-widget.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Mening het voorbeeld </span>
          </a>
        </div>
      </div>
    </div>
  </div>   
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor Badge">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-badges.md" title="Badge Rich Text Editor" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-badge-card.png" alt="Badge Rich Text Editor">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-badges.md" title="Badge Rich Text Editor">Badge Rich Text Editor</a></p>
          <p class="is-size-6">Voeg badges toe aan de Rich Text Editor in de Content Fragment Editor.</p>
          <a href="./examples/editor-rte-badges.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Mening het voorbeeld </span>
          </a>
        </div>
      </div>
    </div>
  </div>

<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Custom fields">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-custom-field.md" title="Aangepaste velden" tabindex="-1">
            <img class="is-bordered-r-small" src="https://video.tv.adobe.com/v/3427585?format=jpeg" alt="Aangepaste velden">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-custom-field.md" title="Aangepaste velden">Aangepaste velden</a></p>
          <p class="is-size-6">Maak aangepaste velden voor inhoudsfragmenten.</p>
          <a href="./examples/editor-custom-field.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Mening het voorbeeld </span>
          </a>
        </div>
      </div>
    </div>
  </div> 
</div>
