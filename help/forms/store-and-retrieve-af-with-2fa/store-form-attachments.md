---
title: Formulierbijlagen opslaan
description: Pak de formulierbijlagen uit en sla deze op een nieuwe locatie op in de CRX-opslagplaats.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6537
thumbnail: 6537.jpg
topic: Development
role: Developer
level: Experienced
exl-id: ec50b9b1-e28c-4d84-ae90-6a21c9700688
duration: 65
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 0%

---

# Formulierbijlagen opslaan

Wanneer u bijlagen toevoegt aan een adaptief formulier, worden de bijlagen opgeslagen op een tijdelijke locatie in de CRX-opslagplaats. Als u het gebruik van hoofdletters en kleine letters wilt gebruiken, moeten we de formulierbijlagen opslaan op een nieuwe locatie in de CRX-opslagplaats.

De dienst OSGi wordt gecreeerd om de vormgehechtheid in een nieuwe plaats in de bewaarplaats van CRX op te slaan. Er wordt een nieuwe bestandstoewijzing gemaakt met de nieuwe locatie van de bijlagen in de CRX en geretourneerd naar de aanroepende toepassing.
Hier volgt de FileMap die naar de servlet wordt verzonden. De sleutel is het adaptieve formulierveld en de waarde is de tijdelijke locatie van de bijlage. In onze servlet zullen wij de gehechtheid halen en het opslaan in nieuwe plaats in de AEM bewaarplaats en het FileMap met de nieuwe plaats bijwerken

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "idcard/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "tableItem11/BankStatement-Sept-2020.pdf"
}
```

Hier volgt de code die de bijlagen uit de aanvraag extraheert en deze in de map **/content/bijattachments** opslaat.

```java
public String storeAFAttachments(JSONObject fileMap, SlingHttpServletRequest request) {
    JSONObject newFileMap = new JSONObject();
    try {
        Iterator keys = fileMap.keys();
        log.debug("The file map is " + fileMap.toString());
        while (keys.hasNext()) {
            String key = (String) keys.next();
            log.debug("#### The key is " + key);
            String attacmenPath = (String) fileMap.get((key));
            log.debug("The attachment path is " + attacmenPath);
            if (!attacmenPath.contains("/content/afattachments")) {
                String fileName = attacmenPath.split("/")[1];
                log.debug("#### The attachment name  is " + fileName);
                InputStream is = request.getPart(attacmenPath).getInputStream();
                Document aemFDDocument = new Document(is);
                String crxPath = saveDocumentInCrx("/content/afattachments", fileName, aemFDDocument);
                log.debug(" ##### written to crx repository  " + attacmenPath.split("/")[1]);
                newFileMap.put(key, crxPath);
            } else {
                log.debug("$$$$ The attachment was already added " + key);
                log.debug("$$$$ The attachment path is " + attacmenPath);
                int position = attacmenPath.indexOf("//");
                log.debug("$$$$ After substring " + attacmenPath.substring(position + 1));
                log.debug("$$$$ After splitting " + attacmenPath.split("/")[1]);
                newFileMap.put(key, attacmenPath.substring(position + 1));
            }
        }
    } catch (Exception ex) {
        log.debug(ex.getMessage());
    }
    return newFileMap.toString();

}


}
```

Dit is de nieuwe FileMap met de bijgewerkte locatie van de formulierbijlagen

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "/content/afattachments/7dc0cbde-404d-49a9-9f7b-9ab5ee7482be/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "/content/afattachments/81653de9-4967-4736-9ca3-807a11542243/BankStatement-Sept-2020.pdf"
}
```

## Volgende stappen

[De formuliergegevens opslaan](./store-form-data.md)