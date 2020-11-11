---
title: Het MyAccountForm maken
description: Maak het mijnaccountformulier om het gedeeltelijk ingevulde formulier op te halen na geslaagde verificatie van de toepassings-id en het telefoonnummer.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---



# Het MyAccountForm maken

Het formulier **MyAccountForm** wordt gebruikt om het gedeeltelijk ingevulde adaptieve formulier op te halen nadat de gebruiker de toepassings-id en het mobiele nummer dat aan de toepassings-id is gekoppeld, heeft geverifieerd.

![Mijn rekeningformulier](assets/6599.JPG)

Wanneer de gebruiker de toepassings-id invoert en op de knop **FetchApplication** klikt, wordt het mobiele nummer dat aan de toepassings-id is gekoppeld, opgehaald uit de database met de functie Ophalen van het formuliergegevensmodel.

In dit formulier wordt gebruikgemaakt van de aanroep van de POST van het formuliergegevensmodel om het mobiele nummer te verifiëren met OTP. De verzendactie van het formulier wordt geactiveerd wanneer het mobiele nummer met de volgende code is geverifieerd. We activeren de klikgebeurtenis van de verzendknop met de naam **submitForm**.

>[!NOTE]
> U moet de API-sleutel en de API-geheime waarden die specifiek zijn voor uw [Nexmo](https://dashboard.nexmo.com/) -account opgeven in de desbetreffende velden van het MyAccountForm

![trigger-submit](assets/trigger-submit.JPG)



Dit formulier is gekoppeld aan een aangepaste verzendactie waarmee de formulierverzending wordt doorgestuurd naar het servlet dat is geïnstalleerd op **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

De code in het servlet op **/bin/renderaf** stuurt de aanvraag door om de opslagbestandsbijlagen adaptief formulier te genereren dat vooraf is ingevuld met de opgeslagen gegevens.


* Het MyAccountForm kan hier worden [gedownload](assets/my-account-form.zip)

* Voorbeeldformulieren zijn gebaseerd op een [aangepaste formuliersjabloon](assets/custom-template-with-page-component.zip) dat naar AEM moet worden geïmporteerd om de voorbeeldformulieren correct te kunnen weergeven.

* [Aangepaste verzendhandler](assets/custom-submit-my-account-form.zip) die aan de MyAccountForm-verzending is gekoppeld, moet in AEM worden geïmporteerd.
