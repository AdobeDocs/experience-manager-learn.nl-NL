---
title: Kanaaldocumenten afdrukken met gecontroleerde map
seo-title: Kanaaldocumenten afdrukken met gecontroleerde map
description: Dit is onderdeel van een zelfstudie met meerdere stappen voor het maken van uw eerste interactieve communicatiedocument voor het afdrukkanaal. In dit deel genereren we afdrukkanaaldocumenten aan de hand van het mechanisme voor gecontroleerde mappen.
seo-description: Dit is onderdeel van een zelfstudie met meerdere stappen voor het maken van uw eerste interactieve communicatiedocument voor het afdrukkanaal. In dit deel genereren we afdrukkanaaldocumenten aan de hand van het mechanisme voor gecontroleerde mappen.
uuid: 9e39f4e3-1053-4839-9338-09961ac54f81
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---


# Kanaaldocumenten afdrukken met gecontroleerde map

In dit deel genereren we afdrukkanaaldocumenten aan de hand van het mechanisme voor gecontroleerde mappen.

Nadat u het afdrukkanaaldocument hebt gemaakt en getest, hebt u een mechanisme nodig om dit document in batchmodus of op verzoek te genereren. Dit soort documenten wordt doorgaans gegenereerd in de batchmodus en het meest gebruikte mechanisme is het gebruik van controlemappen.

Wanneer u een gecontroleerde omslag in AEM vormt, associeert u een manuscript ECMA of Java code die wordt uitgevoerd wanneer een dossier in de gelete op omslag wordt gelaten vallen. In dit artikel richten we ons op het ECMA-script dat afdrukkanaaldocumenten genereert en opslaat in het bestandssysteem.

De gecontroleerde omslagconfiguratie en het manuscript ECMA maken deel uit van de activa u aan het [begin van dit leerprogramma invoerde](introduction.md)

Het invoerbestand dat in de controlemap wordt neergezet, heeft de volgende structuur. Met ECMA-script worden de accountnummers gelezen en wordt een document met het afdrukkanaal voor elk van deze accounts gegenereerd.

Raadpleeg dit artikel voor meer informatie over het ECMA-script voor het genereren van documenten [](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md)

```xml
<accountnumbers>
 <accountnumber>509840</accountnumber>
 <accountnumber>948576</accountnumber>
 <accountnumber>398762</accountnumber>
 <accountnumber>291723</accountnumber>
 <accountnumber>291724</accountnumber>
 <accountnumber>291725</accountnumber>
 <accountnumber>291726</accountnumber>
 <accountnumber>291727</accountnumber>
</accountnumbers>
```

Voer de onderstaande stappen uit om een document met een afdrukkanaal te genereren aan de hand van het mechanisme voor gecontroleerde mappen:

* [Voer de in dit document vermelde stappen uit](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* Meld u aan bij crx en ga naar /etc/fd/watchfolder/scripts/PrintPDF.ecma

* Zorg ervoor dat het pad naar interactiveCommunicationsDocument naar het juiste document verwijst dat u wilt afdrukken.(regel 1)
* Noteer de saveLocation(Line 2). U kunt deze naar wens wijzigen.
* Zorg ervoor dat de invoerparameter voor het formuliergegevensmodel is gebonden aan het aanvraagkenmerk en dat de bindingswaarde is ingesteld op &quot;accountnummer&quot;. Raadpleeg de onderstaande schermafbeelding.
   ![verzoek](assets/requestattributeprintchannel.gif)

* Het bestand accountnumbers.xml maken met de volgende inhoud

```xml
<accountnumbers>
<accountnumber>1</accountnumber>
<accountnumber>100</accountnumber>
<accountnumber>101</accountnumber>
<accountnumber>1009</accountnumber>
<accountnumber>10009</accountnumber>
<accountnumber>11990</accountnumber>
</accountnumbers>
```

* Zet het XML-bestand neer in C:\RenderPrintChannel\input

* Controleer de PDF-bestanden op de opslaglocatie die in het ECMA-script is opgegeven.




