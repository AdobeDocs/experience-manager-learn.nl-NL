---
title: De snelheid van de inhoud versnellen met AEM stijlsystemen
description: Leer hoe u AEM Stijlsystemen kunt gebruiken om ontwerpers, makers van inhoud en ontwikkelaars in uw organisatie in staat te stellen ervaringen te maken en te leveren op de snelheid en schaal die uw klanten verwachten.
solution: Experience Manager
exl-id: 449cd133-6ab6-456e-a0ad-30e3dea9b75b
source-git-commit: 89982f506a5e1ffc12f84a0f616aaa1dc2e00c5b
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 0%

---

# De snelheid van de inhoud versnellen met AEM stijlsystemen

In dit artikel leert u hoe u systemen in AEM stijl kunt gebruiken om ontwerpers, inhoudsauteurs en ontwikkelaars in uw organisatie in staat te stellen ervaringen te maken en te leveren op de snelheid en schaal die uw klanten verwachten.

## Overzicht

AEM stijlsystemen hebben vier belangrijke voordelen:

* Sjabloonauteurs kunnen stijlklassen definiëren in het inhoudsbeleid van een component of pagina
* Inhoudsauteurs kunnen stijlen selecteren die op een gehele pagina moeten worden toegepast of die een component op een pagina bewerken
* Componenten en sjablonen worden flexibeler gemaakt door auteurs in staat te stellen alternatieve visuele variaties weer te geven
* De noodzaak om een aangepaste component en/of complexe dialoogvensters te ontwikkelen om componentvariaties te presenteren, wordt verminderd of volledig verwijderd

## Eerste installatie en gebruik

De installatie in 5 stappen lijkt sterk op een standaardworkflow voor de ontwikkeling van componenten.

| **Leiderschap** | **Designer** | **Ontwikkelaar/architect** | **Sjabloonauteur** | **Inhoudsauteur** |
| --- | --- | --- | --- | --- |
| Bepaalt de inhoud en de doelstellingen voor die component | Bepaalt visuele en ervaringspresentatie van inhoud | Ontwikkelt CSS en JS om ervaring te steunen; definieert en biedt klassenamen die moeten worden gebruikt | Vormt sjabloonbeleid voor opgemaakte componenten door CSS-klassennamen toe te voegen die door ontwikkelaars zijn gedefinieerd. Gebruikervriendelijke namen moeten voor elke stijl worden gebruikt. | Hiermee past u tijdens het ontwerpen van pagina&#39;s waar nodig stijlen toe om de gewenste vormgeving te bereiken |

Hoewel dit de eerste configuratie is, hebben veel van onze klanten extra flexibiliteit bereikt door dit proces te stroomlijnen, bijvoorbeeld door hun CSS naar de DAM te uploaden, waardoor updates van stijlen mogelijk zijn zonder dat implementatie nodig is. Andere klanten hebben een volledig gekenmerkte reeks nutsklassen, die hen toestaat om componenten en stijlen te ontwikkelen die dan zonder plaatsing of ontwikkeling kunnen worden gebruikt.

Stijlsystemen hebben een aantal verschillende kleuren:

1. Lay-outstijlen

   * Verscheidene wijzigingen in ontwerp en layout

   * Wordt gebruikt voor duidelijk gedefinieerde en identificeerbare uitvoering

1. Stijlen weergeven
   * Kleine variaties die de fundamentele aard van de stijl niet veranderen

   * Bijvoorbeeld het wijzigen van het kleurenschema, het lettertype, de afdrukstand, enz.

1. Informatiestijlen

   * Velden tonen/verbergen

>[!NOTE]
>
>Voor een demo over deze functies raden we u aan onze [Webinar voor succes van klant](https://adobecustomersuccess.adobeconnect.com/pob610c9mffjmp4/) met Will Brisbane en Joseph Van Buskirk.

## Aanbevolen procedures

* Standaardstijl eerst verharden
   * Layout en weergave van component wanneer deze op de pagina wordt neergezet voordat stijlsystemen worden toegepast
   * Dit moet de meest gebruikte vertoning zijn
* Probeer alleen stijlopties weer te geven die waar mogelijk een effect hebben
   * Als ineffectieve combinaties worden blootgesteld, zorg ervoor zij geen negatieve gevolgen veroorzaken
   * Bijvoorbeeld een lay-outstijl die de afbeeldingspositie instelt en vergezeld gaat van een ineffectieve weergavestijl die de afbeeldingspositie bepaalt
* Optie voor layoutstijlen boven gecombineerde weergavestijlen
   * Vermindert het aantal permutaties dat moet worden gecontroleerd op kwaliteit
   * Zorgt ervoor dat de merkstandaarden worden nageleefd
   * Vereenvoudigt het ontwerpen voor inhoudsauteurs
   * Helpt u een consistente identiteit van uw merk te maken
* Wees voorzichtig met gecombineerde stijlen
   * Zowel binnen als binnen categorieën
* De juiste tijd toewijzen om gecombineerde stijlen grondig te testen
   * Bijwerkingen voorkomen
* Minimaliseer het aantal stijlopties en permutaties
   * Te veel opties kunnen leiden tot een gebrek aan merkconsistentie voor look and feel
   * Kan verwarring veroorzaken bij auteurs van inhoud waarop combinaties nodig zijn om het gewenste effect te bereiken
   * Hiermee verhoogt u de permutaties die moeten worden gecontroleerd op kwaliteit
* Gebruiksvriendelijke stijllabels en -categorieën gebruiken
   * &quot;Blauw&quot; en &quot;Rood&quot; in plaats van &quot;Primair&quot; en &quot;Secundair
   * &quot;Kaart&quot; en &quot;Hero&quot; in plaats van &quot;Variatie A&quot; en &quot;Variatie B&quot;
   * Dit kan voor sommige klanten meer algemeen zijn; het ontwerpteam, het bedrijfsteam, en het inhoudsteam zijn zeer vertrouwd met wat hun primaire en secundaire kleuren zijn of welke variaties zij testen. Maar voor de flexibiliteit en de mogelijkheden voor toekomstige veranderingen kan het gebruik van specifieke termen efficiënter zijn.

## Toetsen

Stijlsystemen verminderen de behoefte aan complexe dialogen, maar zijn geen vervanging van dialoogvensters. Ze helpen de zaken te vereenvoudigen, maar het kan voorkomen dat u componenteneigenschappen of dialoogvensters wilt gebruiken in plaats van daarvoor een stijlsysteem te maken.

Ze kunnen processen vanuit ontwikkelingsperspectief stroomlijnen. Met één stijlsysteem kunt u meerdere weergaven van dezelfde inhoud maken. Op dezelfde manier vanuit het ontwerpperspectief, in plaats van trainingsauteurs, en auteurs die moeten onthouden welke component aan gebruik in welk paleis, u de auteurssnelheid kunt versnellen.

Het is gewoon schoner. De HTML binnen de kerncomponenten is zeer uitgebreid. Als u dit alles op CSS-niveau doet, wordt de component sneller samengesteld en wordt de code ook schoner.

Tot slot is het gebruik van Stijlsystemen meer kunst dan wetenschap. Zoals wij hebben besproken, zijn er een aantal beste praktijken, maar u zult flexibiliteit in hoe te hebben de opstelling van uw organisatie aanpassen.

Voor meer informatie raadpleegt u onze [Klantsucces Webinar](https://adobecustomersuccess.adobeconnect.com/pob610c9mffjmp4/) met Will Brisbane en Joseph Van Buskirk.
