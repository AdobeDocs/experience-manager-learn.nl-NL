---
title: De welkomstkit-oplossing testen
description: De oplossingselementen implementeren om de oplossing te testen
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
source-git-commit: 0e27907066c7d688549a980ccd17b3f17d74b60b
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# De voorbeeldelementen implementeren en testen

[Het pakket met de welkomstkit installeren](assets/welcomekit.zip). Dit pakket bevat de paginasjabloon, aangepaste component om de elementen, voorbeeldworkflow, e-mailsjabloon en voorbeeld-PDF-documenten weer te geven die u wilt opnemen in de welkomstkit.
[De welkomstkit-bundel installeren](assets/welcomekit.core-1.0.0-SNAPSHOT.jar). Deze bundel bevat de code waarmee de pagina wordt gemaakt en de klasse java waarmee de elementen worden geretourneerd die op de webpagina moeten worden weergegeven.
[Het adaptieve voorbeeldformulier installeren](assets/account-openeing-form.zip)
Configureer de Day CQ Mail Service. De werkstroom verzendt de welkomstkit-koppeling naar de formulierverzender waarvoor SMTP op de juiste wijze is geconfigureerd.
Configureer de component Send Email van de workflow naar wens
[Een voorbeeld van het formulier bekijken](http://localhost:4502/content/dam/formsanddocuments/co-operators/accountopeningform/jcr:content?wcmmode=disabled)
Voer uw gegevens in en selecteer een of meer onderlinge fondsen en verzend het formulier. U ontvangt een e-mail met een koppeling naar de welkomstkit


