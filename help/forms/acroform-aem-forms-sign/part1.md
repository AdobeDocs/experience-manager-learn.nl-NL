---
title: Transformaties met AEM Forms
description: Deel 1 van de integratie van Acrobat met AEM Forms. Een adaptief formulier maken met Acrobat en de gegevens samenvoegen om een PDF te verkrijgen.
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 150
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '218'
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
>Als u het invulbare formulier voor ondertekening met Acrobat Sign wilt verzenden, moet u de velden een overeenkomstige naam geven. U kunt bijvoorbeeld een veld een naam geven **S_ijl_:signer1:handtekening**. Dit is de syntaxis die Acrobat Sign begrijpt.

>[!NOTE]
>
>Als u een XFA-document verzendt, moet u het document afvlakken en moeten de Acrobat Sign-handtekeningcodes aanwezig zijn als statische tekst in het document.

[Acrobat Sign Text Tags Document](https://helpx.adobe.com/sign/using/text-tag.html)

>[!NOTE]
>
>Zorg ervoor dat de bestandsnaam van het acroformulier geen spaties bevat. In de huidige voorbeeldcode worden spaties niet afgehandeld.
>
>De namen van formuliervelden mogen alleen het volgende bevatten:
>
>* enkele spatie
>* enkel onderstrepingsteken
>* alfanumerieke tekens
