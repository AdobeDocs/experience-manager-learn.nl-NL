---
title: Transformaties met AEM Forms
seo-title: Adaptieve formuliergegevens samenvoegen met Acrobat
description: Deel 1 van de integratie van Acrobat met AEM Forms. Een adaptief formulier maken met Acrobat en de gegevens samenvoegen om een PDF te verkrijgen.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 0%

---


# Acroform maken

Acroformulieren zijn formulieren die zijn gemaakt met Acrobat. U kunt een geheel nieuw formulier maken met Acrobat of een bestaand formulier nemen dat is gemaakt in Microsoft Word en het met Acrobat converteren naar Acrobat. De volgende stappen moeten worden uitgevoerd om een formulier dat in Microsoft Word is gemaakt, om te zetten in Acrobat.

* Woorddocument openen met Acrobat
* Gebruik het gereedschap Acrobat voorbereiden om de formuliervelden op het formulier te identificeren.
* Sla de PDF op. Zorg ervoor dat de bestandsnaam geen spaties bevat.


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=9&learn=on)

>[!NOTE]
>
>Als u het invulbare formulier wilt verzenden voor ondertekening met Adobe Sign, geef de velden een overeenkomstige naam. U kunt bijvoorbeeld een veld **Sig_es_:signer1:signature** een naam geven. Dit is de syntaxis die Adobe Sign begrijpt.

>[!NOTE]
>
>Als u een XFA-document verzendt, moet u het document afvlakken en moeten de Adobe Sign-handtekeningcodes aanwezig zijn als statische tekst in het document.

[Adobe Sign Text Tags Document](https://helpx.adobe.com/sign/using/text-tag.html)

>[!NOTE]
>
>Zorg ervoor dat de bestandsnaam van het acroformulier geen spaties bevat. In de huidige voorbeeldcode worden spaties niet afgehandeld.
>
>De namen van formuliervelden mogen alleen het volgende bevatten:
>
>* enkele spatie
>* enkel onderstrepingsteken
>* alfanumerieke tekens

