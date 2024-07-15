---
title: AEM Forms installeren in Linux
description: Leer hoe u 32-bits bibliotheken installeert voor AEM Forms om te werken met Linux-installatie.
feature: Adaptive Forms
type: Tutorial
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-7593
exl-id: b9809561-e9bd-4c67-bc18-5cab3e4aa138
last-substantial-update: 2019-06-09T00:00:00Z
duration: 204
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '938'
ht-degree: 0%

---

# 32-bits versie van gedeelde bibliotheken installeren

Wanneer AEM FORMS OSGi of AEM Forms j2EE op Linux wordt opgesteld, moet u ervoor zorgen dat de versies met 32 bits van een reeks gedeelde bibliotheken geïnstalleerd en beschikbaar zijn.  De beschrijvingen zijn afkomstig van de pakketten zelf.

* expat (Stream-oriented de parser C bibliotheek van XML voor het ontleden van XML, die door James Clark wordt geschreven)
* fontconfig (lettertypeconfiguratie en aanpassingsbibliotheek die zijn ontworpen om lettertypen te zoeken in het systeem en deze te selecteren volgens de vereisten die door toepassingen zijn opgegeven)
* freetype (Font rendering engine, ontwikkeld voor geavanceerde ondersteuning van lettertypen voor verschillende platforms en omgevingen. U kunt lettertypebestanden openen en beheren, maar ook afzonderlijke glyphs efficiënt laden, hints geven en weergeven. Het is geen lettertypeserver of een volledige tekstrenderingbibliotheek)
* glibc (Core libraries for the GNU system and GNU/Linux systems, evenals vele andere systemen die Linux als kernel gebruiken)
* libcurl (client-side bibliotheek voor URL-overdracht)
* libICE (Interclient Exchange Library)
* libicu (Bibliotheek die robuuste en volledig-gekenmerkte steun van Unicode en van de landinstelling - Internationale Componenten voor Unicode verstrekt). Zowel 64-bits als 32-bits versies van deze bibliotheek zijn vereist
* libSM (X11 Session Management Library)
* libuuid (DCE-compatibele Universal Identifier-bibliotheek - wordt gebruikt om unieke id&#39;s te genereren voor objecten die buiten het lokale systeem toegankelijk kunnen zijn)
* libX11 (X11 client-side bibliotheek)
* libXau (X11 Authorization Protocol - nuttig om cliënttoegang tot de vertoning te beperken)
* libxcb (X protocol C-taal Binding - XCB)
* libXext (Bibliotheek voor gemeenschappelijke uitbreidingen aan het protocol X11)
* libXinerama (X11-extensie die ondersteuning biedt voor het uitbreiden van een desktop voor meerdere schermen. De naam is een woordgroep op Cinerama, een breedbeeldfilmindeling die gebruikmaakte van meerdere projectors. libXinerama is de bibliotheek die met de uitbreiding RandR) omzet
* libXrandr (de uitbreiding van Xinerama is tegenwoordig grotendeels verouderd - het is vervangen door de uitbreiding van RandR)
* libXrender (X Rendering Extension Client Library)
nss-softokn-freebl (Freebl-bibliotheek voor Network Security Services)
* zlib (bibliotheek voor algemene doeleinden, zonder patent, zonder gegevensverlies)

Vanaf Red Hat Enterprise Linux 6 heeft de 32-bits editie van een bibliotheek de bestandsnaamextensie .686, terwijl de 64-bits editie .x86_64 heeft. Voorbeeld, expat.i686. Vóór RHEL 6 hadden 32-bits edities de extensie .i386. Voordat u de 32-bits versies installeert, moet u controleren of de nieuwste 64-bits versies zijn geïnstalleerd. Als de 64-bits versie van een bibliotheek ouder is dan de 32-bits versie die wordt geïnstalleerd, wordt een fout als volgt weergegeven:

0mError: Protected multilib versions: libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [ 0mError: Multilib versieproblemen gevonden.]

## Eerste installatie

Gebruik in Red Hat Enterprise Linux de YUM-update Modifier (YUM) om te installeren, zoals hieronder wordt weergegeven:

1. yum install expat.i686
2. yum install fontconfig.i686
3. yum install freetype.i686
4. yum install glibc.i686
5. yum installeert libcurl.i686
6. yum installeert libICE.i686
7. yum installeert libicu.i686
8. yum install libicu
9. yum installeert libSM.i686
10. yum install libuuid.i686
11. yum installeert libX11.i686
12. yum installeert libXau.i686
13. yum installeert libxcb.i686
14. yum installeert libXext.i686
15. yum installeert libXinerama.i686
16. yum installeert libXrandr.i686
17. yum installeert libXrender.i686
18. yum install nss-softokn-freebl.i686
19. yum install zlib.i686

## Symlinks

Bovendien moet u libcurl.so, libcrypto.so, en libssl.so symlinks tot stand brengen die aan de recentste versies met 32 bits van de bibliotheken libcurl, libcrypto, en libssl richten. U kunt de bestanden vinden in /usr/lib/
ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so
ln-s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so
ln-s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so

## Updates voor bestaand systeem

Er kunnen conflicten optreden tussen de x86_64- en i686-architecturen tijdens updates, zoals:
Fout: fout bij transactiecontrole:
bestand /lib/ld-2.28.so uit de installatie van glibc-2.28-72.el8.i686 veroorzaakt een conflict met het bestand uit het pakket glibc32-2.28-42.1.el8.x86_64

Als u dit doet, moet u eerst de installatie van het betreffende pakket ongedaan maken, zoals in dit geval:
yum remove glibc32-2.28-42.1.el8.x86_64

Alles gezegd en gedaan, wilt u de x86_64 en i686 versies precies het zelfde zijn, zoals bijvoorbeeld van deze output aan het bevel:
yum info glibc

Laatste controle van de meta-gegevensvervaldatum: 0 :41: 33 geleden op Zat 18 jan 2020 11 :37: 08 AM EST.
Geïnstalleerde pakketten
Naam: glibc
Versie: 2.28
Release : 72.el8
Architectuur : i686
Grootte: 13 M
Source : glibc-2.28-72.el8.src.rpm
Repository: @System
Van repo : BaseOS
Summary: De GNU libc-bibliotheken
URL: http://www.gnu.org/software/glibc/
Licentie: LGPLv2+ en LGPLv2+ met uitzonderingen en GPLv2+ en GPLv2+ met uitzonderingen en BSD en Inner-Net en ISC en Public Domain en GFDL
Beschrijving: Het glibc-pakket bevat standaardbibliotheken die worden gebruikt door : meerdere programma&#39;s op het systeem. Om schijfruimte en geheugen te besparen en de upgrade te vereenvoudigen, wordt algemene systeemcode opgeslagen op één locatie en gedeeld tussen programma&#39;s. This particular package : contains the main sets of shared libraries: the standard C : library and the standard math library. Zonder deze twee bibliotheken werkt a: Linux niet.

Naam: glibc
Versie: 2.28
Release : 72.el8
Architectuur : x86_64
Grootte: 15 M
Source : glibc-2.28-72.el8.src.rpm
Repository: @System
Van repo : BaseOS
Summary: De GNU libc-bibliotheken
URL: http://www.gnu.org/software/glibc/
Licentie: LGPLv2+ en LGPLv2+ met uitzonderingen en GPLv2+ en GPLv2+ met uitzonderingen en BSD en Inner-Net en ISC en Public Domain en GFDL
Beschrijving: Het glibc-pakket bevat standaardbibliotheken die worden gebruikt door : meerdere programma&#39;s op het systeem. Om schijfruimte en geheugen te besparen en de upgrade te vereenvoudigen, wordt algemene systeemcode opgeslagen op één locatie en gedeeld tussen programma&#39;s. This particular package : contains the main sets of shared libraries: the standard C : library and the standard math library. Zonder deze twee bibliotheken werkt a: Linux niet.

## Enkele handige yum-opdrachten

yum-lijst geïnstalleerd
yum onderzoek [ part_of_package_name ]
yum wat [ package_name ] verstrekt
yum installeert [ package_name ]
yum herinstalleert [ package_name ]
yum info [ package_name ]
yum deplist [ package_name ]
yum verwijdert [ package_name ]
yum controle-update [ package_name ]
yum update [ package_name ]
