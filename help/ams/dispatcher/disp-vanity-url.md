---
title: AEM Dispatcher vanity URL's, functie
description: Begrijp hoe AEM omgaat met vanity URLs en extra technieken gebruikend herschrijf regels om inhoud dichter aan de rand van levering in kaart te brengen.
version: Experience Manager 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 53baef9c-aa4e-4f18-ab30-ef9f4f5513ee
duration: 244
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1159'
ht-degree: 0%

---

# Dispatcher Vanity URL&#39;s

[Inhoudsopgave](./overview.md)

[&lt;- Vorige: Dispatcher flushing](./disp-flushing.md)

## Overzicht

Dit document helpt u begrijpen hoe AEM omgaat met ijdelheidsURL&#39;s en enkele aanvullende technieken met behulp van herschrijfregels om inhoud dichter bij de rand van levering in kaart te brengen

## Wat zijn Vanity URL&#39;s

Wanneer u inhoud hebt die in een omslagstructuur leeft die steek houdt het niet altijd in een URL leeft die gemakkelijk om is te verwijzen. Vanity URL&#39;s zijn vergelijkbaar met sneltoetsen. Kortere of unieke URL&#39;s die verwijzen naar waar de echte inhoud zich bevindt.

Een voorbeeld: `/aboutus` punst bij `/content/we-retail/us/en/about-us.html`

AEM-auteurs hebben een optie om URL-eigenschappen van het type vanity in te stellen voor inhoud in AEM en deze te publiceren.

Deze functie werkt alleen als u de Dispatcher-filters aanpast zodat de ijdelheid erdoor heen kan. Dit wordt onredelijk om met het aanpassen van de de configuratiedossiers van Dispatcher aan het tarief te doen dat de auteurs deze punten van de ijdelheidspagina zouden moeten plaatsen.

Daarom heeft de Dispatcher-module een functie waarmee automatisch alles wordt toegestaan dat als een ijdelheid in de inhoudsstructuur wordt vermeld.


## Hoe werkt het

### URL&#39;s van Auteurs Vanity

De auteur bezoekt een pagina in AEM, klikt de paginaeigenschappen, en voegt ingangen in de _sectie van de Vanity URL_ toe. Als u de wijzigingen opslaat en de pagina activeert, wordt de ijdelheid aan de pagina toegewezen.

De auteurs kunnen _Redirect van de Vanheid URL_ checkbox ook selecteren wanneer het toevoegen van _Vanity URL_ ingangen, dit veroorzaakt ijzelaars om zich als 302 redirects te gedragen. Dit betekent dat de browser wordt gevraagd naar de nieuwe URL (via `Location` antwoordheader) en dat de browser een nieuwe aanvraag doet voor de nieuwe URL.

#### Aanraakinterface:

![ drop-down dialoogmenu voor AEM authoring UI op het scherm van de plaatsredacteur ](assets/disp-vanity-url/aem-page-properties-drop-down.png " aem-page-properties-drop-down ")

![ naam pagina eigenschappen de pagina van de de dialoog pagina ](assets/disp-vanity-url/aem-page-properties.png " a-page-properties ")

#### Klassieke inhoudszoeker:

![ AEM siteAdmin klassieke ui sidekick paginaeigenschappen ](assets/disp-vanity-url/aem-page-properties-sidekick.png " aem-page-properties-sidekick ")

![ Klassieke UI de paginaeigenschappen dialoogdoos ](assets/disp-vanity-url/aem-page-properties-classic.png " aem-pagina-eigenschappen-klassieke ")


>[!NOTE]
>
>Begrijpen dat dit vaak voorkomt bij het benoemen van ruimteproblemen. Vanity-inzendingen zijn wereldwijd op alle pagina&#39;s. Dit is slechts een van de tekortkomingen die u moet plannen voor tijdelijke oplossingen die we later zullen toelichten.


## Resolving/toewijzing van bronnen

Elk ijdelingitem is een sling map-item voor een interne omleiding.

De kaarten zijn zichtbaar door de AEM instances Felix console te bezoeken ( `/system/console/jcrresolver` )

Hier volgt een schermafbeelding van een kaartitem dat is gemaakt door een ijdelingvermelding:
![ consolescherm van een ijdelheidsingang in het middel die regels ](assets/disp-vanity-url/vanity-resource-resolver-entry.png " oplossen vanity-middel-resolver-ingang ")

In het bovenstaande voorbeeld wanneer we de AEM-instantie vragen `/aboutus` te bezoeken, wordt het omgezet in `/content/we-retail/us/en/about-us.html`

## Dispatcher-filters voor automatisch toestaan

De Dispatcher in een veilige status filtert verzoeken uit op het pad `/` via de Dispatcher, omdat dat de basis is van de JCR-structuur.

Het is belangrijk om ervoor te zorgen dat uitgevers alleen inhoud van `/content` en andere veilige paden toestaan, enzovoort, en geen paden zoals `/system` .

Hier ziet u de rub, ijdelheid-URL&#39;s die zich bevinden in de basismap van `/` , dus hoe kunnen we ze toestaan om de uitgevers te bereiken terwijl ze veilig blijven?

Eenvoudige Dispatcher heeft een mechanisme voor automatisch filteren. U moet een AEM-pakket installeren en vervolgens de Dispatcher zodanig configureren dat deze naar die pakketpagina wijst.

[ https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components)

Dispatcher heeft een configuratiesectie in zijn landbouwbedrijfdossier:

```
/vanity_urls { 
    /url    "/libs/granite/dispatcher/content/vanityUrls.html" 
    /file   "/tmp/vanity_urls" 
    /delay  300 
}
```

