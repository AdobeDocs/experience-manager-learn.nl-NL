---
title: XDP-stitching met assemblerservice
description: De Assembler Service in AEM Forms gebruiken om xdp te hechten
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-12-19T00:00:00Z
exl-id: e116038f-7d86-41ee-b1b0-7b8569121d6d
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 0%

---

# XDP Stitching using assembler Service

In dit artikel vindt u de elementen waarmee u kunt aantonen dat u xdp-documenten kunt naaien met gebruik van de assembleerservice.
De volgende JSP-code is geschreven om een subformulier in te voegen dat een naam heeft **adres** vanuit xdp-document met de naam address.xdp in een invoegpositie met de naam **adres** in document master.xdp. De resulterende xdp is opgeslagen in de hoofdmap van de AEM-installatie.

De vergaderingsdienst baseert zich op een geldige DX- documenten om de manipulatie van de documenten van de PDF te beschrijven. U kunt verwijzen naar de [DDX-referentiedocument hier](assets/ddxRef.pdf).Pagina 40 bevat informatie over xdp stitching.

```java
    javax.servlet.http.Part ddxFile = request.getPart("xdpstitching.ddx");
    System.out.println("Got DDX");
    java.io.InputStream ddxIS = ddxFile.getInputStream();
    com.adobe.aemfd.docmanager.Document ddxDocument = new com.adobe.aemfd.docmanager.Document(ddxIS);
    javax.servlet.http.Part masterXdpPart = request.getPart("masterxdp.xdp");
    System.out.println("Got master xdp");
    java.io.InputStream masterXdpPartIS = masterXdpPart.getInputStream();
    com.adobe.aemfd.docmanager.Document masterXdpDocument = new com.adobe.aemfd.docmanager.Document(masterXdpPartIS);

    javax.servlet.http.Part fragmentXDPPart = request.getPart("fragment.xdp");
    System.out.println("Got fragment.xdp");
    java.io.InputStream fragmentXDPPartIS = fragmentXDPPart.getInputStream();
    com.adobe.aemfd.docmanager.Document fragmentXdpDocument = new com.adobe.aemfd.docmanager.Document(fragmentXDPPartIS);

    java.util.Map < String, Object > mapOfDocuments = new java.util.HashMap < String, Object > ();
    mapOfDocuments.put("master.xdp", masterXdpDocument);
    mapOfDocuments.put("address.xdp", fragmentXdpDocument);
    com.adobe.fd.assembler.service.AssemblerService assemblerService = sling.getService(com.adobe.fd.assembler.service.AssemblerService.class);
    if (assemblerService != null)
      System.out.println("Got assembler service");

    com.adobe.fd.assembler.client.AssemblerOptionSpec aoSpec = new com.adobe.fd.assembler.client.AssemblerOptionSpec();
    aoSpec.setFailOnError(true);

    com.adobe.fd.assembler.client.AssemblerResult assemblerResult = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
    com.adobe.aemfd.docmanager.Document finalXDP = assemblerResult.getDocuments().get("stitched.xdp");
    finalXDP.copyToFile(new java.io.File("stitched.xdp"));
```

Het DDX-bestand dat fragmenten moet invoegen in een andere xdp, wordt hieronder weergegeven. De DDX voegt het subformulier in  **adres** van address.xdp in het geroepen toevoegingspunt **adres** in het bestand master.xdp. Het resulterende document met de naam **stitched.xdp** wordt opgeslagen in het bestandssysteem.

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<DDX xmlns="http://ns.adobe.com/DDX/1.0/"> 
        <XDP result="stitched.xdp"> 
           <XDP source="master.xdp"> 
            <XDPContent insertionPoint="address" source="address.xdp" fragment="address"/> 
         </XDP> 
        </XDP>         
</DDX>
```

Om deze functie te laten werken op uw AEM

* Downloaden [XDP-pakket](assets/xdp-stitching.zip) naar uw lokale systeem.
* Upload en installeer het pakket met de [pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)
* [De inhoud van dit ZIP-bestand extraheren](assets/xdp-and-ddx.zip) om het voorbeeld-xdp- en DDX-bestand op te halen

**Nadat u het pakket installeert, zult u volgende URLs in de Filter van CSRF van de Adobe moeten lijsten van gewenste personen Granite.**

1. Volg de onderstaande stappen om de hierboven vermelde paden te lijsten van gewenste personen.
1. [Aanmelden bij configMgr](http://localhost:4502/system/console/configMgr)
1. Zoeken naar Adobe Granite CSRF-filter
1. Het volgende pad toevoegen aan de uitgesloten secties en opslaan `/content/AemFormsSamples/assemblerservice`
1. Zoeken naar het filter Verticale verwijzing
1. Schakel het selectievakje Lege toestaan in. (Deze instelling is alleen bedoeld voor testdoeleinden) Er zijn verschillende manieren om de voorbeeldcode te testen. De snelste en eenvoudigste manier is om Postman-app te gebruiken. Met Postman kunt u POSTEN aanvragen bij uw server. Installeer de Postman-toepassing op uw systeem.
Start de app en voer de volgende URL in om de API voor exportgegevens te testen http://localhost:4502/content/AemFormsSamples/assemblerservice.html

Geef de volgende invoerparameters op, zoals opgegeven in de schermafbeelding. U kunt de voorbeelddocumenten gebruiken die u eerder hebt gedownload,
![xdp-stitch-postman](assets/xdp-stitching-postman.png)

>[!NOTE]
>
>Zorg ervoor dat de AEM Forms-installatie is voltooid. Alle bundels moeten actief zijn.
