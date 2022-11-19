---
title: De functie URL's van de vanity van de AEM
description: Begrijp hoe AEM omgaat met vanity URLs en extra technieken gebruikend herschrijf regels om inhoud dichter aan de rand van levering in kaart te brengen.
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '994'
ht-degree: 0%

---


# URL&#39;s van waarheidsgetrouwheid van verzender

[Inhoudsopgave](./overview.md)

[&lt;- Vorige: Verzending leegmaken](./disp-flushing.md)

## Overzicht

Dit document helpt u begrijpen hoe AEM omgaat met ijdelheidsURL&#39;s en enkele aanvullende technieken met behulp van herschrijfregels om inhoud dichter bij de rand van levering in kaart te brengen

## Wat zijn Vanity URL&#39;s

Wanneer u inhoud hebt die in een omslagstructuur leeft die steek houdt het niet altijd in een URL leeft die gemakkelijk om is te verwijzen.  Vanity URL&#39;s zijn vergelijkbaar met sneltoetsen.  Kortere of unieke URL&#39;s die verwijzen naar waar de echte inhoud zich bevindt.

Een voorbeeld: `/aboutus` gericht `/content/we-retail/us/en/about-us.html`

AEM-auteurs hebben de optie om URL-eigenschappen voor een kwaliteit in te stellen voor inhoud in AEM en deze te publiceren.

Deze functie werkt alleen als u de Dispatcher-filters aanpast, zodat de ijdelheid erdoor blijft.  Dit wordt onredelijk om met het aanpassen van de de configuratiedossiers van de Dispatcher aan het tarief te doen dat de auteurs deze ingang van de ijdelheidspagina zouden moeten plaatsen.

Om deze reden heeft de module Dispatcher een functie om automatisch alles toe te staan dat als een ijdelheid wordt vermeld in de inhoudsstructuur.


## Hoe werkt het

### URL&#39;s van Auteurs Vanity

De auteur bezoekt een pagina in AEM en bezoekt de pagina-eigenschappen en voegt items toe in de vanity URL-sectie.

Nadat de wijzigingen zijn opgeslagen en de pagina is geactiveerd, wordt de ijdelheid nu toegewezen aan deze pagina.

#### Aanraakinterface:

![Vervolgkeuzemenu voor AEM ontwerpgebruikersinterface op scherm van de siteeditor](assets/disp-vanity-url/aem-page-properties-drop-down.png "aem-page-properties-drop-down")

![Pagina-eigenschappen, dialoogvenster](assets/disp-vanity-url/aem-page-properties.png "aem-page-eigenschappen")

#### Klassieke inhoudszoeker:

![AEM eigenschappen van sitadmin classic ui sidekick-pagina](assets/disp-vanity-url/aem-page-properties-sidekick.png "aem-page-properties-sidekick")

![Eigenschappen van klassieke UI-pagina, dialoogvenster](assets/disp-vanity-url/aem-page-properties-classic.png "aem-page-properties-classic")

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Opmerking:</b>
Dit is erg gevoelig voor naamgevingsproblemen.

Vanity-inzendingen zijn wereldwijd op alle pagina&#39;s. Dit is slechts een van de tekortkomingen die u moet plannen voor tijdelijke oplossingen die we later zullen toelichten.
</div>

## Resolving/toewijzing van bronnen

Elk ijdelingitem is een sling map-item voor een interne omleiding.

Deze kaarten zijn zichtbaar door de AEM instanties van de console van Felix te bezoeken ( `/system/console/jcrresolver` )

Hier is een schermafbeelding van een kaartitem dat is gemaakt door een ijdelingvermelding:
![console schermafbeelding van een ijdelheidsingang in het middel die regels oplossen](assets/disp-vanity-url/vanity-resource-resolver-entry.png "vanity-resource-resolver-entry")

In het bovenstaande voorbeeld wanneer we de AEM vragen `/aboutus` zij zal `/content/we-retail/us/en/about-us.html`

## Filters die automatisch worden toegestaan door Dispatcher

De verzender in een veilige staat filtert uit verzoeken bij weg `/` via de Dispatcher, omdat dat de basis is van de JCR-boom.

Het is belangrijk om ervoor te zorgen dat uitgevers alleen inhoud van de `/content` en andere veilige paden enz.  en niet paden zoals `/system` enz.

Hier zijn de rub, ijdeloze URL&#39;s live in de basismap van `/` hoe kunnen we hen dan toestaan de uitgevers te bereiken terwijl ze veilig blijven ?

De eenvoudige Verzender heeft een auto-filter staat mechanisme toe en u moet een AEM pakket installeren en dan de Verzender vormen om aan die pakketpagina te richten.

