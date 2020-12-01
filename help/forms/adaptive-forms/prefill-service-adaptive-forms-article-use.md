---
title: Prefill-service in adaptieve Forms
seo-title: Prefill-service in adaptieve Forms
description: Aangepaste formulieren vooraf invullen door gegevens op te halen uit achterwaartse gegevensbronnen.
seo-description: Aangepaste formulieren vooraf invullen door gegevens op te halen uit achterwaartse gegevensbronnen.
sub-product: formulieren
feature: adaptive-forms
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 26a8cba3-7921-4cbb-a182-216064e98054
discoiquuid: 936ea5e9-f5f0-496a-9188-1a8ffd235ee5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---


# Prefill-service gebruiken in Adaptive Forms

U kunt de velden van een adaptief formulier vooraf invullen met bestaande gegevens. Wanneer een gebruiker een formulier opent, worden de waarden voor die velden vooraf ingevuld. Er zijn meerdere manieren om aangepaste formuliervelden vooraf in te vullen. In dit artikel bekijken we het vooraf ingevulde adaptieve formulier met de AEM Forms Prefill-service.

Als u meer wilt weten over verschillende methoden om aangepaste formulieren vooraf in te vullen, [volgt u deze documentatie](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

Als u een vooraf aangepast formulier wilt invullen met de Prefill-service, moet u een klasse maken die de DataProvider-interface implementeert. De methode getPrefillData zal de logica hebben om gegevens te bouwen en terug te keren die het adaptieve formulier zal verbruiken om de gebieden vooraf in te vullen. In deze methode kunt u de gegevens ophalen van elke bron en de invoerstream van het gegevensdocument retourneren. De volgende voorbeeldcode haalt de gebruikersprofielinformatie van de aangemelde gebruiker op en maakt een XML-document waarvan de invoerstream wordt geretourneerd voor gebruik door de adaptieve formulieren.

In het codefragment hieronder hebben we een klasse die de DataProvider-interface implementeert. We krijgen toegang tot de aangemelde gebruiker en halen vervolgens de profielgegevens van de aangemelde gebruiker op. Vervolgens maken we een XML-document met het basisknooppuntelement &#39;data&#39; en voegen we de juiste elementen toe aan dit gegevensknooppunt. Wanneer het XML-document is samengesteld, wordt de invoerstream van het XML-document geretourneerd.

Deze klasse wordt dan gemaakt in bundel OSGi en opgesteld in AEM. Zodra de bundel wordt opgesteld, is deze prefill dienst beschikbaar om als prefill dienst van uw Aangepast Vorm te worden gebruikt.

```java
public class PrefillAdaptiveForm implements DataProvider {
 private Logger logger = LoggerFactory.getLogger(PrefillAdaptiveForm.class);

 public String getServiceName() {
  return "Default Prefill Service";
 }
 
 public String getServiceDescription() {
  return "This is default prefill service to prefill adaptive form with user data";
 }
 
 public PrefillData getPrefillData(final DataOptions dataOptions) throws FormsException {
  PrefillData prefillData = new PrefillData() {
   public InputStream getInputStream() {
    return getData(dataOptions);
   }
   
   public ContentType getContentType() {
    return ContentType.XML;
   }
  };
  return prefillData;
 }

 private InputStream getData(DataOptions dataOptions) throws FormsException {  
  try {
   Resource aemFormContainer = dataOptions.getFormResource();
   ResourceResolver resolver = aemFormContainer.getResourceResolver();
   Session session = resolver.adaptTo(Session.class);
   UserManager um = ((JackrabbitSession) session).getUserManager();
   Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
   DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
   DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
   Document doc = docBuilder.newDocument();
   Element rootElement = doc.createElement("data");
   doc.appendChild(rootElement);
   Element firstNameElement = doc.createElement("fname");
   firstNameElement.setTextContent(loggedinUser.getProperty("profile/givenName")[0].getString());
     .
     .
     .
   InputStream inputStream = new ByteArrayInputStream(rootElement.getTextContent().getBytes());
   return inputStream;
  } catch (Exception e) {
   logger.error("Error while creating prefill data", e);
   throw new FormsException(e);
  }
 }
}
```

Voer het volgende uit om deze mogelijkheid op uw server te testen

* [Download en extraheer de inhoud van het ZIP-bestand naar uw computer](assets/prefillservice.zip)
* Zorg ervoor het het programma geopende [gebruikersprofiel](http://localhost:4502/libs/granite/security/content/useradmin) informatie volledig wordt ingevuld. Dit is een vereiste voor het voorbeeld. Het voorbeeld bevat geen foutcontrole voor ontbrekende eigenschappen van gebruikersprofielen.
* De bundel implementeren met de [AEM webconsole](http://localhost:4502/system/console/bundles)
* Adaptief formulier maken met de XSD
* &quot;Custom Aem Form Pre Fill Service&quot; koppelen als de vooraf ingevulde service voor uw adaptieve formulier
* Schema-elementen naar het formulier slepen en neerzetten
* Geef een voorbeeld van het formulier weer

>[!NOTE]
>
>Als het adaptieve formulier is gebaseerd op XSD, moet u ervoor zorgen dat het XML-document dat door de Prefill-service wordt geretourneerd, overeenkomt met de XSD waarop het adaptieve formulier is gebaseerd.
>
>Als het adaptieve formulier niet is gebaseerd op XSD, moet u de velden handmatig binden. Als u bijvoorbeeld een adaptief formulierveld bindt aan een naamelement in de XML-gegevens die u `/data/fname` gebruikt in de bindingsverwijzing van het adaptieve formulierveld.

