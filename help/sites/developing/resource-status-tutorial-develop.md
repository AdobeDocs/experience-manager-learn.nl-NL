---
title: Bronstatussen ontwikkelen in AEM Sites
description: De status API's van de Adobe Experience Manager Resource Status, zijn een pluggable raamwerk voor het toegankelijk maken van statusberichten in AEM verschillende redacteursWeb UIs.
topics: development
audience: developer
doc-type: tutorial
activity: develop
version: 6.4, 6.5
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---


# Bronstatussen ontwikkelen {#developing-resource-statuses-in-aem-sites}

De status API&#39;s van de Adobe Experience Manager Resource Status, zijn een pluggable raamwerk voor het toegankelijk maken van statusberichten in AEM verschillende redacteursWeb UIs.

## Overzicht {#overview}

Het kader van de Status van het Middel voor Editors verstrekt server-kant en cliënt-kant APIs voor het tonen van en het in wisselwerking staan met redacteurstatussen, op een standaard en eenvormige manier.

De statusbalken van de editor zijn standaard beschikbaar in de editors Pagina, Ervaar fragment en Sjabloon van AEM.

Voorbeelden van gebruiksgevallen voor aangepaste bronnenstatusproviders zijn:

* Auteurs op de hoogte stellen wanneer een pagina binnen 2 uur na de geplande activering valt
* Auteurs ervan op de hoogte stellen dat een pagina in de afgelopen 15 minuten is geactiveerd
* Auteurs ervan in kennis stellen dat een pagina in de laatste 5 minuten is bewerkt en door wie

![Overzicht van de bronstatus van AEM editor](assets/sample-editor-resource-status-screenshot.png)

## Resource Status Provider Framework {#resource-status-provider-framework}

Wanneer het ontwikkelen van de Statussen van het douaneMiddel, wordt het ontwikkelingswerk samengesteld uit:

1. De implementatie ResourceStatusProvider, die voor het bepalen of een status wordt vereist, en de basisinformatie over de status verantwoordelijk is: titel, bericht, prioriteit, variant, pictogram en beschikbare acties.
2. Alternatief, GraniteUI JavaScript die de functionaliteit van om het even welke beschikbare acties uitvoert.

   ![bronstatusarchitectuur](assets/sample-editor-resource-status-application-architecture.png)

3. De statusbron die als onderdeel van de editors Pagina, Ervingsfragment en Sjabloon wordt opgegeven, krijgt een type via de bronnen &quot;[!DNL statusType]&quot; eigenschap.

   * Pagina-editor: `editor`
   * Experience Fragment Editor: `editor`
   * Sjablooneditor: `template-editor`

4. De statusbron `statusType` komt overeen met geregistreerd `CompositeStatusType` OSGi geconfigureerd `name` eigenschap.

   Voor alle overeenkomsten `CompositeStatusType's` de typen worden verzameld en gebruikt om de `ResourceStatusProvider` implementaties van dit type, via `ResourceStatusProvider.getType()`.

5. De overeenkomst `ResourceStatusProvider` wordt doorgegeven aan `resource` in de redacteur, en bepaalt als `resource` heeft de status die moet worden weergegeven. Als de status vereist is, is deze implementatie verantwoordelijk voor het bouwen van 0 of veel `ResourceStatuses` om terug te keren, elk die een status aan vertoning vertegenwoordigen.

   Doorgaans wordt een `ResourceStatusProvider` retourneert 0 of 1 `ResourceStatus` per `resource`.

6. ResourceStatus is een interface die door de klant kan worden uitgevoerd, of nuttig `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` kan worden gebruikt om een status samen te stellen. Een status bestaat uit:

   * Titel
   * Bericht
   * Pictogram
   * Variant
   * Prioriteit
   * Handelingen
   * Gegevens

7. Optioneel, indien `Actions` worden `ResourceStatus` -object, zijn ondersteunende clientlibs vereist om functionaliteit te binden aan de actiekoppelingen in de statusbalk.

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. Elke ondersteunende JavaScript of CSS ter ondersteuning van de acties moet via de respectievelijke clientbibliotheken van elke editor worden uitgebreid om ervoor te zorgen dat de front-end code beschikbaar is in de editor.

   * Categorie Paginaeditor: `cq.authoring.editor.sites.page`
   * Categorie Experience Fragment Editor: `cq.authoring.editor.sites.page`
   * Categorie Sjablooneditor: `cq.authoring.editor.sites.template`

## De code weergeven {#view-the-code}

[Zie code op GitHub](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## Aanvullende bronnen {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)
