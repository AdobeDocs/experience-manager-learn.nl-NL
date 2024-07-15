---
title: AEM Forms Portal-componenten inschakelen
description: Een AEM Forms Portal maken met behulp van kerncomponenten
solution: Experience Manager
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Core Components
jira: KT-10373
exl-id: ab01573a-e95f-4041-8ccf-16046d723aba
duration: 78
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 0%

---

# Forms Portal-componenten

AEM Forms biedt de volgende poortcomponenten uit de verpakking:

**Onderzoek &amp; van het Registreertoestel**: Deze component staat u toe om vormen van de vormenbewaarplaats op uw portalpagina een lijst te maken en verstrekt configuratieopties om van vormen een lijst te maken die op gespecificeerde criteria worden gebaseerd.

**Concepten &amp; Verzendingen**: Terwijl de component van het Onderzoek &amp; van het Registreertoestel vormen toont die door de auteur van Forms openbaar worden gemaakt, toont de componenten Concepten &amp; van Verzending vormen die als ontwerp voor de voltooiing later en ingediende vormen worden bewaard. Deze component verstrekt gepersonaliseerde ervaring aan om het even welke aangemelde gebruiker.

**Verbinding**: Deze component staat u toe om een verbinding aan een vorm overal op de pagina tot stand te brengen.

## Forms Portal-componenten inschakelen

Start IntelliJ en open het BankingApplication-project dat in de [ vroegere stap is gemaakt.](./getting-started.md) De ui.apps->src->main->content->jcr_root->apps.bankingapplication->componenten uitbreiden

Als u een kerncomponent (inclusief de onderdelen van de out-of-the-box portal) wilt gebruiken op een Adobe Experience Manager-site (AEM), moet u een proxycomponent maken en deze inschakelen voor uw site.
De nieuwe proxycomponent moet naar de uitpunt van de box form component verwijzen, zodat deze alles van de component overerft. Dit wordt gedaan door resourceSuperType in content.xml van de volmachtscomponent te veranderen. In content.xml geven we ook de titel en de componentgroep op.
>[!NOTE]
>
> U kunt het middel super type voor elk van [ deze componenten van hier construeren ](https://github.com/adobe/aem-core-forms-components/tree/master/ui.apps/src/main/content/jcr_root/apps/core/fd/components/formsportal)


### Concepten en verzendingen

Maak een exemplaar van een bestaande component (bijvoorbeeld `button`) en noem het als _concepten en voorlegging_.
![ concepten en voorlegging ](assets/forms-portal-components2.png)
Vervang de inhoud in de `.content.xml` door de volgende XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Drafts And Submissions"
          sling:resourceSuperType="core/fd/components/formsportal/draftsandsubmissions/v1/draftsandsubmissions"
          componentGroup="BankingApplication - Content"/>
```

### Zoeken en registreren

Maak een exemplaar van knoopcomponent en noem het aan _onderzoek andlister_ anders.
Vervang de inhoud in de `.content.xml` door de volgende XML:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Search And Lister"
          sling:resourceSuperType="core/fd/components/formsportal/searchlister/v1/searchlister"
          componentGroup="BankingApplication - Content"/>
```

### Component koppelen

Maak een exemplaar van knoopcomponent en noem het aan _verbinding_ anders.
Vervang de inhoud in de `.content.xml` door de volgende XML:


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
