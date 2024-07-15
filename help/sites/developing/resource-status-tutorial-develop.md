---
title: Bronstatussen ontwikkelen in AEM Sites
description: De status API's van de Adobe Experience Manager Resource Status, zijn een pluggable raamwerk voor het toegankelijk maken van statusberichten in AEM verschillende redacteursWeb UIs.
doc-type: Tutorial
version: 6.4, 6.5
duration: 88
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 0%

---


# Bronstatussen ontwikkelen {#developing-resource-statuses-in-aem-sites}

De status API&#39;s van de Adobe Experience Manager Resource Status, zijn een pluggable raamwerk voor het toegankelijk maken van statusberichten in AEM verschillende redacteursWeb UIs.

## Overzicht {#overview}

Het kader van de Status van het Middel voor Editors verstrekt server-kant en cliënt-kant APIs voor het tonen van en het in wisselwerking staan met redacteurstatussen, op een standaard en eenvormige manier.

De statusbalken van de editor zijn standaard beschikbaar in de editors Pagina, Ervaar fragment en Sjabloon van AEM.

Voorbeelden van gebruiksgevallen voor aangepaste Resource Status Providers zijn:

* Auteurs op de hoogte stellen wanneer een pagina binnen 2 uur na de geplande activering valt
* Auteurs ervan op de hoogte stellen dat een pagina in de afgelopen 15 minuten is geactiveerd
* Auteurs ervan in kennis stellen dat een pagina in de laatste 5 minuten is bewerkt en door wie

![ overzicht van de het middelstatus van AEM redacteur ](assets/sample-editor-resource-status-screenshot.png)

## Resource Status Provider Framework {#resource-status-provider-framework}

Wanneer het ontwikkelen van de Statussen van het douaneMiddel, wordt het ontwikkelingswerk samengesteld uit:

1. De implementatie ResourceStatusProvider, die voor het bepalen of een status wordt vereist, en de basisinformatie over de status verantwoordelijk is: titel, bericht, prioriteit, variant, pictogram, en beschikbare acties.
2. Naar keuze, JavaScript GraniteUI die de functionaliteit van om het even welke beschikbare acties uitvoert.

   ![ architectuur van de middelstatus ](assets/sample-editor-resource-status-application-architecture.png)

3. Het statusmiddel dat als deel van de redacteurs van de Pagina, van het Fragment van de Ervaring en van het Malplaatje wordt verstrekt wordt een type via het bezit &quot;[!DNL statusType]&quot;van middelen gegeven.

   * Pagina-editor: `editor`
   * Experience Fragment Editor: `editor`
   * Sjablooneditor: `template-editor`

4. De `statusType` van de statusbron komt overeen met de geregistreerde eigenschap `CompositeStatusType` OSGi configured `name` .

   Voor alle overeenkomsten worden de `CompositeStatusType's` -typen verzameld en gebruikt om de `ResourceStatusProvider` -implementaties van dit type via `ResourceStatusProvider.getType()` te verzamelen.

5. De overeenkomende `ResourceStatusProvider` wordt doorgegeven aan de `resource` in de editor en bepaalt of de `resource` de status heeft die moet worden weergegeven. Als de status vereist is, is deze implementatie verantwoordelijk voor het bouwen van 0 of veel `ResourceStatuses` die moeten worden geretourneerd, elk met een status die moet worden weergegeven.

   Een `ResourceStatusProvider` retourneert doorgaans 0 of 1 `ResourceStatus` per `resource` .

6. ResourceStatus is een interface die door de klant kan worden uitgevoerd, of nuttig `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` kan worden gebruikt om een status te construeren. Een status bestaat uit:

   * Titel
   * Bericht
   * Pictogram
   * Variant
   * Prioriteit
   * Handelingen
   * Gegevens

7. Als `Actions` wordt opgegeven voor het `ResourceStatus` -object, is ondersteuning voor clientlibs vereist om functionaliteit te binden aan de handelingskoppelingen in de statusbalk.

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. Elke ondersteunende JavaScript of CSS die de acties ondersteunt, moet worden uitgebreid via de respectievelijke clientbibliotheken van elke editor om ervoor te zorgen dat de front-end code beschikbaar is in de editor.

   * Categorie in paginaeditor: `cq.authoring.editor.sites.page`
   * Experience Fragment Editor category: `cq.authoring.editor.sites.page`
   * Sjabloonbewerkingscategorie: `cq.authoring.editor.sites.template`

## De code weergeven {#view-the-code}

[ zie code op GitHub ](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## Aanvullende bronnen {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)