De parameter `/delay`, gemeten in seconden, werkt niet met een vaste intervalbasis, maar met een op voorwaarde gebaseerde controle. De Dispatcher beoordeelt de tijdstempel van de wijziging van `/file` (waarin de lijst met herkende vanity-URL&#39;s wordt opgeslagen) wanneer een aanvraag voor een niet-vermelde URL wordt ontvangen. `/file` wordt niet vernieuwd als het tijdverschil tussen het huidige moment en de laatste wijziging van `/file` kleiner is dan de duur van `/delay` . Het vernieuwen van `/file` vindt plaats onder twee voorwaarden:

1. De binnenkomende aanvraag is voor een URL die niet in de cache is geplaatst of die niet in de `/file` wordt vermeld.
1. Er zijn ten minste `/delay` seconden verstreken sinds `/file` voor het laatst is bijgewerkt.

Dit mechanisme wordt ontworpen om tegen Ontkenning van de aanvallen van de Dienst (Dos) te beschermen, die anders Dispatcher met verzoeken kon overweldigen, die de eigenschap van Vanity URLs uitbuiten.

Eenvoudiger gezegd, wordt `/file` die vanity URLs bevat slechts bijgewerkt als een verzoek voor een URL aankomt niet reeds in `/file` en als de laatste wijziging van `/file` langer geleden dan de `/delay` periode was.

Als u expliciet wilt dat de functie `/file` wordt vernieuwd, kunt u een niet-bestaande URL aanvragen nadat u hebt gecontroleerd of de vereiste `/delay` -tijd sinds de laatste update is verstreken. Voorbeeld-URL&#39;s voor dit doel zijn:

- `https://dispatcher-host-name.com/this-vanity-url-does-not-exist`
- `https://dispatcher-host-name.com/please-hand-me-that-planet-maestro`
- `https://dispatcher-host-name.com/random-vanity-url`

Deze benadering dwingt de Dispatcher om `/file` bij te werken, op voorwaarde dat het opgegeven `/delay` -interval is verstreken sinds de laatste wijziging.

De cache van de reactie wordt opgeslagen in het argument `/file` , dus in dit voorbeeld `/tmp/vanity_urls`

Dus als u de AEM-instantie op de URI bezoekt, ziet u wat deze ophaalt:

![ het schermschot van de inhoud die van /libs/granite/dispatcher/content/vanityUrls.html ](assets/disp-vanity-url/vanity-url-component.png " wordt teruggegeven vanity-url-component ")

Het is letterlijk een lijst, super simpel

## Regels herschrijven als Vanity Rules

Waarom zouden we het gebruik van herschrijfregels noemen in plaats van het standaardmechanisme dat in AEM is ingebouwd, zoals hierboven beschreven?

Uitgelicht eenvoudig, namespace kwesties, prestaties, en hoger-vlakke logica die beter kunnen worden behandeld.

Laten we een voorbeeld van de ijdelheid-vermelding `/aboutus` doornemen naar de inhoud `/content/we-retail/us/en/about-us.html` met de module `mod_rewrite` van Apache.

```
RewriteRule ^/aboutus /content/we-retail/us/en/about-us.html [PT,L,NC]
```

Deze regel zoekt naar de ijdelheid `/aboutus` en haalt de volledige weg van renderer met de vlag van PT (pas door).

Het houdt ook op met het verwerken van alle andere regels L-markering (Last), wat betekent dat het niet nodig is om een enorme lijst regels te doorlopen, zoals JCR Resolving.

De AEM-uitgever hoeft geen proxy voor de aanvraag op te geven en moet wachten tot deze twee elementen van deze methode op de aanvraag reageren. Hierdoor werkt de toepassing veel beter.

Dan is het icing op de cake hier de markering NC (Geen case-Sensitive) die betekent als een klant URI met `/AboutUs` in plaats van `/aboutus` typt het nog werkt.

Als u hiervoor een herschrijfregel wilt maken, maakt u een configuratiebestand op de Dispatcher (bijvoorbeeld: `/etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules` ) en neemt u dit op in het `.vhost` -bestand dat het domein afhandelt waarvoor deze ijdelheidsURL&#39;s moeten worden toegepast.

Hier volgt een voorbeeld van een codefragment of include-bestand in `/etc/httpd/conf.d/enabled_vhosts/we-retail.vhost`

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

Het gebruik van AEM om items met een ijdelheid te besturen, heeft de volgende voordelen

- Auteurs kunnen ze direct maken
- Ze leven met de inhoud en kunnen worden verpakt met de inhoud

Het gebruik van `mod_rewrite` om items met een ijdelheid te besturen, heeft de volgende voordelen

- Sneller inhoud oplossen
- Dichter bij de rand van verzoeken om inhoud voor eindgebruikers
- Meer uitbreidbaarheid en opties om te bepalen hoe inhoud wordt toegewezen aan andere voorwaarden
- Kan niet hoofdlettergevoelig zijn

Gebruik beide methoden, maar u kunt de volgende adviezen en criteria gebruiken:

- Als de ijdelheid tijdelijk is en lage geplande niveaus van verkeer heeft, dan gebruik de ingebouwde eigenschap van AEM
- Als de ijdelheid een nietszeggend eindpunt is dat niet vaak verandert en vaak gebruikt dan gebruik een `mod_rewrite` regel.
- Als de naamruimte vanity (bijvoorbeeld: `/aboutus` ) opnieuw moet worden gebruikt voor veel merken op dezelfde AEM-instantie, gebruikt u herschrijfregels.

>[!NOTE]
>
>U kunt een naamgevingsconventie maken als u de functie AEM-ijdelheid wilt gebruiken en naamruimte wilt vermijden. Met ijdeloze URL&#39;s die zijn genest als `/brand1/aboutus` , `brand2/aboutus` , `brand3/aboutus` .

[Volgende -> Gemeenschappelijke Logboekregistratie](./common-logs.md)
