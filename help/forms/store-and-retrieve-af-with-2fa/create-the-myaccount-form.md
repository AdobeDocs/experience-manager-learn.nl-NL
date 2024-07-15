---
title: Het MyAccountForm maken
description: Maak het mijnaccountformulier om het gedeeltelijk ingevulde formulier op te halen na geslaagde verificatie van de toepassings-id en het telefoonnummer.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6599
thumbnail: 6599.jpg
topic: Development
role: User
level: Beginner
exl-id: 1ecd8bc0-068f-4557-bce4-85347c295ce0
duration: 53
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# Het MyAccountForm maken

De vorm **MyAccountForm** wordt gebruikt om de gedeeltelijk voltooide adaptieve vorm terug te winnen nadat de gebruiker toepassings identiteitskaart en het mobiele aantal verbonden aan toepassings identiteitskaart heeft geverifieerd.

![ mijn rekeningsvorm ](assets/6599.JPG)

Wanneer de gebruiker toepassingsidentiteitskaart ingaat en de **knoop FetchApplication** klikt, wordt het mobiele aantal verbonden aan toepassingsidentiteitskaart opgehaald van het gegevensbestand gebruikend de Get verrichting van het model van vormgegevens.

In dit formulier wordt gebruikgemaakt van de aanroep van de POST van het formuliergegevensmodel om het mobiele nummer te verifiëren met OTP. De verzendactie van het formulier wordt geactiveerd nadat het mobiele nummer met de volgende code is geverifieerd. Wij teweegbrengen de klikgebeurtenis van voorlegt genoemde knoop **submitForm**.

>[!NOTE]
> U zult API Sleutel en de API Geheime waarden specifiek voor uw [ Nexmo ](https://dashboard.nexmo.com/) rekening op de aangewezen gebieden van MyAccountForm moeten verstrekken

![ trigger-submit ](assets/trigger-submit.JPG)



Dit formulier is gekoppeld aan een aangepaste verzendactie waarmee de formulierverzending wordt doorgestuurd naar het servlet dat op **/bin/renderaf** is gemonteerd.

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

De code in servlet op **/bin/renderaf** door:sturen het verzoek om de storeafwithattachments aanpassende vorm terug te geven vooraf bevolkt met de bewaarde gegevens.


* MyAccountForm kan [ van hier worden gedownload ](assets/my-account-form.zip)

* De vormen van de steekproef zijn gebaseerd op [ douane adaptieve vormmalplaatje ](assets/custom-template-with-page-component.zip) dat in AEM voor de steekproefvormen moet worden ingevoerd om correct terug te geven.

* [ Douane legt manager ](assets/custom-submit-my-account-form.zip) verbonden aan de voorlegging MyAccountForm moet in AEM worden ingevoerd.

## Volgende stappen

[Test de oplossing door de steekproefactiva op te stellen](./deploy-this-sample.md)
