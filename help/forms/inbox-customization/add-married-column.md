---
title: Aangepaste kolommen toevoegen
description: Aangepaste kolommen toevoegen om aanvullende gegevens over de workflow weer te geven
feature: Adaptive Forms
doc-type: article
version: Experience Manager 6.5
jira: KT-5830
topic: Development
role: Developer
level: Experienced
exl-id: 0b141b37-6041-4f87-bd50-dade8c0fee7d
duration: 75
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---

# Aangepaste kolommen toevoegen

Om werkschemagegevens in inbox te tonen, moeten wij variabelen in het werkschema bepalen en bevolken. De waarde van de variabele moet worden ingesteld voordat een taak aan een gebruiker wordt toegewezen. We hebben u een voorbeeldworkflow geboden die klaar is om te worden geïmplementeerd op uw AEM-server.

* [&#x200B; Login aan AEM &#x200B;](http://localhost:4502/crx/de/index.jsp)
* [De revisiewerkstroom importeren](assets/review-workflow.zip)
* [&#x200B; Herzie het werkschema &#x200B;](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

Deze workflow heeft twee gedefinieerde variabelen (isMarried en inkomen) en de waarden ervan worden ingesteld met behulp van de variabele-component set. Deze variabelen worden beschikbaar gesteld als kolommen die moeten worden toegevoegd aan AEM inbox

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
>De bovenstaande code werkt alleen als u AEM 6.5.5 Uber.jar in uw project opneemt

![&#x200B; uber-jar &#x200B;](assets/uber-jar.PNG)

## Testen op uw server

* [&#x200B; Login aan de Webconsole van AEM &#x200B;](http://localhost:4502/system/console/bundles)
* [Aanpassingsbundel inbox implementeren en starten](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [&#x200B; Open uw inbox &#x200B;](http://localhost:4502/aem/inbox)
* Open Controle Admin door _pictogram van de Mening van de Lijst_ naast _te klikken creeert_ knoop
* Gehuwde kolom toevoegen aan Postvak IN en uw wijzigingen opslaan
* [&#x200B; ga naar FormsAndDocuments UI &#x200B;](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [&#x200B; de Invoer de steekproefvorm &#x200B;](assets/snap-form.zip) door _Dossier te selecteren uploadt_ van _creeert_ menu
* [&#x200B; Voorproef de vorm &#x200B;](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Selecteer de _burgerlijke status_ en leg de vorm voor
  [&#x200B; mening inbox &#x200B;](http://localhost:4502/aem/inbox)

Als u het formulier verzendt, wordt de workflow geactiveerd en wordt een taak toegewezen aan de gebruiker van de &quot;beheerder&quot;. U zou een waarde onder de Getrouwde kolom zoals aangetoond in dit het schermschot moeten zien

![&#x200B; getrouwd-kolom &#x200B;](assets/married-column.PNG)

## Volgende stappen

[Gehuwde kolom weergeven](./use-sightly-template.md)
