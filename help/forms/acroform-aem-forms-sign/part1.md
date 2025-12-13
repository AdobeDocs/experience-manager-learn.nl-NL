---
title: Transformaties met AEM Forms
description: Deel 1 van de integratie van Acrobat met AEM Forms. Een adaptief formulier maken met Acrobat en de gegevens samenvoegen voor een PDF.
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.5
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 144
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---


# Acroform maken

Acroformulieren zijn formulieren die zijn gemaakt met Acrobat. U kunt een geheel nieuw formulier maken met Acrobat of een bestaand formulier nemen dat is gemaakt in Microsoft Word en het met Acrobat converteren naar Acrobat. De volgende stappen moeten worden uitgevoerd om een formulier dat in Microsoft Word is gemaakt, om te zetten in Acrobat.

* Woorddocument openen met Acrobat
* Gebruik het gereedschap Acrobat voorbereiden om de formuliervelden op het formulier te identificeren.
* Sla de PDF op. Zorg ervoor dat de bestandsnaam geen spaties bevat.


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=12&learn=on)

>[!NOTE]
>
>Als u het invulbare formulier voor ondertekening met Acrobat Sign wilt verzenden, moet u de velden een overeenkomstige naam geven. U kunt bijvoorbeeld een veld een naam geven **`Sig_es_:signer1:signature`** . Dit is de syntaxis die Acrobat Sign begrijpt.

>[!NOTE]
>
>Als u een XFA-document verzendt, moet u het document afvlakken en moeten de Acrobat Sign-handtekeningcodes aanwezig zijn als statische tekst in het document.

[ het Document van de Markeringen van de Tekst van Acrobat Sign ](https://helpx.adobe.com/sign/using/text-tag.html)

>[!NOTE]
>
>Zorg ervoor dat de bestandsnaam van het acroformulier geen spaties bevat. In de huidige voorbeeldcode worden spaties niet afgehandeld.
>
>De namen van formuliervelden mogen alleen het volgende bevatten:
>
>* enkele spatie
>* enkel onderstrepingsteken
>* alfanumerieke tekens
