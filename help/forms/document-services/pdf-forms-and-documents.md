---
title: De verschillende typen PDF forms en documenten begrijpen
description: PDF is in feite een reeks bestandsindelingen. In dit artikel worden de typen PDF's beschreven die belangrijk en relevant zijn voor formulierontwikkelaars.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: 6.3,6.4, 6.5
feature: PDF Generator
kt: 7071
topic: Ontwikkeling
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '1696'
ht-degree: 0%

---


# PDF

PDF (Portable Document Format) is in feite een reeks bestandsindelingen en in dit artikel worden de bestandsindelingen beschreven die het meest relevant zijn voor formulierontwikkelaars. Veel van de technische details en standaarden van verschillende PDF-typen worden steeds verder ontwikkeld en gewijzigd. Sommige van deze formaten en specificaties zijn ISO-normen (International Organisation for Standardization) en sommige zijn specifieke intellectuele eigendom van Adobe.

In dit artikel wordt uitgelegd hoe u verschillende typen PDF&#39;s kunt maken. Het helpt u begrijpen hoe en waarom om elk te gebruiken. Al deze typen werken het beste in de Premier Client Tool voor het weergeven en werken met PDF&#39;s—Adobe Acrobat DC.

Hieronder ziet u een voorbeeld van een PDF/A-bestand in Acrobat DC.

![Pdfa](assets/pdfa-file-in-acrobat.png)

Voorbeeldbestanden kunnen [hier worden gedownload](assets/pdf-file-types.zip)

## XML Forms Architecture PDF

Adobe gebruikt de term PDF-formulier om te verwijzen naar de interactieve en dynamische Forms die u maakt met AEM Forms Designer. De Forms en de bestanden die u met Designer maakt, zijn gebaseerd op de XML Forms Architecture (XFA) van Adobe. In veel opzichten is de XFA PDF-bestandsindeling dichter bij een HTML-bestand dan bij een traditioneel PDF-bestand. De volgende code toont bijvoorbeeld hoe een eenvoudig tekstobject eruitziet in een XFA PDF-bestand.

![Tekstveld](assets/text-field.JPG)

XFA Forms is gebaseerd op XML. Met deze goed gestructureerde en flexibele indeling kan een AEM Forms Server uw Designer-bestanden transformeren in verschillende indelingen, zoals PDF, PDF/A en HTML. U kunt de volledige XML-structuur van uw Forms in Designer zien door het tabblad XML-bron van de Indelingseditor te selecteren. U kunt zowel statische als dynamische XFA Forms maken in AEM Forms Designer.

## Statische PDF

De statische lay-out van PDF forms XFA verandert nooit bij runtime, maar zij kunnen voor de gebruiker interactief zijn. Hier volgen enkele voordelen van statische XFA-PDF forms:

* De statische lay-out van PDF forms XFA verandert nooit bij runtime, maar zij kunnen voor de gebruiker interactief zijn.
* Statische Forms biedt ondersteuning voor Acrobat&#39;s gereedschappen voor opmerkingen en markeringen.
* Met statische Forms kunt u Acrobat-opmerkingen importeren en exporteren.
* Statische Forms biedt ondersteuning voor subsets van lettertypen. Dit is een techniek die op een AEM Forms-server kan worden uitgevoerd.
* Statische Forms kan worden gerenderd met de ingebouwde PDF-viewer die bij moderne browsers wordt geleverd.

>[!NOTE]
>
> U kunt statische PDF&#39;s maken met AEM Forms Designer door de XDP op te slaan als een Adobe statisch PDF-formulier

## PDF-indelingen

PDF (Portable Document Format) is in feite een reeks bestandsindelingen en in dit artikel worden de bestandsindelingen beschreven die het meest relevant zijn voor formulierontwikkelaars. Veel van de technische details en standaarden van verschillende PDF-typen worden steeds verder ontwikkeld en gewijzigd. Sommige van deze formaten en specificaties zijn ISO-normen (International Organisation for Standardization) en sommige zijn specifieke intellectuele eigendom van Adobe.

In dit artikel wordt uitgelegd hoe u verschillende typen PDF&#39;s kunt maken. Het zal u helpen begrijpen hoe en waarom om elk te gebruiken. Al deze typen werken het beste in de Premier Client Tool voor het weergeven en werken met PDF&#39;s—Adobe Acrobat DC.

Dit is een voorbeeld van een PDF/A-bestand in Acrobat DC.

![pdfa](assets/pdfa-file-in-acrobat.png)

Voorbeeldbestanden kunnen [hier worden gedownload](assets/pdf-file-types.zip)

### XFA PDF

