---
title: HTML5 Forms maken
description: HTML5-formulieren maken en configureren
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 4419
thumbnail: kt-4419.jpg
topic: Ontwikkeling
role: Zakelijke praktiserer
level: Begin
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '491'
ht-degree: 4%

---


# HTML5-formulieren maken

HTML5-formulieren zijn een nieuwe functie in Adobe Experience Manager die XFA-formuliersjablonen (xdp) in HTML5-indeling biedt. Met deze functie wordt de rendering van formulieren ingeschakeld op mobiele apparaten en in desktopbrowsers die geen XFA-gebaseerd pdf&#39;s ondersteunen. HTML5-formulieren ondersteunen niet alleen de bestaande mogelijkheden van XFA-formuliersjablonen, maar voegen ook nieuwe mogelijkheden toe, zoals een krabbelhandtekening, voor mobiele apparaten.

## Vereiste

Controleer of je een werkende versie van AEM Forms hebt. Volg de [installatiehandleiding](https://docs.adobe.com/content/help/en/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) om AEM Forms te installeren en configureren

## Uw eerste HTML5-formulier maken

1. [Download en extraheer de inhoud van het ZIP-bestand](assets/assets.zip). Het ZIP-bestand bevat xdp en een gegevensbestand
2. [Naar Forms en Documenten navigeren](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. Klik op Maken -> Bestand uploaden
4. Selecteer de xdp-sjabloon die u in stap 2 hebt gedownload

## Voorvertonen als HTML

U kunt een voorbeeld van de xdp bekijken in de HTML5-indeling of in de PDF-indeling. Ga als volgt te werk om een voorvertoning van het xdp in HTML5-indeling weer te geven

* Tik op de zojuist geüploade xdp en klik op _Voorvertoning -> Voorvertoning als HTML_. De xdp wordt weergegeven als HTML5

>[!NOTE]
>Als u de optie _Voorvertonen als PDF_ selecteert, wordt het weergegeven PDF-bestand niet weergegeven in de browser omdat AEM Forms dynamische PDF&#39;s rendert waarvoor Acrobat-insteekmodule is vereist. U moet de PDF downloaden en openen met Adobe Acrobat/Reader om het bestand te kunnen weergeven


## Voorvertonen met gegevens

Ga als volgt te werk om een voorvertoning van de xdp in HTML5-indeling met het gegevensbestand weer te geven:

* Tik op de zojuist geüploade xdp en klik op _Voorvertoning -> Voorvertoning met gegevens_. Blader en selecteer het gegevensbestand en klik op _Voorvertoning_.
* De sjabloon moet worden weergegeven in de HTML5-indeling die vooraf is gevuld met de gegevens

## Geavanceerde eigenschappen van de XDP-sjabloon verkennen

Met de geavanceerde eigenschappen van de xdp-sjabloon kunt u de publicatiedatum opgeven, de verzendhandler, het profiel voor het formulier weergeven, de service Prefill enzovoort. Tik op de xdp om de geavanceerde eigenschappen van de sjabloon weer te geven en klik op _eigenschappen -> Geavanceerd_. Hier vindt u een aantal eigenschappen. Enkele van deze eigenschappen worden hier behandeld.

**URL**  verzenden - Dit is de URL die uw HTML5-formulier verzendt. In de volgende les zullen we dit behandelen. Als hier geen verzendURL is opgegeven, wordt de standaardverzendhandler aangeroepen die de formuliergegevens retourneert naar de browser.

**HTML-renderprofiel**  - HTML5-formulieren hebben het idee van profielen die als REST-eindpunten worden weergegeven om de mobiele weergave van formuliersjablonen mogelijk te maken. De meeste keren dat het standaardrenderprofiel voldoende is om het formulier te genereren. Als het standaardrenderprofiel niet aan uw behoeften voldoet, kan een [aangepast profiel](https://docs.adobe.com/content/help/en/experience-manager-64/forms/html5-forms/custom-profile.html) worden gemaakt en aan het formulier worden gekoppeld.

**De vooraf ingevulde Dienst**  - de vooraf ingevulde dienst wordt typisch gebruikt om uw vorm met gegevens te bevolken die van een achterste gegevensbron worden opgehaald.

