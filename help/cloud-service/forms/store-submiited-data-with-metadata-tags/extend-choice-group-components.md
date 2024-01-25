---
title: Zelfstudie om door de gebruiker opgegeven metagegevenstags toe te voegen
description: Leer hoe u adaptieve formuliergegevens kunt opslaan en ophalen van Azure-opslagaccount.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14501
duration: 41
exl-id: 94454327-86d9-468e-9f08-50b8a9c530f3
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '106'
ht-degree: 0%

---

# Groepcomponenten van keuzen uitbreiden

De componenten checkboxGroup, dropdown en radioButtonCore zijn uitgebreid om een Extra lusje van Eigenschappen te omvatten. Het tabblad Extra-eigenschappen bevat een selectievakje om aan te geven of het veld moet worden gebruikt als tabblad voor een blob-index
![aanvullende eigenschappen](assets/drop-down-additonal-properties.png). Wanneer het selectievakje is ingeschakeld, wordt een eigenschap met de naam Searchable gemaakt en wordt de waarde ervan ingesteld op true in de jcr-opslagplaats, zoals wordt weergegeven in de volgende schermafbeelding
![doorzoekbaar](assets/searchable-true.png).

Het volgende .content.xml is gemaakt in de map _cq_dialog.

![drop-down-project-view](assets/drop-down-project-view.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
          jcr:primaryType="nt:unstructured"
          jcr:title="Check box group"
          sling:resourceType="cq/gui/components/authoring/dialog">
    <content
            jcr:primaryType="nt:unstructured"
            sling:resourceType="granite/ui/components/coral/foundation/container">
        <items jcr:primaryType="nt:unstructured">
            <tabs
                    jcr:primaryType="nt:unstructured"
                    sling:resourceType="granite/ui/components/coral/foundation/tabs"
                    maximized="{Boolean}false">
                <items jcr:primaryType="nt:unstructured">

                    <properties
                            jcr:primaryType="nt:unstructured"
                            jcr:title="Additional Properties"
                            sling:resourceType="granite/ui/components/coral/foundation/container"
                            margin="{Boolean}true">
                        <items jcr:primaryType="nt:unstructured">
                            <columns
                                    jcr:primaryType="nt:unstructured"
                                    sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns"
                                    margin="{Boolean}true">
                                <items jcr:primaryType="nt:unstructured">
                                    <column
                                            jcr:primaryType="nt:unstructured"
                                            sling:resourceType="granite/ui/components/coral/foundation/container">
                                        <items jcr:primaryType="nt:unstructured">
                                            <Searchable
                                                    jcr:primaryType="nt:unstructured"
                                                    sling:resourceType="granite/ui/components/coral/foundation/form/checkbox"
                                                    emptyText="Want to include in search?"
                                                    fieldDescription="Indicate if you want to use in search"
                                                    text="Want to use this field in query"
                                                    value="{Boolean}true"
                                                    uncheckedValue="{Boolean}false"

                                                    name="./Searchable"
                                                    checked="{Boolean}false"
                                                    required="{Boolean}false"/>


                                        </items>
                                    </column>
                                </items>
                            </columns>
                        </items>
                    </properties>
                </items>
            </tabs>
        </items>
    </content>
</jcr:root>
```

## Volgende stappen

[Azure Portal-configuratie maken](./create-osgi-configuration.md)
