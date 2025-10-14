---
title: Gegevens samenvoegen met de XDP-sjabloon
description: Een POST-verzoek indienen voor het eindpunt met de benodigde parameters
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-8185
thumbnail: 332439.jpg
exl-id: d144b3f6-7c7a-46a7-bc5f-1767895749d0
duration: 49
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 0%

---

# Maak de POST vraag


De volgende stap is een POST van HTTP vraag aan het eindpunt met de noodzakelijke parameters te maken. Het malplaatje en de gegevensdossiers worden verstrekt als middeldossiers. Eigenschappen van de gegenereerde PDF worden via de parameter van de optie in de aanvraag opgegeven. De eigenschap embedFonts wordt gebruikt om aangepaste lettertypen in te sluiten in de gegenereerde PDF.[&#x200B; Gelieve te volgen deze documentatie om douanedoopvonten aan uw de wolkeninstantie van Forms op te stellen.](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-set-up.html?lang=nl-NL) De eigenschappen worden opgegeven in het bestand options.json resource. Aangezien, heeft het eindpunt symbolische gebaseerde authentificatie wij het Token van de Toegang in de verzoekkopbal overgaan.

De volgende code is gebruikt om pdf te genereren door gegevens samen te voegen met de sjabloon

```java
public class DocumentGeneration
{
        public String SAVE_LOCATION = "c:\\aspire1";
        public void mergeDataWithXdpTemplate(String postURL)
        {
                HttpPost httpPost = new HttpPost(postURL);
                CredentialUtilites cu = new CredentialUtilites();
                String accessToken = cu.getAccessToken();
                httpPost.addHeader("Authorization", "Bearer " + accessToken);
                ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
                URL templateFile = classLoader.getResource("templates/custom_fonts.xdp");
                File xdpTemplate = new File(templateFile.getPath());
                URL url = classLoader.getResource("datafiles");
                System.out.println(url.getPath());
                File files[] = new File(url.getPath()).listFiles();
                for (int i = 0; i < files.length; i++) {
                        MultipartEntityBuilder builder = MultipartEntityBuilder.create();
                        ContentType strContent = ContentType.create("text/plain", Charset.forName("UTF-8"));
                        builder.setMode(HttpMultipartMode.BROWSER_COMPATIBLE);
                        builder.addBinaryBody("data", files[i]);
                        builder.addBinaryBody("template", xdpTemplate);
                        builder.addBinaryBody("options",GetOptions.getPDFOptions().getBytes(),ContentType.APPLICATION_JSON,"options"
                        try {
                                HttpEntity entity = builder.build();
                                httpPost.setEntity(entity);
                                CloseableHttpClient httpclient = HttpClients.createDefault();
                                CloseableHttpResponse response = httpclient.execute(httpPost);
                                InputStream generatedPDF = response.getEntity().getContent();
                                byte[] bytes = IOUtils.toByteArray(generatedPDF);
                                File saveLocation = new File(SAVE_LOCATION);
                                if (!saveLocation.exists()) {
                                        saveLocation.mkdirs();
                                }
                                File outputFile = new File(SAVE_LOCATION+File.separator+files[i].getName().replace("xml", "pdf"));
                                try (FileOutputStream outputStream = new FileOutputStream(outputFile)) {
                                        outputStream.write(bytes);
                                }
                        } catch (Exception e) {
                                System.out.println("The error is " + e.getMessage());
                        }

                }
                System.out.println("Done generating " + files.length + " files");

        }

}
```
