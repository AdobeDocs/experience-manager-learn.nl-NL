---
title: De verschillende typen PDF forms en documenten begrijpen
description: PDF is eigenlijk een familie van dossierformaten, en dit artikel beschrijft de types van PDF die belangrijk en relevant voor vormontwikkelaars zijn.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: 6.3,6.4, 6.5
feature: PDF Generator
kt: 7071
topic: Development
exl-id: ffa9d243-37e5-420c-91dc-86c73a824083
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
workflow-type: tm+mt
source-wordcount: '1277'
ht-degree: 0%

---

# PDF

PDF (Portable Document Format) is in feite een reeks bestandsindelingen en in dit artikel worden de bestandsindelingen beschreven die het meest relevant zijn voor formulierontwikkelaars. Veel van de technische details en normen van verschillende soorten PDF evolueren en veranderen. Sommige van deze formaten en specificaties zijn ISO-normen (International Organisation for Standardization) en sommige zijn specifieke intellectuele eigendom van Adobe.

In dit artikel wordt uitgelegd hoe u verschillende typen PDF kunt maken. Het helpt u begrijpen hoe en waarom om elk te gebruiken. Al deze typen werken het beste in de Premier Client Tool voor weergave en het werken met PDF—Adobe Acrobat DC.

Hieronder ziet u een voorbeeld van een PDF/A-bestand in Acrobat DC.

![Pdfa](assets/pdfa-file-in-acrobat.png)

Voorbeeldbestanden kunnen [hier gedownload](assets/pdf-file-types.zip)

## XML Forms Architecture PDF (XFA PDF)

Adobe gebruikt de term XFA-PDF-formulier om te verwijzen naar de interactieve en dynamische Forms die u maakt met AEM Forms Designer. De Forms en de bestanden die u met Designer maakt, zijn gebaseerd op de XML Forms Architecture (XFA) van Adobe. In veel opzichten is de XFA PDF-bestandsindeling dichter bij een HTML-bestand dan bij een traditioneel PDF-bestand. De volgende code toont bijvoorbeeld hoe een eenvoudig tekstobject eruitziet in een XFA-PDF-bestand.

![Tekstveld](assets/text-field.JPG)

XFA Forms is gebaseerd op XML. Met deze goed gestructureerde en flexibele indeling kan een AEM Forms Server uw Designer-bestanden omzetten in verschillende indelingen, zoals traditionele PDF, PDF/A en HTML. U kunt de volledige XML-structuur van uw Forms in Designer zien door het tabblad XML-bron van de Indelingseditor te selecteren. U kunt zowel statische als dynamische XFA Forms maken in AEM Forms Designer.

## Statische PDF

De statische lay-out van PDF forms XFA verandert nooit bij runtime, maar zij kunnen voor de gebruiker interactief zijn. Hier volgen enkele voordelen van statische XFA-PDF forms:

* De statische lay-out van PDF forms XFA verandert nooit bij runtime, maar zij kunnen voor de gebruiker interactief zijn.
* Statische Forms biedt ondersteuning voor Acrobat&#39;s gereedschappen voor opmerkingen en markeringen.
* Met statische Forms kunt u Acrobat-opmerkingen importeren en exporteren.
* Statische Forms biedt ondersteuning voor subsets van lettertypen. Dit is een techniek die op een AEM Forms-server kan worden uitgevoerd.
* Statische Forms kan worden gerenderd met de ingebouwde PDF-viewer die bij moderne browsers wordt geleverd.

>[!NOTE]
>
> U kunt statische PDF maken met AEM Forms Designer door de XDP op te slaan als Adobe statisch PDF formulier



### Dynamische Forms

Dynamische XFA-PDF kunnen hun lay-out tijdens runtime wijzigen, zodat de functies voor opmerkingen en markeringen niet worden ondersteund. Dynamische XFA-PDF bieden echter wel de volgende voordelen:

* Dynamische formulieren ondersteunen clientscripts die de indeling en paginering van het formulier wijzigen. Zo wordt Purchase Order.xdp uitgebreid en gepagineerd om ruimte te maken voor een eindeloze hoeveelheid gegevens als u deze opslaat als een dynamisch formulier
* Dynamische formulieren ondersteunen alle eigenschappen van het formulier tijdens runtime, terwijl statische formulieren alleen een subset ondersteunen

>[!NOTE]
>
> U kunt dynamische PDF&#39;s maken met AEM Forms Designer door de XDP op te slaan als dynamisch XML-formulier voor Adobe-XML

>[!NOTE]
>
> Dynamische formulieren kunnen niet worden gegenereerd met de ingebouwde PDF-viewers van moderne browsers.

### PDF-bestand (traditionele PDF)

Een gecertificeerd document biedt PDF-documenten en Forms-ontvangers extra garanties ten aanzien van authenticiteit en integriteit.

De populairste en wijdverbreide PDF-indeling is het traditionele PDF-bestand. Er zijn veel manieren om een traditioneel PDF-bestand te maken, bijvoorbeeld met Acrobat en vele andere tools. Acrobat biedt de volgende manieren om traditionele PDF-bestanden te maken. Als u Acrobat niet hebt geïnstalleerd, ziet u deze opties mogelijk niet op uw computer.

