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
duration: 60
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# Het MyAccountForm maken

Het formulier **MyAccountForm** wordt gebruikt om het gedeeltelijk ingevulde adaptieve formulier op te halen nadat de gebruiker de toepassings-id en het mobiele nummer dat aan de toepassings-id is gekoppeld, heeft geverifieerd.

![Mijn rekeningformulier](assets/6599.JPG)

Wanneer de gebruiker de toepassings-id invoert en op de knop **FetchApplication** wordt het mobiele nummer dat aan de toepassings-id is gekoppeld, opgehaald uit de database met de functie Ophalen van het formuliergegevensmodel.

In dit formulier wordt gebruikgemaakt van de aanroep van de POST van het formuliergegevensmodel om het mobiele nummer te verifiëren met OTP. De verzendactie van het formulier wordt geactiveerd nadat het mobiele nummer met de volgende code is geverifieerd. De gebeurtenis click van de verzendknop met de naam **submitForm**.

>[!NOTE]
> U moet de API-sleutel en de specifieke API-beveiligingswaarden opgeven voor uw [Nexmo](https://dashboard.nexmo.com/) account in de betreffende velden van het MyAccountForm

![trigger-submit](assets/trigger-submit.JPG)



Dit formulier is gekoppeld aan een aangepaste verzendactie waarmee de formulierverzending wordt doorgestuurd naar het servlet dat is geïnstalleerd op **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

De code in het servlet gemonteerd op **/bin/renderaf** stuurt de aanvraag door om de opslagbijlagen een adaptief formulier te geven dat vooraf is ingevuld met de opgeslagen gegevens.


* Het MyAccountForm kan [hier gedownload](assets/my-account-form.zip)

* Voorbeeldformulieren zijn gebaseerd op [aangepaste adaptieve formuliersjabloon](assets/custom-template-with-page-component.zip) die in AEM moeten worden geïmporteerd om de voorbeeldformulieren correct te kunnen weergeven.

* [Aangepaste verzendhandler](assets/custom-submit-my-account-form.zip) die zijn gekoppeld aan het verzenden van het MyAccountForm-formulier, moet worden geïmporteerd in AEM.

## Volgende stappen

[Test de oplossing door de steekproefactiva op te stellen](./deploy-this-sample.md)
