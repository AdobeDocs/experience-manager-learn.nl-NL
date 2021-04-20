---
title: Clientbibliotheken maken
description: Clientbibliotheek maken om de klikgebeurtenis van de knop "Opslaan en afsluiten" af te handelen
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 1%

---

# Clientbibliotheek maken

Maak [client lib](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html) die de code bevat om de methode `doAjaxSubmitWithFileAttachment` van de `guideBridge`-API aan te roepen op de klikgebeurtenis van de knop die wordt aangeduid door de CSS-klasse **savebutton**.  We geven de adaptieve formuliergegevens `fileMap` en `mobileNumber` door aan het eindpunt dat luistert op `**/bin/storeafdatawithattachments`

Nadat de formuliergegevens zijn opgeslagen, wordt een unieke toepassings-id gegenereerd en in een dialoogvenster aan de gebruiker getoond. Als de gebruiker het dialoogvenster sluit, wordt hij naar het formulier geleid waarmee hij het opgeslagen adaptieve formulier kan ophalen met de unieke toepassings-id.

```java
$(document).ready(function () {
  
  $(".savebutton").click(function () {
    var tel = guideBridge.resolveNode(
      "guide[0].guide1[0].guideRootPanel[0].contactInformation[0].basicContact[0].telephoneNumber[0]"
    );
    var telephoneNumber = tel.value;
    guideBridge.getFormDataString({
      success: function (data) {
        var map = guideBridge._getFileAttachmentMapForSubmit();
        guideBridge.doAjaxSubmitWithFileAttachment(
          "/bin/storeafdatawithattachments",
          {
            data: data.data,
            fileMap: map,
            mobileNumber: telephoneNumber,
          },
          {
            success: function (x) {
              bootbox.alert(
                "This is your reference number.<br>" +
                  x.data.path +
                  " <br>You will need this to retrieve your application",
                function () {
                  console.log(
                    "This was logged in the callback! After the ok button was pressed"
                  );
                  window.location.href =
                    "http://localhost:4502/content/dam/formsanddocuments/myaccountform/jcr:content?wcmmode=disabled";
                }
              );
              console.log(x.data.path);
            },
          },
          guideBridge._getFileAttachmentsList()
        );
      },
    });
  });
});
```

>[!NOTE]
> We hebben [bootbox javascript library](http://bootboxjs.com/examples.html) gebruikt om het dialoogvenster weer te geven

De clientbibliotheken die in dit voorbeeld worden gebruikt, kunnen [hier worden gedownload](assets/client-libraries.zip)
