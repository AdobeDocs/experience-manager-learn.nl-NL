---
title: Adaptief formulier verzenden naar externe server
description: Aangepast formulier verzenden naar REST-eindpunt dat wordt uitgevoerd op externe server
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
topic: Development
role: Developer
level: Beginner
exl-id: 5363c3f7-9006-4430-b647-f3283a366a64
last-substantial-update: 2020-07-07T00:00:00Z
duration: 78
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 0%

---

# Adaptief formulier verzenden naar externe server {#submitting-adaptive-form-to-external-server}

Gebruik Submit aan de actie van het Eindpunt van het REST om de voorgelegde gegevens aan REST URL te posten. De URL kan van een interne (de server waarop het formulier wordt gegenereerd) of van een externe server zijn.

Klanten willen de formuliergegevens meestal naar een externe server verzenden voor verdere verwerking.

Als u gegevens naar een interne server wilt posten, geeft u een pad naar de bron op. De gegevens worden gepost de weg van het middel. Bijvoorbeeld: &lt;/content restendpoint=&quot;&quot;> . Voor dergelijke postverzoeken wordt de authenticatieinformatie van het verzendverzoek gebruikt.

Geef een URL op om gegevens naar een externe server te posten. De opmaak van de URL is <http://host:port/path_to_rest_end_point>. Zorg ervoor dat u de weg hebt gevormd om het verzoek van de POST anoniem te behandelen.

In het kader van dit artikel heb ik een eenvoudig oorlogsdossier geschreven dat op uw exemplaar van de tomcat kan worden opgesteld. Ervan uitgaande dat uw tomcat op poort 8080 wordt uitgevoerd, zal de POST-URL

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

wanneer u uw Aangepast Vorm vormt om aan dit eindpunt voor te leggen, de vormgegevens en de gehechtheid als om het even welk in servlet door de volgende code kan worden gehaald

```java
System.out.println("form was submitted");
Part attachment = request.getPart("attachments");
if(attachment!=null)
{
    System.out.println("The content type of the attachment added is "+attachment.getContentType());
}
Enumeration<String> params = request.getParameterNames();
while(params.hasMoreElements())
{
String paramName = params.nextElement();
System.out.println("The param Name is "+paramName);
String data = request.getParameter(paramName);System.out.println("The data  is "+data);
}
```

![formatie](assets/formsubmission.gif)
Ga als volgt te werk om dit op uw server te testen

1. Installeer Tomcat als u dit nog niet hebt. [Hier zijn instructies voor het installeren van tomcat beschikbaar](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. Download de [zip-bestand](assets/aemformsenablement.zip) aan dit artikel zijn gekoppeld. Pak het bestand uit om het oorlogsbestand op te halen.
1. Implementeer het oorlogsbestand in uw tomcat-server.
1. Maak een eenvoudig adaptief formulier met de component Bestandsbijlage en configureer de verzendactie zoals in de bovenstaande schermafbeelding wordt getoond. De POST-URL is <http://localhost:8080/AemFormsEnablement/HandleFormSubmission>. Wijzig de URL als uw AEM en tomcat niet worden uitgevoerd op localhost.
1. Als u het verzenden van formuliergegevens met meerdere delen naar Tomcat wilt inschakelen, voegt u het volgende kenmerk toe aan het contextelement van het dialoogvenster &lt;tomcatinstalldir>\conf\context.xml en start de Tomcat-server opnieuw op.
1. **&lt;Context allowCasualMultipartParsing=&quot;true&quot;>**
1. Geef een voorbeeld van het adaptieve formulier weer, voeg een bijlage toe en verzend deze. Controleer het venster van de tomcat console voor berichten.
