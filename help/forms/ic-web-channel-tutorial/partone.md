---
title: Tomcat installeren en configureren
description: Dit is een onderdeel van een zelfstudie met meerdere stappen voor het maken van uw eerste interactieve communicatiedocument. In dit onderdeel wordt TOMCAT geïnstalleerd en wordt het bestand sampleRest.war in TOMCAT geïmplementeerd.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 4f400c22-6c96-4018-851c-70d988ce7c6c
topic: Development
role: Developer
level: Beginner
exl-id: f0f19838-1ade-4eda-b736-a9703a3916c2
duration: 66
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 0%

---

# Tomcat installeren en configureren {#install-and-configure-tomcat}

In dit deel, installeren wij TOMCAT en stellen het sampleRest.war- dossier in TOMCAT op. Het REST eindpunt dat door dit dossier van WAR wordt blootgesteld is de basis voor ons Gegevensbron en Model van de Gegevens van de Vorm.

Volg de volgende instructies om Tomcat in te stellen:

1. Download en installeer JDK1.8.
2. Stel JAVA_HOME in om naar JDK1.8 te wijzen.
3. Downloaden [tomcat](https://tomcat.apache.org/). Dit oorlogsbestand is getest met Tomcat versie 8.5.x en 9.0.x.
4. Download de nieuwste versie van uw voorkeur. U kunt de 64-bits Windows zip downloaden onder de kernsectie.
5. Pak de inhoud uit tot aan uw c:\tomcat.
6. Je moet iets als dit zien in je c-drive **c:\tomcat\apache-tomcat-8.5.27** afhankelijk van de versie van uw tomcat
7. Maak een omgevingsvariabele met de naam &quot;CATALINA_HOME&quot; en stel de waarde ervan in op het voorbeeld van de map tomcat-install c:\tomcat\apache- tomcat-8.5.27
8. Kopieer het bestand SampleRest.war naar de map webapps van uw Tomcat-installatie
9. Nieuw opdrachtpromptvenster starten.
10. Navigeren naar &lt;tomcat install=&quot;&quot; folder=&quot;&quot;>\bin en voert het bestand start.bat uit
11. Als uw tomcat is gestart, test u het eindpunt dat door WAR File wordt getoond door [hier klikken](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. U zou steekproefgegevens als resultaat van deze vraag moeten krijgen.

Gefeliciteerd!!! U kunt het bestand SampleRest.war instellen en implementeren.

## Volgende stappen

[RESTful-gegevensbron configureren](./parttwo.md)