* Door de afdrukstroom van een bureaubladtoepassing vast te leggen: Kies de opdracht Afdrukken van een ontwerptoepassing en selecteer het Adobe PDF-printerpictogram. In plaats van een afgedrukte kopie van het document hebt u een PDF-bestand van het document gemaakt
* Met de Acrobat PDFMaker-plug-in in Microsoft Office-toepassingen: Wanneer u Acrobat installeert, wordt een Adobe PDF-menu toegevoegd aan Microsoft Office-toepassingen en een pictogram aan het Office-lint. U kunt deze toegevoegde eigenschappen gebruiken om PDF dossiers direct tot stand te brengen in Microsoft Office
* Met Acrobat Distiller kunt u Postscript- en Encapsulated PostScript-bestanden (EPS) omzetten in PDF: Distiller wordt doorgaans gebruikt in gedrukte publicaties en andere workflows die een omzetting van de PostScript-indeling naar de PDF-indeling vereisen
* Onder de kap is een traditionele PDF heel anders dan een XFA-PDF. De klasse heeft niet dezelfde XML-structuur en aangezien deze is gemaakt door het vastleggen van de afdrukstroom van een bestand, is een traditionele PDF een statisch bestand met het kenmerk Alleen-lezen.

Een gecertificeerd document biedt ontvangers van PDF-documenten en formulieren extra garanties ten aanzien van authenticiteit en integriteit.

### Acrovormen

Acroformen zijn de oudere interactieve vormtechnologie van Adobe; ze dateren van Acrobat versie 3. Adobe biedt de [Acrobat Forms API-naslag](assets/FormsAPIReference.pdf), gedateerd mei 2003, om de technische details van deze technologie te verstrekken. Acrovormen zijn een combinatie van de volgende items:

* Een traditionele PDF die de statische indeling en afbeeldingen van het formulier definieert.
* Interactieve formuliervelden die op de voorgrond zijn bevestigd met de formuliergereedschappen van het Adobe Acrobat-programma. Deze formuliergereedschappen vormen een kleine subset van wat beschikbaar is in AEM Forms Designer.

### PDF/A (PDF voor archivering)

PDF/A (PDF voor archieven) bouwt voort op de voordelen van de documentopslag van traditionele PDF met vele specifieke details die het archiveren op lange termijn verbeteren. De traditionele bestandsindeling PDF biedt veel voordelen voor langdurige documentopslag. Het compacte karakter van PDF vergemakkelijkt gemakkelijke overdracht en bewaart ruimte, en zijn goed gestructureerde aard laat krachtige indexering en onderzoeksmogelijkheden toe. Traditionele PDF biedt uitgebreide ondersteuning voor metagegevens en PDF heeft een lange geschiedenis van ondersteuning voor verschillende computeromgevingen.

Net als PDF is PDF/A een ISO-standaardspecificatie. Het werd ontwikkeld door een task force die AIIM (Association for Information and Image Management), NPES (National Printing Equipment Association) en de Administratieve Office van de Amerikaanse rechtbanken omvatte. Aangezien het doel van de PDF/A specificatie een archiefformaat op lange termijn moet verstrekken, worden vele eigenschappen van PDF weggelaten zodat kunnen de dossiers met alle accomodatie zijn. Hieronder volgen enkele belangrijke punten in de specificatie die de reproduceerbaarheid op lange termijn van het PDF/A-bestand verbeteren:

* Alle inhoud moet in het bestand staan en er kunnen geen afhankelijkheden zijn van externe bronnen, zoals hyperlinks, lettertypen of softwareprogramma&#39;s.
* Alle lettertypen moeten worden ingesloten en het moeten lettertypen zijn met een licentie voor onbeperkt gebruik voor elektronische documenten.
* JavaScript is niet toegestaan
* Transparantie is niet toegestaan
* Versleuteling is niet toegestaan
* Audio- en video-inhoud is niet toegestaan
* Kleurruimten moeten op een apparaatonafhankelijke manier worden gedefinieerd
* Alle metagegevens moeten aan bepaalde normen voldoen

### Een PDF/A-bestand weergeven

Twee bestanden in de voorbeeldbestanden zijn gemaakt van hetzelfde Microsoft Word-bestand. De ene is gemaakt als een traditionele PDF en de andere als een PDF/A-bestand. Open deze twee bestanden in Acrobat Professional:

* simpleWordFile.pdf
* simpleWordFilePDFA.pdf

Hoewel de documenten er hetzelfde uitzien, wordt het PDF/A-bestand geopend met een blauwe balk boven in het scherm. Dit geeft aan dat u dit document in de modus PDF/A weergeeft. Deze blauwe balk is de documentberichtenbalk van Acrobat. Deze wordt weergegeven wanneer u bepaalde typen PDF-bestanden opent.

![PDF-img](assets/pdfa-message.png)

De documentberichtenbalk bevat instructies en mogelijk knoppen waarmee u een taak kunt uitvoeren. De kleur is gecodeerd en u ziet de blauwe kleur wanneer u speciale typen PDF (zoals dit PDF/A-bestand) en gecertificeerde en digitaal ondertekende PDF opent. De balk verandert van paars voor PDF forms in geel als u deelneemt aan een PDF-revisie.

>[!NOTE]
>
> Als u op Bewerken inschakelen klikt, haalt u dit document uit de PDF/A-compatibiliteit.
