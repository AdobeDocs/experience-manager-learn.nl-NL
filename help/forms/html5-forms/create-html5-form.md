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
topic: Development
role: User
level: Beginner
exl-id: 67a01c41-d284-4518-adb5-21702e22ccfa
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 4%

---

# HTML5-formulieren maken

HTML5-formulieren zijn een nieuwe functie in Adobe Experience Manager die XFA-formuliersjablonen (xdp) in HTML5-indeling biedt. Met deze functie wordt de rendering van formulieren ingeschakeld op mobiele apparaten en in desktopbrowsers die geen XFA-gebaseerd pdf&#39;s ondersteunen. HTML5-formulieren ondersteunen niet alleen de bestaande mogelijkheden van XFA-formuliersjablonen, maar voegen ook nieuwe mogelijkheden toe, zoals een krabbelhandtekening, voor mobiele apparaten.

## Vereiste

Controleer of je een werkende versie van AEM Forms hebt. Volg de [installatiegids](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) AEM Forms installeren en configureren

## Uw eerste HTML5-formulier maken

1. [De inhoud van het ZIP-bestand downloaden en uitpakken](assets/assets.zip). Het ZIP-bestand bevat xdp en een gegevensbestand
2. [Naar Forms en Documenten navigeren](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. Klik op Maken -> Bestand uploaden
4. Selecteer de xdp-sjabloon die u in stap 2 hebt gedownload

## Voorvertonen als HTML

De xdp kan worden voorvertoond in de indeling HTML5 of PDF. Voer de volgende stappen uit om een voorvertoning van de xdp in HTML5-indeling te bekijken.

* Tik op de zojuist geüploade xdp en klik op _Voorvertoning -> Voorvertoning als HTML_. De xdp wordt weergegeven als HTML5

>[!NOTE]
>Wanneer u _Voorvertonen als PDF_ de weergegeven PDF wordt niet weergegeven in de browser omdat AEM Forms dynamische PDF&#39;s rendert waarvoor Acrobat-plug-in vereist is. U moet de PDF downloaden en openen met Adobe Acrobat/Reader om deze te kunnen weergeven


## Voorvertonen met gegevens

Ga als volgt te werk om een voorvertoning van de xdp in HTML5-indeling met het gegevensbestand weer te geven:

* Tik op de zojuist geüploade xdp en klik op _Voorvertoning -> Voorvertoning met gegevens_. Bladeren en het gegevensbestand selecteren en klikken op _Voorvertoning_.
* De sjabloon moet worden weergegeven in de HTML5-indeling die vooraf is ingevuld met de gegevens

## Geavanceerde eigenschappen van de XDP-sjabloon verkennen

Met de geavanceerde eigenschappen van de xdp-sjabloon kunt u de publicatiedatum opgeven, de verzendhandler, het profiel voor het formulier weergeven, de service Prefill enzovoort. Tik op de xdp om de geavanceerde eigenschappen van de sjabloon weer te geven en klik op _eigenschappen -> Geavanceerd_. Hier vindt u een aantal eigenschappen. Enkele van deze eigenschappen worden hier behandeld.

**URL verzenden** - Dit is de URL waarmee uw HTML5-formulier wordt verzonden. In de volgende les zullen we dit behandelen. Als hier geen verzendURL is opgegeven, wordt de standaardverzendhandler aangeroepen die de formuliergegevens retourneert naar de browser.

**HTML-renderprofiel** - HTML5-formulieren hebben het idee van profielen die als REST-eindpunten worden weergegeven om de mobiele weergave van formuliersjablonen mogelijk te maken. De meeste keren dat het standaardrenderprofiel voldoende is om het formulier te genereren. Als het standaardrenderprofiel niet aan uw behoeften voldoet, voert u een [aangepast profiel](https://experienceleague.adobe.com/docs/experience-manager-64/forms/html5-forms/custom-profile.html) kunnen worden gemaakt en gekoppeld aan het formulier.

**Prefill-service** - De vooraf ingevulde service wordt doorgaans gebruikt om uw formulier te vullen met gegevens die worden opgehaald van een gegevensbron met de achtergrond.
