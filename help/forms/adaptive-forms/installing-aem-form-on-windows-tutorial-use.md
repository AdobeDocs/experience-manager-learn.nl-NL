---
title: Vereenvoudigde stappen voor het installeren van AEM Forms in Windows
description: Snelle en eenvoudige stappen om AEM Forms in vensters te installeren
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Admin
level: Beginner
exl-id: 80288765-0b51-44a9-95d3-3bdb2da38615
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '578'
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
>* Microsoft Visual C++ 2008 herdistribueerbaar
>* Microsoft Visual C++ 2010 herdistribueerbaar
>* Microsoft Visual C++ 2012 herdistribueerbaar
>* Microsoft Visual C++ 2013 herdistribueerbaar (vanaf 6.5)


Hoewel we het volgende aanbevelen [officiële documentatie](https://helpx.adobe.com/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) voor het installeren van AEM Forms. U kunt de volgende stappen volgen om AEM Forms in Windows-omgeving te installeren en configureren:

* Zorg ervoor dat de juiste JDK is geïnstalleerd
   * AEM 6.2 hebt u nodig: Oracle SE 8 JDK 1.8.x (64-bits)
* 
   * AEM 6.3 en AEM 6.4 hebt u nodig: Oracle SE 8 JDK 1.8.x (64-bits)
* AEM 6.5 hebt u JDK 8 of JDK 11 nodig
* [Officiële JDK-vereisten](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/technical-requirements.html?lang=en) hier vermeld
* Zorg ervoor JAVA_HOME wordt geplaatst om aan JDK te richten u hebt geïnstalleerd.
   * Volg onderstaande stappen om de JAVA_HOME-variabele in vensters te maken:
      * Klik met de rechtermuisknop op Deze computer en selecteer Eigenschappen
      * Selecteer op het tabblad Geavanceerd de optie Omgevingsvariabelen en maak een nieuwe systeemvariabele met de naam JAVA_HOME.
      * Stel de waarde van de variabele in op de JDK die op uw systeem is geïnstalleerd. Bijvoorbeeld c:\program files\java\jdk1.8.0_25

* Maak een map met de naam AEMForms op station C
* Zoek het bestand AEMQuickStart.Jar en verplaats het naar de map AEMForms
* Kopieer het bestand license.properties naar deze map van AEMForms
* Maak een batchbestand met de naam &quot;StartAemForms.bat&quot; met de volgende inhoud:
   * `java -d64 -Xmx2048M -jar AEM_6.5_Quickstart.jar -gui`
      * Hier AEM_6.5_Quickstart.jar is de naam van mijn AEM quickstart jar.
   * U kunt de naam van de jar wijzigen, maar zorg dat de naam wordt weergegeven in het batchbestand. Sla het batchbestand op in de map AEMForms.

* Open een nieuwe bevelherinnering, en navigeer aan _c:\aemforms_.

* Voer het StartAemForms.bat- dossier van de bevelherinnering uit.

* Er moet een klein dialoogvenster verschijnen waarin de voortgang van het opstarten wordt aangegeven.

* Wanneer het opstarten is voltooid, opent u het bestand sling.properties. U vindt deze in c:\AEMForms\crx-quickstart\conf folder.

* Kopieer de volgende twee regels naar de onderkant van het bestand
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.&#42;** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.&#42;**
* Deze twee eigenschappen zijn vereist voor documentservices om te kunnen werken
* Het bestand sling.properties opslaan
* [Download het juiste adrespakket voor formulieren](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=en)
* De toegevoegde formulieren op het pakket installeren met [pakketbeheer.](http://localhost:4502/crx/packmgr/index.jsp)
* Nadat u het pakket hebt geïnstalleerd, moet u de volgende stappen volgen

       * **Zorg ervoor alle bundels in actieve staat zijn. (Met uitzondering van de bundel voor AEMFD-handtekeningen).**
       * **Het duurt meestal 5 of meer minuten voordat alle bundels actief worden.**
   
   * **Zodra alle bundels actief zijn (behalve de bundel van Handtekeningen AEMFD), begin uw systeem opnieuw om de installatie van AEM Forms te voltooien**

## sun.util.agenda-pakket aan de lijst van gewenste personen

1. Felix-webconsole openen in uw [browservenster](http://localhost:4502/system/console/configMgr)
2. Configuratie van firewall voor zoeken en openen: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
3. Toevoegen `sun.util.calendar` als nieuwe vermelding onder `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
4. Sla de wijzigingen op.

Gefeliciteerd!! U hebt nu AEM Forms op uw systeem geïnstalleerd en geconfigureerd.
Afhankelijk van uw behoeften kunt u configureren  [Reader-extensies](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html?lang=en) of [ PDFG](https://experienceleague.adobe.com/docs/experience-manager-64/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=en) op uw server