Adobe gebruikt de term PDF-formulier om te verwijzen naar de interactieve en dynamische formulieren die u maakt met AEM Forms Designer. Het is belangrijk om te weten dat er een ander type PDF-formulier is, een zogenaamd Acrobat-formulier, dat afwijkt van de PDF forms die u maakt in AEM Forms Designer. De formulieren en bestanden die u met Designer maakt, zijn gebaseerd op de XML Forms Architecture (XFA) van Adobe. In veel opzichten is de XFA PDF-bestandsindeling dichter bij een HTML-bestand dan bij een traditioneel PDF-bestand. De volgende code toont bijvoorbeeld hoe een eenvoudig tekstobject eruitziet in een XFA PDF-bestand.

![tekstveld](assets/text-field.JPG)

Zoals u ziet, zijn XFA-formulieren gebaseerd op XML. Met deze goed gestructureerde en flexibele indeling kan een AEM Forms Server uw Designer-bestanden transformeren in verschillende indelingen, zoals traditionele PDF-, PDF/A- en HTML-indelingen. U kunt de volledige XML-structuur van uw formulieren bekijken in Designer door het tabblad XML-bron van de Indelingseditor te selecteren. U kunt zowel statische als dynamische XFA-formulieren maken in AEM Forms Designer.

### Statische PDF

De statische PDF forms XFA veranderen niet hun lay-out bij runtime, maar zij kunnen voor de gebruiker interactief zijn. Hier volgen enkele voordelen van statische XFA-PDF forms:

* De statische PDF forms XFA veranderen niet hun lay-out bij runtime, maar zij kunnen voor de gebruiker interactief zijn.
* Statische formulieren ondersteunen de gereedschappen Opmerkingen en Markeringen van Acrobat.
* Met statische formulieren kunt u Acrobat-opmerkingen importeren en exporteren.
* Statische formulieren ondersteunen subsets van lettertypen. Dit is een techniek die op een AEM Forms-server kan worden uitgevoerd.
* Statische formulieren kunnen worden weergegeven met de ingebouwde PDF-viewer die bij moderne browsers wordt geleverd.

>[!NOTE]
> U kunt statische PDF&#39;s maken met AEM Forms Designer door de XDP op te slaan als Adobe statisch PDF-formulier

### Dynamische Forms

Dynamische XFA-PDF&#39;s kunnen hun indeling tijdens runtime wijzigen, zodat de functies voor opmerkingen en markeringen niet worden ondersteund. Dynamische XFA PDF&#39;s bieden echter wel de volgende voordelen:

* Dynamische formulieren ondersteunen clientscripts die de indeling en paginering van het formulier wijzigen. Zo wordt Purchase Order.xdp uitgebreid en gepagineerd om ruimte te maken voor een eindeloze hoeveelheid gegevens als u deze opslaat als een dynamisch formulier
* Dynamische formulieren ondersteunen alle eigenschappen van het formulier tijdens runtime, terwijl statische formulieren alleen een subset ondersteunen

>[!NOTE]
>
> U kunt dynamische PDF&#39;s maken met AEM Forms Designer door de XDP op te slaan als dynamisch XML-formulier voor Adobe-XML

>[!NOTE]
>
> Dynamische formulieren kunnen niet worden gegenereerd met de ingebouwde PDF-viewers van moderne browsers.

### PDF-bestand (traditionele PDF)

Een gecertificeerd document biedt ontvangers van PDF-documenten en Forms extra garanties ten aanzien van authenticiteit en integriteit.

De populairste en meest uitgebreide PDF-indeling is het traditionele PDF-bestand. U kunt een traditioneel PDF-bestand op verschillende manieren maken, bijvoorbeeld met Acrobat en een groot aantal hulpprogramma&#39;s van derden. Acrobat biedt de volgende manieren om traditionele PDF-bestanden te maken. Als u Acrobat niet hebt geïnstalleerd, ziet u deze opties mogelijk niet op uw computer.

* Door de afdrukstroom van een bureaubladtoepassing vast te leggen: Kies de opdracht Afdrukken van een ontwerptoepassing en selecteer het Adobe PDF-printerpictogram. In plaats van een afgedrukte kopie van het document hebt u een PDF-bestand van het document gemaakt
* Met de Acrobat PDFMaker-plug-in in Microsoft Office-toepassingen: Wanneer u Acrobat installeert, wordt een Adobe PDF-menu toegevoegd aan Microsoft Office-toepassingen en een pictogram aan het Office-lint. U kunt deze toegevoegde functies gebruiken om PDF-bestanden rechtstreeks in Microsoft Office te maken
* Met Acrobat Distiller kunt u PostScript- en EPS-bestanden (Encapsulated PostScript) converteren naar PDF&#39;s: Distiller wordt doorgaans gebruikt voor drukpublicaties en andere workflows die een conversie van de PostScript-indeling naar de PDF-indeling vereisen
* Onder de kap wijkt een traditionele PDF sterk af van een XFA PDF. De PDF heeft niet dezelfde XML-structuur en omdat deze is gemaakt door het vastleggen van de afdrukstroom van een bestand, is een traditionele PDF een statisch bestand met het kenmerk Alleen-lezen.

