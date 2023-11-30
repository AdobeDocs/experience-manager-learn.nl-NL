---
title: Clientbibliotheken maken
description: Clientbibliotheek maken voor het afhandelen van de klikgebeurtenis "Opslaan en afsluiten"
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6597
thumbnail: 6597.pg
topic: Development
role: Developer
level: Intermediate
exl-id: c90eea73-bd44-40af-aa98-d766aa572415
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# Clientbibliotheek maken

Maken [client lib](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html) die de code bevat om de methode aan te roepen `doAjaxSubmitWithFileAttachment` van de `guideBridge` API op de klikgebeurtenis van de knoop die door de CSS klasse wordt geïdentificeerd **knop Opslaan**.  We geven de adaptieve formuliergegevens door, `fileMap`en de `mobileNumber` aan het eindpunt luisterend bij `**/bin/storeafdatawithattachments`

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
                  x.data.applicationID +
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
> We hebben [box JavaScript-bibliotheek](https://bootboxjs.com/examples.html) naar dialoogvenster

De in dit voorbeeld gebruikte clientbibliotheken kunnen [hier gedownload.](assets/store-af-with-attachments-client-lib.zip)

## Volgende stappen

[Verifieer gebruikers met de dienst OTP](./verify-users-with-otp.md)
