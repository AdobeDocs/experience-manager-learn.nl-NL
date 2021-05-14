---
title: Service-interface maken
description: Definieer de methoden in de interface die u toegankelijk wilt maken
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: ontwikkeling
thumbnail: 7825.jpg
kt: 7825
source-git-commit: 84499d5a7c8adac87196f08c6328e8cb428c0130
workflow-type: tm+mt
source-wordcount: '24'
ht-degree: 0%

---

# Interface

Creeer een interface met de volgende 2 methodedefinities.

```java
package com.aemforms.doccloud.core;

import java.io.InputStream;

import com.adobe.aemfd.docmanager.Document;

public interface DocumentCloudSDKService {
	
	public Document getPDF(String location,String accessToken,String fileName);
	public Document createPDFFromInputStream(InputStream is,String fileName);

}


}
```
