---
title: HTML5 Forms maken
description: HTML5-formulieren maken en configureren
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.5
jira: KT-4419
thumbnail: kt-4419.jpg
topic: Development
role: User
level: Beginner
exl-id: 67a01c41-d284-4518-adb5-21702e22ccfa
last-substantial-update: 2019-07-07T00:00:00Z
duration: 101
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 0%

---

# HTML5-formulieren maken

HTML5-formulieren zijn een nieuwe functie in Adobe Experience Manager die XFA-formuliersjablonen (xdp) in HTML5-indeling biedt. Met deze functie wordt de weergave mogelijk van formulieren op mobiele apparaten en desktopbrowsers waarop XFA-gebaseerde PDF niet wordt ondersteund. HTML5-formulieren ondersteunen niet alleen de bestaande mogelijkheden van XFA-formuliersjablonen, maar voegen ook nieuwe mogelijkheden toe, zoals een krabbelhandtekening, voor mobiele apparaten.

## Vereiste

Controleer of je een werkende versie van AEM Forms hebt. Gelieve te volgen de [&#x200B; installatiegids &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html?lang=nl-NL) om AEM Forms te installeren en te vormen

## Uw eerste HTML5-formulier maken

1. [&#x200B; Download en haalt de inhoud van zip dossier &#x200B;](assets/assets.zip). Het ZIP-bestand bevat xdp en een gegevensbestand
2. [&#x200B; ga aan Forms en Documenten &#x200B;](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. Klik op Maken -> Bestand uploaden
4. Selecteer de xdp-sjabloon die u in stap 2 hebt gedownload

## Voorvertonen als HTML

U kunt een voorvertoning van de xdp weergeven in de HTML5-indeling of in de PDF-indeling. Ga als volgt te werk om een voorvertoning van het xdp-bestand in HTML5-indeling weer te geven

* Tik op onlangs geupload xdp en klik _Voorproef -> Voorproef als HTML_. De xdp wordt weergegeven als HTML5

>[!NOTE]
>Wanneer u _Voorproef als PDF_ optie selecteert zal teruggegeven PDF niet in browser worden getoond omdat AEM Forms dynamische pdf&#39;s teruggeeft die de stop van Acrobat vereisen.U zult PDF moeten downloaden en het openen gebruikend Adobe Acrobat/Reader om te bekijken


## Voorvertonen met gegevens

Ga als volgt te werk om een voorvertoning van de xdp in HTML5-indeling met het gegevensbestand weer te geven:

* Tik op onlangs geupload xdp en klik _Voorproef -> Voorproef met Gegevens_. Blader en selecteer het gegevensdossier en klik op _Voorproef_.
* De sjabloon moet worden weergegeven in de HTML5-indeling die vooraf is ingevuld met de gegevens

## Geavanceerde eigenschappen van de XDP-sjabloon verkennen

Met de geavanceerde eigenschappen van de xdp-sjabloon kunt u de publicatiedatum opgeven, de verzendhandler, het profiel voor het formulier weergeven, de service Prefill enzovoort. Om de geavanceerde eigenschappen van het malplaatje te bekijken kraken op xdp en _eigenschappen -> Geavanceerd_ te klikken. Hier vindt u een aantal eigenschappen. Enkele van deze eigenschappen worden hier behandeld.

**legt URL** voor - dit is URL die uw HTML5 vormvoorlegging zal behandelen. In de volgende les zullen we dit behandelen. Als hier geen verzendURL is opgegeven, wordt de standaardverzendhandler aangeroepen die de formuliergegevens retourneert naar de browser.

**HTML geeft Profiel** terug - de vormen HTML5 hebben het begrip van Profielen die als Eindpunten van REST worden blootgesteld om Mobiele Rendering van de Malplaatjes van de Vorm toe te laten. De meeste keren dat het standaardrenderprofiel voldoende is om het formulier te genereren. Als het gebrek teruggeeft profiel niet aan uw behoeften voldoet, kan het a [&#x200B; douaneprofiel &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-65/forms/html5-forms/custom-profile.html?lang=nl-NL) worden gecreeerd en met de vorm worden geassocieerd.

**de Vooraf ingevulde Dienst** - de Prefill dienst wordt typisch gebruikt om uw vorm met gegevens te bevolken die van een achterste gegevensbron worden gehaald.
