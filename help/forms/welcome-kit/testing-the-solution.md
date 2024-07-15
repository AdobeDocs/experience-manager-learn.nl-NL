---
title: De welkomstkit-oplossing testen
description: De oplossingselementen implementeren om de oplossing te testen
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: 07a1a9fc-7224-4e2d-8b6d-d935b1125653
duration: 29
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# De voorbeeldelementen implementeren en testen

[ installeer het welkome pakket van Kit ](assets/welcomekit.zip). Dit pakket bevat de paginasjabloon, aangepaste component om de elementen, voorbeeldworkflow, e-mailsjabloon en voorbeeld-PDF-documenten weer te geven die u wilt opnemen in de welkomstkit.
[ installeer de welkom-Kit bundel ](assets/welcomekit.core-1.0.0-SNAPSHOT.jar). Deze bundel bevat de code waarmee de pagina wordt gemaakt en de klasse java waarmee de elementen worden geretourneerd die op de webpagina moeten worden weergegeven.
[ installeer de steekproef adaptieve vorm ](assets/account-openeing-form.zip)
Configureer de Day CQ Mail Service. De werkstroom verzendt de welkomstkit-koppeling naar de formulierverzender waarvoor SMTP op de juiste wijze is geconfigureerd.
Configureer de component Send Email van de workflow naar wens
[ Voorproef de vorm ](http://localhost:4502/content/dam/formsanddocuments/co-operators/accountopeningform/jcr:content?wcmmode=disabled)
Voer uw gegevens in en selecteer een of meer onderlinge fondsen en verzend het formulier
U moet een e-mail ontvangen met een koppeling naar de welkomstkit
