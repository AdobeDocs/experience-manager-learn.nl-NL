---
title: Smart Imaging
description: Smart Imaging in Dynamic Media Classic verbetert de prestaties van de afbeeldingslevering door de afbeeldingsindeling en -kwaliteit automatisch te optimaliseren op basis van de mogelijkheden van de clientbrowser. Dit gebeurt door Adobe Sensei AI-mogelijkheden te benutten en met bestaande voorinstellingen voor afbeeldingen te werken. Meer informatie over Smart Imaging en hoe u deze kunt gebruiken om klanten betere ervaringen te bieden dankzij snellere paginabelasting.
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 678671c3-af25-4da1-bc14-cbc4cc19be8d
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '687'
ht-degree: 1%

---

# Smart Imaging {#smart-imaging}

Een van de belangrijkste aspecten van de klantervaring op uw website of mobiele site of app is de laadtijd van de pagina. Klanten zullen vaak een site of app verlaten als het te lang duurt om een pagina te laden. Afbeeldingen vormen het grootste deel van de laadtijd van de pagina. Smart Imaging in Dynamic Media Classic verbetert de prestaties van de afbeeldingslevering door de afbeeldingsindeling en -kwaliteit automatisch te optimaliseren op basis van de mogelijkheden van de clientbrowser. Dit gebeurt door Adobe Sensei AI-mogelijkheden te benutten en met bestaande voorinstellingen voor afbeeldingen te werken. Met Smart Imaging verkleint u de afbeeldingsgrootte met 30 procent of meer. Dit betekent dat pagina&#39;s sneller worden geladen en dat de klant er beter van wordt.

Smart Imaging is ook nuttig dankzij de extra prestatieverhoging die het mogelijk maakt om volledig te worden geïntegreerd met de beste hoogwaardige service van de Adobe. Deze dienst vindt de optimale Internet route tussen servers, netwerken, en peerpunten die de laagste latentie, en/of pakketverliestarief dan de standaardroute op Internet hebben.

Meer informatie over [Slimme afbeeldingen](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html).

## Voordelen van Smart Imaging

Omdat de beelden de meerderheid van de ladingstijd van een pagina vormen, kan de prestatiesverbetering van Slimme Beeldvorming een diepgaande invloed op zaken KPIs, zoals hogere omzetting, tijd die aan plaats wordt doorgebracht, en lagere plaats het stuiteren tarief hebben.

![afbeelding](assets/smart-imaging/smart-imaging-1.png)

## Hoe slim afbeeldingen werken

Zoals eerder vermeld, maakt Smart Imaging gebruik van Adobe Sensei AI-mogelijkheden en werkt het met bestaande voorinstellingen voor afbeeldingen om afbeeldingen automatisch om te zetten in optimale volgende generatie afbeeldingsindelingen, zoals WebP, terwijl de visuele getrouwheid behouden blijft.

Meer informatie over [Hoe slim afbeeldingen werken](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work), inclusief details zoals ondersteunde afbeeldingsindelingen (en wat er gebeurt als u deze indelingen niet gebruikt) en de invloed ervan op bestaande afbeeldingsvoorinstellingen die in gebruik zijn.

## Effecten van slimme beelden

U bent waarschijnlijk bezorgd dat u veranderingen in uw URLs, Beeld zult moeten aanbrengen vooraf instelt, en code op uw plaats om uit Slimme Beeldvorming voordeel te halen. Als u voldoet aan de voorwaarden voor het gebruik van Smart Imaging en u alleen werkt met afbeeldingen in de ondersteunde JPEG- en PNG-afbeeldingsindelingen, hoeft u geen wijzigingen aan te brengen.

Slimme afbeeldingen werken met afbeeldingen die via HTTP, HTTPS en HTTP/2 worden geleverd.

>[!NOTE]
>
>Als u over gaat naar Smart Imaging, wordt de cache bij de CDN gewist. De cache in de CDN wordt doorgaans binnen een of twee dagen opnieuw opgebouwd.

Smart Imaging is inbegrepen bij uw bestaande licentie van Dynamic Media Classic. Er zijn geen extra kosten voor deze functie. Om uit het voordeel te halen, moet u aan twee vereisten voldoen: hebben een Adobe-gebundelde CDN en een specifiek domein. Vervolgens moet u deze voor uw account inschakelen, omdat deze functie niet automatisch wordt ingeschakeld.

Als u Smart Imaging inschakelt, begint u met het verzenden van een verzoek om technische ondersteuning door |tot instelling van een steungeval| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html). De steun zal met u aan opstelling werken een douanedomein dat u met Slimme Beelden zult associëren. U gaat één parameter met betrekking tot caching (Tijd te leven, of TTL) veranderen en de steun zal het geheime voorgeheugen ontruimen. U kunt desgewenst ook een optionele stapsgewijze stap uitvoeren voordat u naar de productie gaat. Als Smart Imaging vervolgens is ingeschakeld, levert u kleinere afbeeldingen op maat voor klanten, maar met dezelfde kwaliteit als u had gevraagd. Dat betekent dat ze sneller pagina&#39;s laden. Dit gebeurt allemaal automatisch omdat Adobe Sensei de meest efficiënte grootte kan kiezen.

Als u Smart Imaging hebt ingeschakeld, wilt u controleren of het werkt zoals u had verwacht.

U hebt waarschijnlijk aanvullende vragen over Smart Imaging. We hebben een lijst met veelgestelde vragen (FAQ&#39;s) met antwoorden samengesteld. Lees de [Veelgestelde vragen](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html).

## Aanvullende bronnen

Kijk naar de [Dynamic Media Classic Optimizing Page Performance Skill Builder](https://seminars.adobeconnect.com/pzc1gw0cihpv) webinar op aanvraag voor meer informatie over Smart Imaging.
