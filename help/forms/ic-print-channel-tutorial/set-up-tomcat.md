---
title: Tomcat installeren en configureren
seo-title: Tomcat installeren en configureren
description: Dit is een onderdeel van de zelfstudie met meerdere stappen voor het maken van uw eerste interactieve communicatiedocument. In dit onderdeel wordt TOMCAT geïnstalleerd en wordt het bestand sampleRest.war in TOMCAT geïmplementeerd. Het REST eindpunt dat door dit dossier van WAR wordt blootgesteld zal de basis voor ons Gegevensbron en Model van de Gegevens van de Vorm zijn.
seo-description: Dit is een onderdeel van de zelfstudie met meerdere stappen voor het maken van uw eerste interactieve communicatiedocument. In dit onderdeel wordt TOMCAT geïnstalleerd en wordt het bestand sampleRest.war in TOMCAT geïmplementeerd. Het REST eindpunt dat door dit dossier van WAR wordt blootgesteld zal de basis voor ons Gegevensbron en Model van de Gegevens van de Vorm zijn.
uuid: 835e2342-82b6-4f0c-9a6b-467bbbd8527a
feature: interactieve communicatie
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
thumbnail: 37815.jpg
discoiquuid: 5f68be3d-aa35-4a3f-aaea-b8ee213c87ae
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 0%

---


# Tomcat {#install-and-configure-tomcat} installeren en configureren

In dit deel, zullen wij TOMCAT installeren en het sampleRest.war- dossier in TOMCAT opstellen. Het REST eindpunt dat door dit dossier van WAR wordt blootgesteld zal de basis voor ons Gegevensbron en Model van de Gegevens van de Vorm zijn.

>[!VIDEO](https://video.tv.adobe.com/v/37815/?quality=9&learn=on)

Volg de volgende instructies om Tomcat in te stellen:

1. Download en installeer JDK1.8.
2. Stel JAVA_HOME in om naar JDK1.8 te wijzen.
3. Download [tomcat](https://tomcat.apache.org/). Dit oorlogsbestand is getest met Tomcat versie 8.5.x en 9.0.x.
4. Download de nieuwste versie van uw voorkeur. U kunt de 64-bits Windows zip downloaden onder de kernsectie.
5. Pak de inhoud uit op c:\tomcat.
6. U zou iets als dit in uw aandrijving van c **c:\tomcat\apache-tomcat-8.5.27** afhankelijk van de versie van uw kat moeten zien
7. Maak een omgevingsvariabele met de naam &quot;CATALINA_HOME&quot; en stel de waarde ervan in op het voorbeeld van de map voor tomcat-installatie c:\tomcat\apache- tomcat-8.5.27
8. Kopieer het bestand SampleRest.war naar de map webapps van uw Tomcat-installatie
9. Nieuw opdrachtpromptvenster starten.
10. Navigeer naar &lt;tomcat install folder>\bin en voer het start.bat uit
11. Zodra uw tomcat is begonnen, test het eindpunt dat door het Dossier van WAR door [klikkend hier](http://localhost:8080/SampleRest/webapi/getStatement/9586) wordt blootgesteld
12. U zou steekproefgegevens als resultaat van deze vraag moeten krijgen.

Gefeliciteerd!!! U kunt het bestand SampleRest.war instellen en implementeren.

In de volgende video wordt de implementatie van een voorbeeldtoepassing in Tomcat uitgelegd
>[!VIDEO](https://video.tv.adobe.com/v/37815)