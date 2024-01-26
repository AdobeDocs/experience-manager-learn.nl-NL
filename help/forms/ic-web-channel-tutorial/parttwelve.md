---
title: De levering van een webkanaaldocument instellen
description: Dit is het laatste deel van een zelfstudie met meerdere stappen voor het maken van uw eerste interactieve communicatiedocument. In dit deel bekijken we de levering van een webkanaaldocument via e-mail.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 1a7cf095-c5d8-4d92-a018-883cda76fe70
topic: Development
role: Developer
level: Beginner
exl-id: 510d1782-59b9-41a6-a071-a16170f2cd06
duration: 90
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 0%

---

# De levering van een webkanaaldocument instellen {#setting-up-the-delivery-of-web-channel-document}


In dit deel bekijken we de levering van een webkanaaldocument via e-mail.

Nadat u het interactieve document met webkanalen hebt gedefinieerd en getest, hebt u een leveringsmechanisme nodig om het webkanaaldocument aan de ontvanger te leveren.

Als u e-mail wilt gebruiken als een leveringsmechanisme voor ons webkanaaldocument, moet u het gegevensmodel van het formulier enigszins wijzigen.

[Meer informatie over webkanaallevering via e-mail](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

Aanmelden bij AEM Forms.

* Ga naar Forms ->Gegevensintegratie

* Open het gegevensmodel RetirementAccountStatement in de bewerkingsmodus.

* Selecteer het object balances en klik op de knop Bewerken.

* Selecteer het pictogram &#39;Potlood&#39; om het id-argument te openen in de bewerkingsmodus.

* Wijzig de binding in &quot;RequestAttribute&quot;.

* Stel het accountnummer in de bindingswaarde in, zoals hieronder wordt weergegeven.

* Op deze manier geven we het accountnummer door via het aanvraagkenmerk aan het formuliergegevensmodel.

* Zorg ervoor dat u de wijzigingen opslaat.
  ![fdm](assets/requestattribute.gif)

## E-maillevering van webkanaaldocument testen {#test-email-delivery-of-web-channel-document}

* [De voorbeeldbestanden installeren met pakketbeheer](assets/webchanneldelivery.zip)
* [Aanmelden bij crx](http://localhost:4502/crx/de/index.jsp#)

* Naar /home/users navigeren

* Zoek naar admin gebruiker onder de knoop van de gebruiker.

* Selecteer het profielknooppunt van de beheerder.

* Maak een eigenschap met de naam &quot;accountnummer&quot;. Controleer of het eigenschapstype een tekenreeks is.

* Stel de waarde van deze eigenschap accountnumber in op &quot;3059827&quot;. U kunt deze waarde op elk willekeurig getal instellen zoals u wilt.

* [Open getad.html](http://localhost:4502/content/getad.html)

* De code die aan deze URL is gekoppeld, krijgt het accountnummer van de aangemelde gebruiker. Dit accountnummer wordt vervolgens als aanvraagkenmerk aan de FDM doorgegeven. De FDM haalt dan de gegevens op die aan dit accountnummer zijn gekoppeld en vult het document met het webkanaal.

>[!NOTE]
>
>Kijk eens naar de **/apps/AEMForms/fetchad/GET.jsp** bestand in crx. Zorg ervoor dat de tekenreeksvariabele webChannelDocument naar een geldig communicatiedocumentpad verwijst.

## Volgende stappen

[E-maillevering instellen](../interactive-communications/delivery-of-web-channel-document-tutorial-use.md)