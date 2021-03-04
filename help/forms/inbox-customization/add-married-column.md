---
title: Aanpassing van Postvak IN
description: Aangepaste kolommen toevoegen om aanvullende gegevens over de workflow weer te geven
feature: Adaptieve Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
topic: Ontwikkeling
role: Developer
level: Ervaren
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 1%

---


# Aangepaste kolommen toevoegen

Om werkschemagegevens in inbox te tonen, moeten wij variabelen in het werkschema bepalen en bevolken. De waarde van de variabele moet worden ingesteld voordat een taak aan een gebruiker wordt toegewezen. Om u een hoofdstart te geven, hebben we een voorbeeldworkflow geleverd die klaar is om op uw AEM server te worden geïmplementeerd.

* [Aanmelden bij AEM](http://localhost:4502/crx/de/index.jsp)
* [De revisiewerkstroom importeren](assets/review-workflow.zip)
* [De workflow controleren](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

Deze workflow heeft twee gedefinieerde variabelen (isMarried en inkomen) en de waarden ervan worden ingesteld met behulp van de variabele-component set. Deze variabelen worden beschikbaar gesteld als kolommen die aan AEM inbox moeten worden toegevoegd

## Service maken

Voor elke kolom die wij in onze inbox moeten tonen zouden wij de dienst moeten schrijven. Met de volgende service kunnen we een kolom toevoegen om de waarde van de variabele isMarried weer te geven

```java
import com.adobe.cq.inbox.ui.column.Column;
import com.adobe.cq.inbox.ui.column.provider.ColumnProvider;

import com.adobe.cq.inbox.ui.InboxItem;
import org.osgi.service.component.annotations.Component;

import java.util.Map;

/**
 * This provider does not require any sightly template to be defined.
 * It is used to display the value of 'ismarried' workflow variable as a column in inbox
 */
@Component(service = ColumnProvider.class, immediate = true)
public class MaritalStatusProvider implements ColumnProvider {@Override
public Column getColumn() {
return new Column("married", "Married", Boolean.class.getName());
}

// Return True or False if 'ismarried' is set. Else returns null
private Boolean isMarried(InboxItem inboxItem) {
Boolean ismarried = null;

Map metaDataMap = inboxItem.getWorkflowMetadata();
if (metaDataMap != null) {
if (metaDataMap.containsKey("isMarried")) {
    ismarried = (Boolean) metaDataMap.get("isMarried");
}
}

return ismarried;
}

@Override
public Object getValue(InboxItem inboxItem) {
return isMarried(inboxItem);
}
}
```

>[!NOTE]
>
>U moet AEM 6.5.5 Uber.jar in uw project voor de bovengenoemde code omvatten om te werken

![uber-jar](assets/uber-jar.PNG)

## Testen op uw server

* [Aanmelden bij AEM webconsole](http://localhost:4502/system/console/bundles)
* [Aanpassingsbundel inbox implementeren en starten](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [De inbox openen](http://localhost:4502/aem/inbox)
* Open Beheer door op _Lijstweergave_ te klikken naast de knop _Maken_
* Gehuwde kolom toevoegen aan Postvak IN en uw wijzigingen opslaan
* [Naar interface FormsAndDocuments gaan](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Importeer de voorbeeldindeling ](assets/snap-form.zip) door  _Bestand_ uploaden te selecteren in  __ Createmenu
* [Geef een voorbeeld van het formulier weer](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Selecteer de _huwelijksstatus_ en verzend het formulier
   [inbox weergeven](http://localhost:4502/aem/inbox)

Als u het formulier verzendt, wordt de workflow geactiveerd en wordt een taak toegewezen aan de gebruiker van de &quot;beheerder&quot;. U zou een waarde onder de Getrouwde kolom zoals aangetoond in dit het schermschot moeten zien

![gehuwde kolom](assets/married-column.PNG)