[https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components)

De afzender heeft een configuratiesectie in het landbouwbedrijfdossier:

```
/vanity_urls { 
    /url    "/libs/granite/dispatcher/content/vanityUrls.html" 
    /file   "/tmp/vanity_urls" 
    /delay  300 
}
```

Deze configuratie vertelt de Dispatcher om dit URL van het AEM instantie te halen het negeert om de 300 seconden om de lijst van punten te halen wij door willen toestaan.

Het slaat het als geheim voorgeheugen van de reactie in op `/file` argument so in dit voorbeeld `/tmp/vanity_urls`

Zo als u de AEM instantie bij URI bezoekt zult u zien wat het haalt:
![screenshot van de inhoud die is gerenderd via /libs/granite/dispatcher/content/vanityUrls.html](assets/disp-vanity-url/vanity-url-component.png "vanity-url-component")

Het is letterlijk een lijst, supereenvoudig

## Regels herschrijven als Vanity Rules

Waarom zouden wij het gebruiken van herschrijven regels in plaats van het standaardmechanisme noemen dat in AEM zoals hierboven beschreven wordt gebouwd?

Uitgelicht eenvoudig, namespace kwesties, prestaties, en hogere niveaulogica die beter kunnen worden behandeld.

Laten we een voorbeeld van de ijdelheid-vermelding bekijken `/aboutus` naar inhoud `/content/we-retail/us/en/about-us.html` met Apache&#39;s `mod_rewrite` om dit te bereiken.

```
RewriteRule ^/aboutus /content/we-retail/us/en/about-us.html [PT,L,NC]
```

Deze regel zoekt naar de ijdelheid `/aboutus` en haalt de volledige weg van renderer met de vlag van PT (pas door).

Het zal ook ophouden met het verwerken van alle andere regels L markering (Laatste), wat betekent dat het geen enorme lijst van regels zoals het Oplossen van JCR moet oversteken.

Samen met het niet moeten van volmacht het verzoek en wachten op de AEM uitgever om op deze twee elementen van deze methode te antwoorden maakt het veel uitvoerbaarder.

Dan is het icing op de cake hier de vlag NC (geen case-Sensitive) die betekent als een klant URI met overvloeit `/AboutUs` in plaats van `/aboutus` het zal nog werken en zal de juiste pagina toestaan om te halen.

Als u een herschrijfregel wilt maken, maakt u een configuratiebestand op de Dispatcher (bijvoorbeeld: `/etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules`) en in de `.vhost` bestand dat het domein afhandelt waarvoor deze vanity-URL&#39;s moeten worden toegepast.

Hier volgt een voorbeeld van een codefragment van het include-bestand in `/etc/httpd/conf.d/enabled_vhosts/we-retail.vhost`

```
<VirtualHost *:80> 
 ServerName weretail.com 
 ServerAlias www.weretail.com 
        ........ SNIP ........ 
 <IfModule mod_rewrite.c> 
  ReWriteEngine on 
  LogLevel warn rewrite:info 
  Include /etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules 
 </IfModule> 
        ........ SNIP ........ 
</VirtualHost>
```

## Welke methode en waar

Het gebruik van AEM om ijdelingangen te controleren heeft de volgende voordelen
- Auteurs kunnen ze direct maken
- Ze leven met de inhoud en kunnen worden verpakt met de inhoud

Gebruiken `mod_rewrite` om ijdelingangen te controleren heeft de volgende voordelen
- Sneller inhoud oplossen
- Dichter bij de rand van verzoeken om inhoud voor eindgebruikers
- Meer uitbreidbaarheid en opties om te bepalen hoe inhoud wordt toegewezen aan andere voorwaarden
- Kan niet hoofdlettergevoelig zijn

Gebruik beide methoden, maar u kunt de volgende adviezen en criteria gebruiken:
- Als de ijdelheid tijdelijk is en lage geplande niveaus van verkeer heeft dan gebruik de AEM ingebouwde eigenschap
- Als de ijdelheid een nietszeggend eindpunt is dat niet vaak verandert en vaak gebruikt dan gebruik `mod_rewrite` regel.
- Als de naamruimte vanity (bijvoorbeeld: `/aboutus`) moet opnieuw worden gebruikt voor een groot aantal merken op hetzelfde AEM.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Opmerking:</b>

Als u de functie AEM ijdelheid wilt gebruiken en naamruimte wilt vermijden, kunt u een naamgevingsconventie maken.  Met ijdeloze URL&#39;s die zijn genest `/brand1/aboutus`, `brand2/aboutus`, `brand3/aboutus`.
</div>

[Volgende -> Algemene aanmelding](./common-logs.md)