Een gecertificeerd document biedt ontvangers van PDF-documenten en formulieren extra garanties ten aanzien van authenticiteit en integriteit.

### Acrovormen

Acroformen zijn de oudere interactieve vormtechnologie van Adobe; ze dateren van Acrobat versie 3. Adobe biedt de [Acrobat Forms API Reference](assets/FormsAPIReference.pdf) van mei 2003 om de technische details voor deze technologie te verstrekken. Acrovormen zijn een combinatie van de
de volgende items:

* Een traditionele PDF die de statische indeling en afbeeldingen van het formulier definieert.
* Interactieve formuliervelden die op de voorgrond zijn bevestigd met de formuliergereedschappen van het Adobe Acrobat-programma. Deze formuliergereedschappen vormen een kleine subset van wat beschikbaar is in AEM Forms Designer.

### PDF/A (PDF&#39;s voor archief)

PDF/A (PDF voor archieven) bouwt voort op de voordelen van de documentopslag van traditionele PDF&#39;s met veel specifieke details die archivering op lange termijn verbeteren. De traditionele PDF-bestandsindeling biedt veel voordelen voor langdurige documentopslag. Het compacte karakter van PDF vergemakkelijkt gemakkelijke overdracht en bewaart ruimte, en zijn goed gestructureerde aard laat krachtige indexering en onderzoeksmogelijkheden toe. Traditionele PDF biedt uitgebreide ondersteuning voor metagegevens en PDF biedt al een lange periode ondersteuning voor verschillende computeromgevingen.

PDF/A is net als PDF een ISO-standaardspecificatie. Het werd ontwikkeld door een task force die AIIM (Association for Information and Image Management), NPES (National Printing Equipment Association) en de Administratieve Office van de Amerikaanse rechtbanken omvatte. Omdat de PDF/A-specificatie als doel heeft een archiefindeling voor de lange termijn te bieden, worden veel PDF-functies weggelaten, zodat de bestanden op zichzelf kunnen worden opgeslagen. Hieronder volgen enkele belangrijke punten in de specificatie die de reproduceerbaarheid op lange termijn van het PDF/A-bestand verbeteren:

* Alle inhoud moet in het bestand staan en er kunnen geen afhankelijkheden zijn van externe bronnen, zoals hyperlinks, lettertypen of softwareprogramma&#39;s.
* Alle lettertypen moeten worden ingesloten en het moeten lettertypen zijn met een licentie voor onbeperkt gebruik voor elektronische documenten.
* JavaScript is niet toegestaan
* Transparantie is niet toegestaan
* Versleuteling is niet toegestaan
* Audio- en video-inhoud is niet toegestaan
* Kleurruimten moeten op een apparaatonafhankelijke manier worden gedefinieerd
* Alle metagegevens moeten aan bepaalde normen voldoen

### Een PDF/A-bestand weergeven

Twee bestanden in de voorbeeldbestanden zijn gemaakt van hetzelfde Microsoft Word-bestand. Het ene bestand is gemaakt als een traditioneel PDF-bestand en het andere als een PDF/A-bestand. Open deze twee bestanden in Acrobat Professional:

* simpleWordFile.pdf
* simpleWordFilePDFA.pdf

Hoewel de documenten er hetzelfde uitzien, wordt het PDF/A-bestand geopend met een blauwe balk aan de bovenzijde, ten teken dat u dit document in PDF/A-modus weergeeft. Deze blauwe balk is de documentberichtenbalk van Acrobat. Deze wordt weergegeven wanneer u bepaalde typen PDF-bestanden opent.

![PDF-img](assets/pdfa-message.png)

De documentberichtenbalk bevat instructies en mogelijk knoppen waarmee u een taak kunt uitvoeren. De PDF heeft een kleurcodering en u ziet de blauwe kleur wanneer u speciale typen PDF&#39;s (zoals dit PDF/A-bestand) en gecertificeerde en digitaal ondertekende PDF&#39;s opent. De balk verandert van paars voor PDF forms in geel als u deelneemt aan een PDF-revisie.

>[!NOTE]
>
> Als u op Bewerken inschakelen klikt, haalt u dit document uit de PDF/A-compatibiliteit.




