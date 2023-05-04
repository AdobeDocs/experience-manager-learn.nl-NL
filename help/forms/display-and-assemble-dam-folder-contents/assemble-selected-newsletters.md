---
title: Geselecteerde nieuwsbrieven combineren in één bestand
description: Geselecteerde nieuwsbrieven combineren met montageservice
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
last-substantial-update: 2023-01-01T00:00:00Z
exl-id: 3a64315f-f699-4538-b999-626e7a998c05
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 0%

---

# Geselecteerde nieuwsbrieven combineren tot één pdf

De selecties van de gebruiker worden opgeslagen in een verborgen veld. De waarde van dit verborgen veld wordt doorgegeven aan het servlet, dat de selecties in één PDF combineert met [Forms Assembler-service](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/assembler/service/AssemblerService.html).


## Servlet voor het samenstellen van pdf-bestanden

In de volgende code worden de geselecteerde nieuwsbrieven samengesteld. De code maakt een kaart met documenten op basis van de selecties van de gebruiker. Van deze kaart wordt DDX gecreeerd en dit DDX samen met de kaart van documenten wordt overgegaan tot de aanroepmethode van de dienst van de Assembler om het gecombineerde document te krijgen. Het geassembleerde pdf wordt opgeslagen in de repository en het pad ervan wordt geretourneerd aan de aanroepende toepassing.

```java
protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response)
   {
   
       String []newsletters = request.getParameter("selectedNewsLetters").split(",");
       Map<String, Object> mapOfDocuments = new HashMap<String, Object>();
       for(int i= 0;i<newsletters.length;i++)
       {
           Resource resource = request.getResourceResolver().getResource(newsletters[i]);
           
           log.debug("The resource name is "+resource.getName());
           Document newsletter = new Document(resource.getPath());
           mapOfDocuments.put(resource.getName(), newsletter);
       }
       log.debug("The newsletters selected: "+newsletters);
       Document ddxDocument = createDDXFromMapOfDocuments(mapOfDocuments);
       AssemblerOptionSpec aoSpec = new AssemblerOptionSpec();
       aoSpec.setFailOnError(true);
       AssemblerResult ar = null;
       try {
               ar = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
               Document assembledPDF = ar.getDocuments().get("GeneratedPDF.pdf");
               // This is my custom code to get fd-service user
               ResourceResolver formsServiceResolver = getResolver.getFormsServiceResolver();
               
               Resource nodeResource = formsServiceResolver.getResource("/content/newsletters");
           
               UUID uuid = UUID.randomUUID();
               String uuidString = uuid.toString();
               javax.jcr.Node assembledNewsletters = nodeResource.adaptTo(Node.class);
               javax.jcr.Node assembledNewsletter =  assembledNewsletters.addNode(uuidString + ".pdf", "nt:file");
               javax.jcr.Node resNode = assembledNewsletter.addNode("jcr:content", "nt:resource");
               ValueFactory valueFactory = formsServiceResolver.adaptTo(Session.class).getValueFactory();
               Binary contentValue = valueFactory.createBinary(assembledPDF.getInputStream());
               resNode.setProperty("jcr:data", contentValue);
               formsServiceResolver.commit();
               PrintWriter out = response.getWriter();
               response.setContentType("application/json");
               response.setCharacterEncoding("UTF-8");
               JsonObject asset = new JsonObject();
          
               asset.addProperty("assetPath", assembledNewsletter.getPath());
               out.print(new Gson().toJson(asset));
               out.flush();  
               
           } 
           catch (IOException | OperationException | RepositoryException e)
           {
           
               log.error("Error is "+e.getMessage());
           }
   }
```

## Hulpfuncties

De volgende nutsfuncties werden gebruikt bij de assemblage van nieuwsbrieven. Met deze hulpprogrammafuncties maakt u DDX van een kaart met documenten en zet u het document org.w3c.dom.Document om in het documentobject AEMFD.


```java
public Document createDDXFromMapOfDocuments(Map<String, Object> mapOfDocuments)
     {
         
        final DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
        org.w3c.dom.Document ddx = null;
        try
               {
                   DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
                   ddx = docBuilder.newDocument();
                   Element rootElement = ddx.createElementNS("http://ns.adobe.com/DDX/1.0/", "DDX");
                ddx.appendChild(rootElement);
                Element pdfResult = ddx.createElement("PDF");
                pdfResult.setAttribute("result","GeneratedPDF.pdf");
                rootElement.appendChild(pdfResult);
                for (String key : mapOfDocuments.keySet())
                    {
                        log.debug(key + " " + mapOfDocuments.get(key));
                        Element pdfSourceElement = ddx.createElement("PDF");
                        pdfSourceElement.setAttribute("source", key);
                        pdfSourceElement.setAttribute("bookmarkTitle",key);
                        pdfResult.appendChild(pdfSourceElement);
                    }
                return orgw3cDocumentToAEMFDDocument(ddx);
            }
            catch (ParserConfigurationException e)
                {
                    log.debug("Error:"+e.getMessage());
                }
            return null;
     }
```

```java
public Document orgw3cDocumentToAEMFDDocument( org.w3c.dom.Document xmlDocument)
     {
         final ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
         DOMSource source = new DOMSource(xmlDocument);
         log.debug("$$$$In orgW3CDocumentToAEMFDDocument method");
         StreamResult outputTarget = new StreamResult(outputStream);
         try
             {
               TransformerFactory.newInstance().newTransformer().transform(source, outputTarget);
               InputStream is1 = new ByteArrayInputStream(outputStream.toByteArray());
               Document xmlAEMFDDocument = new Document(is1);
               if (log.isDebugEnabled())
                   {
                    xmlAEMFDDocument.copyToFile(new File("dataxmldocument.xml"));
                }
             return xmlAEMFDDocument;
            }
            catch (Exception e)
                 {
                    log.error("Error in generating ddx " + e.getMessage());
                    return null;
                 }
        }
```

## Volgende stappen

[De voorbeeldbestanden op uw systeem implementeren](./deploy-on-your-system.md)