---
title: Het MyAccountForm maken
description: Maak het mijnaccountformulier om het gedeeltelijk ingevulde formulier op te halen na geslaagde verificatie van de toepassings-id en het telefoonnummer.
feature: Adaptieve Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
topic: Ontwikkeling
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---



# Het MyAccountForm maken

Het formulier **MyAccountForm** wordt gebruikt om het gedeeltelijk ingevulde adaptieve formulier op te halen nadat de gebruiker de toepassings-id en het mobiele nummer dat aan de toepassings-id is gekoppeld, heeft geverifieerd.

![Mijn rekeningformulier](assets/6599.JPG)

Wanneer de gebruiker toepassingsidentiteitskaart ingaat en **FetchApplication** knoop klikt, wordt het mobiele aantal verbonden aan toepassings identiteitskaart opgehaald uit het gegevensbestand gebruikend de Get verrichting van het model van vormgegevens.

In dit formulier wordt gebruikgemaakt van de aanroep van de POST van het formuliergegevensmodel om het mobiele nummer te verifiëren met OTP. De verzendactie van het formulier wordt geactiveerd wanneer het mobiele nummer met de volgende code is geverifieerd. We activeren de gebeurtenis click van de verzendknop met de naam **submitForm**.

>[!NOTE]
> U moet de API-sleutel en de API-geheime waarden opgeven die specifiek zijn voor uw [Nexmo](https://dashboard.nexmo.com/)-account in de betreffende velden van het MyAccountForm

![trigger-submit](assets/trigger-submit.JPG)



Dit formulier is gekoppeld aan een aangepaste verzendactie die de formulierverzending doorstuurt naar het servlet dat is gemonteerd op **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

De code in het servlet op **/bin/renderaf** stuurt het verzoek door om de storeafwithattachments adaptieve vorm terug te geven die met de bewaarde gegevens vooraf wordt bevolkt.


* MyAccountForm kan [worden gedownload van hier](assets/my-account-form.zip)

* Voorbeeldformulieren zijn gebaseerd op aangepaste formuliersjablonen ](assets/custom-template-with-page-component.zip) die naar AEM moeten worden geïmporteerd om de voorbeeldformulieren correct te kunnen weergeven.[

* [Aangepaste verzendhandler ](assets/custom-submit-my-account-form.zip) die is gekoppeld aan de MyAccountForm-verzending moet worden geïmporteerd in AEM.
