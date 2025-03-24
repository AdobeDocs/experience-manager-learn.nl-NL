---
title: Zelfstudie om door de gebruiker opgegeven metagegevenstags toe te voegen
description: Aangepaste verzending maken om de formuliergegevens op te slaan met metagegevenstags in Azure
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-14501
duration: 43
exl-id: eb9bcd1b-c86f-4894-bcf8-9c17e74dd6ec
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '115'
ht-degree: 0%

---

# Blob-indextags toevoegen

Aangezien datasets groter worden, kan het vinden van een specifiek voorwerp in een zee van gegevens moeilijk zijn. Blob-indextags bieden mogelijkheden voor gegevensbeheer en detectie met behulp van tagkenmerken voor sleutelwaarden-indextags. U kunt objecten in één container of in alle containers in uw opslagaccount categoriseren en zoeken. Bijvoorbeeld, blob indexmarkering _**CustomerType=Platinum**_, waar Platinum de waarde van het gebied CustomerType is.

![ index-markeringen ](assets/blob-with-index-tags1.png)
Met de volgende code wordt de tekenreeks met de labels voor blob-indexgegevens gemaakt met de bijbehorende waarden uit de verzonden gegevens

```java
@Override
    public String getMetaDataTags(String submittedFormName,String formPath,Session session,String formData) {

        JsonObject jsonObject = JsonParser.parseString(formData).getAsJsonObject();
        List<String>metaDataTags = new ArrayList<String>();
        metaDataTags.add("formName="+submittedFormName);
        Map< String, String > map = new HashMap< String, String >();
        map.put("path", formPath);
        map.put("1_property", "Searchable");
        map.put("1_property.value","true");
        Query query = queryBuilder.createQuery(PredicateGroup.create(map),session);
        query.setStart(0);
        query.setHitsPerPage(20);
        SearchResult result = query.getResult();
        logger.debug("Get result hits " + result.getHits().size());
        for (Hit hit: result.getHits()) {
            try {
                    logger.debug(hit.getPath());
                    String jsonElementName = (String) hit.getProperties().get("name");
                    String fieldName = hit.getProperties().get("name").toString();
                    if(jsonObject.get(jsonElementName).isJsonArray())
                    {
                        
                        JsonArray arrayOfValues = jsonObject.get(jsonElementName).getAsJsonArray();
                        StringBuilder valuesString = new StringBuilder();
                        for(int j=0;j<arrayOfValues.size();j++)
                        {
                            valuesString.append(arrayOfValues.get(j).getAsString());
                            if(j < arrayOfValues.size() -1)
                            {
                                valuesString.append(" and ");
                            }
                        }

                        
                        metaDataTags.add(fieldName + "=" + valuesString.toString());

                    }
                    else
                    {
                        logger.debug("The searchable field name is " + fieldName + "the json element name is " + jsonElementName);
                        metaDataTags.add(fieldName + "=" + jsonObject.get(jsonElementName).getAsString());
                    }

            } catch (RepositoryException e) {
                throw new RuntimeException(e);
            }
        }
        return String.join("&",metaDataTags);
    }
```

## Volgende stappen

[Aangepaste verzendhandler maken](./create-custom-submit.md)
