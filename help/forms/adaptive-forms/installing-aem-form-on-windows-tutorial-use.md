---
title: Vereenvoudigde stappen voor het installeren van AEM Forms in Windows
seo-title: Vereenvoudigde stappen voor het installeren van AEM Forms in Windows
description: Snelle en eenvoudige stappen om AEM Forms in vensters te installeren
seo-description: Snelle en eenvoudige stappen om AEM Forms in vensters te installeren
uuid: a148b8f0-83db-47f6-89d3-c8a9961be289
feature: Adaptive Forms
topics: administration
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 1182ef4d-5838-433b-991d-e24ab805ae0e
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---


# Vereenvoudigde stappen voor het installeren van AEM Forms in Windows

>[!NOTE]
>
>Dubbelklik nooit op de AEM Quick Start-jar als u AEM Forms wilt gebruiken.
>
>Zorg er ook voor dat het pad naar de installatiemap van AEM Forms geen spaties bevat.
>
>Installeer AEM Forms bijvoorbeeld niet in c:\jack and jill\AEM Forms folder

>[!NOTE]
>
>Als u AEM Forms 6.5 installeert, zorg gelieve ervoor u de volgende 32 beetjeMicrosoft Visuele C++ redistributables hebt geïnstalleerd.
>
>* Microsoft Visual C++ 2008 redistributable
>* Microsoft Visual C++ 2010 redistributable
>* Microsoft Visual C++ 2012 redistributable
>* Microsoft Visual C++ 2013 redistributable (vanaf 6.5)


Hoewel we het volgende aanbevelen in de [officiële documentatie](https://helpx.adobe.com/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) voor het installeren van AEM Forms. U kunt de volgende stappen volgen om AEM Forms in Windows-omgeving te installeren en configureren:

* Zorg ervoor dat de juiste JDK is geïnstalleerd
   * AEM 6.2 hebt u nodig: Oracle SE 8 JDK 1.8.x (64-bits)
* 
   * AEM 6.3 en AEM 6.4 hebt u nodig: Oracle SE 8 JDK 1.8.x (64-bits)
* AEM 6.5 hebt u JDK 8 of JDK 11 nodig
* [Officiële JDK-](https://helpx.adobe.com/experience-manager/6-3/sites/deploying/using/technical-requirements.html) vereisten worden hier vermeld
* Zorg ervoor JAVA_HOME wordt geplaatst om aan JDK te richten u hebt geïnstalleerd.
   * Volg onderstaande stappen om de JAVA_HOME-variabele in vensters te maken:
      * Klik met de rechtermuisknop op Deze computer en selecteer Eigenschappen
      * Selecteer op het tabblad Geavanceerd de optie Omgevingsvariabelen en maak een nieuwe systeemvariabele met de naam JAVA_HOME.
      * Stel de waarde van de variabele in op de JDK die op uw systeem is geïnstalleerd. Bijvoorbeeld c:\program files\java\jdk1.8.0_25

* Maak een map met de naam AEMForms op station C
* Zoek het bestand AEMQuickStart.Jar en verplaats het naar de map AEMForms
* Kopieer het bestand license.properties naar deze map van AEMForms
* Maak een batchbestand met de naam &quot;StartAemForms.bat&quot; met de volgende inhoud:
   * java -d64 -Xmx2048M -jar AEM_6.3_Quickstart.jar -gui.here AEM_6.3_Quickstart.jar is de naam van mijn AEM quickstart jar.
   * U kunt de naam van de pot wijzigen, maar zorg dat de naam wordt weergegeven in het batchbestand. Sla het batchbestand op in de map AEMForms.

* Open een nieuwe bevelherinnering, en navigeer aan c:\aemforms.

* Voer het StartAemForms.bat- dossier van de bevelherinnering uit.

* Er moet een klein dialoogvenster verschijnen waarin de voortgang van het opstarten wordt aangegeven.

* Als het opstarten is voltooid, opent u het bestand sling.properties. U vindt deze in c:\AEMForms\crx-quickstart\conf folder.

* Kopieer de volgende twee regels naar de onderkant van het bestand
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.*** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.***
* Deze twee eigenschappen zijn vereist voor documentservices om te kunnen werken
* Het bestand sling.properties opslaan

* [Aanmelden voor delen pakket](http://localhost:4502/crx/packageshare/login.html)

   * U hebt Adobe-id nodig om u aan te melden voor delen van pakket
   * Zoeken naar AEM Forms Add in een pakket dat geschikt is voor uw versie van AEM Forms en besturingssysteem
   * Of [u kunt het juiste formulieraddon-pakket downloaden](https://helpx.adobe.com/aem-forms/kb/aem-forms-releases.html)
   * Nadat u de add-on hebt geïnstalleerd, moeten de volgende stappen worden uitgevoerd

      * **Zorg ervoor dat alle bundels actief zijn. (Met uitzondering van de bundel voor AEMFD-handtekeningen).**
      * **Het duurt meestal 5 of meer minuten voordat alle bundels actief worden.**
   * **Zodra alle bundels actief zijn (behalve de bundel van Handtekeningen AEMFD), begin uw systeem opnieuw om de installatie van AEM Forms te voltooien**


* Voeg `sun.util.calendar` pakket aan de lijst van gewenste personen toe:

   1. Felix-webconsole openen in uw [browservenster](http://localhost:4502/system/console/configMgr)
   2. Configuratie van firewall voor zoeken en openen: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
   3. `sun.util.calendar` toevoegen als een nieuw item onder `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
   4. Sla de wijzigingen op.

Gefeliciteerd!! U hebt nu AEM Forms op uw systeem geïnstalleerd en geconfigureerd.
Afhankelijk van uw behoeften kunt u [Reader-extensies configureren](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html) of [ PDFG](https://helpx.adobe.com/experience-manager/6-3/forms/using/install-configure-pdf-generator.html) op uw server
