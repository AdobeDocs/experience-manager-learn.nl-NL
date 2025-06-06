---
title: Vereenvoudigde stappen voor het installeren van AEM Forms in Windows
description: Snelle en eenvoudige stappen om AEM Forms in vensters te installeren
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Admin
level: Beginner
exl-id: 80288765-0b51-44a9-95d3-3bdb2da38615
last-substantial-update: 2020-06-09T00:00:00Z
duration: 113
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 0%

---

# Vereenvoudigde stappen voor het installeren van AEM Forms in Windows

>[!NOTE]
>
>Dubbelklik nooit op de AEM Quick Start jar als u AEM Forms wilt gebruiken.
>
>Zorg er ook voor dat het pad naar de installatiemap van AEM Forms geen spaties bevat.
>
>Installeer AEM Forms bijvoorbeeld niet in de map c:\jack en jill\AEM Forms

>[!NOTE]
>
>Als u AEM Forms 6.5 installeert, zorg gelieve ervoor u de volgende 32 beetjeMicrosoft Visuele C++ redistributables hebt geïnstalleerd.
>
>* Microsoft Visual C++ 2008 herdistribueerbaar
>* Microsoft Visual C++ 2010 herdistribueerbaar
>* Microsoft Visual C++ 2012 herdistribueerbaar
>* Microsoft Visual C++ 2013 herdistribueerbaar (vanaf 6.5)

Hoewel wij na de [ officiële documentatie ](https://helpx.adobe.com/nl/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) voor het installeren van AEM Forms adviseren. U kunt de volgende stappen volgen om AEM Forms in Windows-omgeving te installeren en configureren:

* Zorg ervoor dat de juiste JDK is geïnstalleerd
   * AEM 6.2 die u nodig hebt: Oracle SE 8 JDK 1.8.x (64-bits)
   * AEM 6.3 en AEM 6.4 hebt u nodig: Oracle SE 8 JDK 1.8.x (64-bits)
   * AEM 6.5 hebt u JDK 8 of JDK 11 nodig
   * [ Officiële JDK Vereisten ](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/technical-requirements.html?lang=nl-NL) zijn hier vermeld
* Zorg ervoor JAVA_HOME wordt geplaatst om aan JDK te richten u hebt geïnstalleerd.
   * Volg onderstaande stappen om de JAVA_HOME-variabele in vensters te maken:
      * Klik met de rechtermuisknop op Deze computer en selecteer Eigenschappen
      * Selecteer op het tabblad Geavanceerd de optie Omgevingsvariabelen en maak een nieuwe systeemvariabele met de naam JAVA_HOME.
      * Stel de waarde van de variabele in op de JDK die op uw systeem is geïnstalleerd. Bijvoorbeeld c:\program files\java\jdk1.8.0_25

* Maak een map met de naam AEMForms op station C
* Zoek AEMQuickStart.Jar en verplaats het naar de map AEMForms
* Kopieer het bestand license.properties naar deze map van AEMForms
* Maak een batchbestand met de naam &quot;StartAemForms.bat&quot; met de volgende inhoud:
   * `java -d64 -Xmx2048M -jar AEM_6.5_Quickstart.jar -gui`
      * Hier is AEM_6.5_Quickstart.jar de naam van mijn AEM quickstart jar.
   * U kunt de naam van de jar wijzigen, maar zorg dat de naam wordt weergegeven in het batchbestand. Sla het batchbestand op in de map AEMForms.

* Open een nieuwe bevelherinnering, en navigeer aan _c:\ aemforms_.

* Voer het StartAemForms.bat- dossier van de bevelherinnering uit.

* Er moet een klein dialoogvenster verschijnen waarin de voortgang van het opstarten wordt aangegeven.

* Wanneer het opstarten is voltooid, opent u het bestand sling.properties. U vindt deze in de map c:\AEMForms\crx-quickstart\conf.

* Kopieer de volgende twee regels naar de onderkant van het bestand
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.&#42;** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.&#42;**
* Deze twee eigenschappen zijn vereist voor documentservices die werken
* Het bestand sling.properties opslaan
* [ Download de aangewezen vormen addon pakket ](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=nl-NL)
* Installeer de vormen toevoegen op pakket gebruikend [ pakketmanager ](http://localhost:4502/crx/packmgr/index.jsp).
* Nadat u het pakket hebt geïnstalleerd, moet u de volgende stappen volgen

   * **zorg ervoor alle bundels in actieve staat zijn. (Met uitzondering van de AEMFD-bundel Handtekeningen).**
   * **het zou typisch 5 of meer notulen voor alle bundels nemen om aan actieve staat te krijgen.**

   * **Zodra alle bundels (behalve de bundel van de Handtekeningen AEMFD) actief zijn, nieuw begin uw systeem om de installatie van AEM Forms te voltooien**

## sun.util.agenda-pakket aan de lijst van gewenste personen

1. Open het Webconsole van Felix in uw [ browser venster ](http://localhost:4502/system/console/configMgr)
1. Firewall-configuratie deserialization zoeken en openen: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
1. `sun.util.calendar` toevoegen als een nieuw item onder `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
1. Sla de wijzigingen op.

Gefeliciteerd!! U hebt nu AEM Forms op uw systeem geïnstalleerd en geconfigureerd.
Afhankelijk van uw behoeften kunt u [ Uitbreidingen van Reader ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html?lang=nl-NL) vormen of [ PDFG ](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=nl-NL) op uw server
