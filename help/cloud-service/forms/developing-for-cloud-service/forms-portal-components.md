---
title: AEM Forms Portal-componenten inschakelen
description: Een AEM Forms Portal maken met behulp van kerncomponenten
solution: Experience Manager
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Core Components
kt: 10373
exl-id: ab01573a-e95f-4041-8ccf-16046d723aba
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 0%

---

# Forms Portal-componenten

AEM Forms biedt de volgende poortcomponenten uit de verpakking:

**Zoeken en registreren**: Met deze component kunt u formulieren weergeven vanuit de formulieropslagplaats naar uw portalpagina en configuratieopties voor het weergeven van formulieren op basis van opgegeven criteria.

**Concepten en verzendingen**: In het onderdeel Zoeken en opslaan worden formulieren weergegeven die door de auteur van Forms openbaar zijn gemaakt, maar in het onderdeel Concepten en verzendingen worden formulieren weergegeven die zijn opgeslagen als concept voor het later invullen van formulieren en die zijn verzonden. Deze component verstrekt gepersonaliseerde ervaring aan om het even welke het programma geopende gebruiker.

**Koppeling**: Met deze component kunt u overal op de pagina een koppeling naar een formulier maken.

## Forms Portal-componenten inschakelen

Start IntelliJ en open het BankingApplication-project dat in het dialoogvenster [eerder.](./getting-started.md) De ui.apps->src->main->content->jcr_root->apps.bankingapplication->componenten uitbreiden

Als u een kerncomponent (inclusief de onderdelen van de out-of-the-box portal) wilt gebruiken op een Adobe Experience Manager-site (AEM), moet u een proxycomponent maken en deze inschakelen voor uw site.
De nieuwe proxycomponent moet naar de uitpunt van de box form component verwijzen, zodat deze alles van de component overerft. Dit wordt gedaan door resourceSuperType in content.xml van de volmachtscomponent te veranderen. In content.xml geven we ook de titel en de componentgroep op.
>[!NOTE]
>
> U kunt het resource super type voor elk van [deze componenten van hier](https://github.com/adobe/aem-core-forms-components/tree/master/ui.apps/src/main/content/jcr_root/apps/core/fd/components/formsportal)


### Concepten en verzendingen

Kopie maken van een bestaande component (bijvoorbeeld `button`) en noemt het als _ontwerpdocumenten en opmerkingen_.
![ontwerpdocumenten en opmerkingen](assets/forms-portal-components2.png)
De inhoud in het dialoogvenster `.content.xml` met de volgende XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Drafts And Submissions"
          sling:resourceSuperType="core/fd/components/formsportal/draftsandsubmissions/v1/draftsandsubmissions"
          componentGroup="BankingApplication - Content"/>
```

### Zoeken en registreren

Een kopie van de knopcomponent maken en de naam ervan wijzigen in _zoekmachine_.
De inhoud in het dialoogvenster `.content.xml` met de volgende XML:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Search And Lister"
          sling:resourceSuperType="core/fd/components/formsportal/searchlister/v1/searchlister"
          componentGroup="BankingApplication - Content"/>
```

### Component koppelen

Een kopie van de knopcomponent maken en de naam ervan wijzigen in _link_.
De inhoud in het dialoogvenster `.content.xml` met de volgende XML:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Link to Adaptive Form"
          sling:resourceSuperType="core/fd/components/formsportal/link/v2/link"
          componentGroup="BankingApplication - Content"/>
```

Zodra uw project wordt opgesteld zou u deze componenten in uw AEM pagina moeten kunnen gebruiken om het portaal van Forms tot stand te brengen.

## Volgende stappen

[Inclusief configuratie van cloudservices](./azure-storage-fdm.md)
