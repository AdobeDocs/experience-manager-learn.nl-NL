---
title: Aangepaste processtap implementeren
seo-title: Aangepaste processtap implementeren
description: Aangepaste formulierbijlagen naar bestandssysteem schrijven met behulp van een stap voor aangepast proces
seo-description: Aangepaste formulierbijlagen naar bestandssysteem schrijven met behulp van een stap voor aangepast proces
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '895'
ht-degree: 0%

---


# Aangepaste processtap

Deze zelfstudie is bedoeld voor AEM Forms-klanten die aangepaste processtappen moeten implementeren. Een processtap kan ECMA-script uitvoeren of aangepaste Java-code aanroepen om bewerkingen uit te voeren. Dit leerprogramma zal de stappen verklaren nodig om WorkflowProcess uit te voeren dat door de processtap wordt uitgevoerd.

De belangrijkste reden voor het uitvoeren van de stap van het douaneproces is de AEMWerkstroom uit te breiden. Als u bijvoorbeeld AEM Forms-componenten in uw workflowmodel gebruikt, kunt u de volgende bewerkingen uitvoeren

* De adaptieve formulierbijlage(s) opslaan naar het bestandssysteem
* De ingediende gegevens bewerken

Om het bovengenoemde gebruikscase te verwezenlijken, zult u typisch een dienst OSGi schrijven die door de processtap wordt uitgevoerd.

## Maven Project maken

De eerste stap bestaat uit het maken van een gemodelleerd project met het juiste type Adobe Maven Archetype. De gedetailleerde stappen worden vermeld in dit [artikel](https://helpx.adobe.com/experience-manager/using/maven_arch13.html). Zodra u uw beproefd die project hebt in eclipse wordt ingevoerd, bent u klaar beginnen uw eerste component te schrijven OSGi die in uw processtap kan worden gebruikt.


### Klasse maken die WorkflowProcess implementeert

Open het beproefde project in uw eclipse winde. Breid **projectnaam** > **core** omslag uit. Vouw de map src/main/java uit. U moet een pakket zien dat eindigt met &quot;core&quot;. Maak een Java-klasse die WorkflowProcess in dit pakket implementeert. U moet de uitvoeringsmethode overschrijven. De handtekening van de uitvoeringsmethode is als volgt
public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)genereert WorkflowException
De methode execute geeft toegang tot de volgende drie variabelen

**WorkItem**: De variabele workItem geeft toegang tot gegevens die betrekking hebben op de workflow. De openbare API documentatie is beschikbaar [hier.](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession**: Deze workflowSession-variabele geeft u de mogelijkheid om de workflow te beheren. De openbare API-documentatie is [hier](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html) beschikbaar

**MetaDataMap**: Alle metagegevens die aan de workflow zijn gekoppeld. Alle procesargumenten die aan de processtap worden doorgegeven, zijn beschikbaar via het object MetaDataMap.[API-documentatie](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

In deze zelfstudie gaan we de bijlagen die zijn toegevoegd aan Adaptief formulier naar het bestandssysteem schrijven als onderdeel van de AEM workflow.

Voor dit gebruik is de volgende Java-klasse geschreven

Laten we deze code eens bekijken

```
@Component(property = { Constants.SERVICE_DESCRIPTION + "=Write Adaptive Form Attachments to File System",
        Constants.SERVICE_VENDOR + "=Adobe Systems",
        "process.label" + "=Save Adaptive Form Attachments to File System" })
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {
     private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
     @Override
    public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)
            throws WorkflowException {
        // TODO Auto-generated method stub
        log.debug("The string I got was ..." + processArguments.get("PROCESS_ARGS", "string").toString());
        String[] params = processArguments.get("PROCESS_ARGS", "string").toString().split(",");
        String attachmentsPath = params[0];
        String saveToLocation = params[1];
        log.debug("The seperator is" + File.separator);
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
 
        String attachmentsFilePath = payloadPath + "/" + attachmentsPath + "/attachments";
        log.debug("The data file path is " + attachmentsFilePath);
 
        ResourceResolver resourceResolver = workflowSession.adaptTo(ResourceResolver.class);
 
        Resource attachmentsNode = resourceResolver.getResource(attachmentsFilePath);
        Iterator<Resource> attachments = attachmentsNode.listChildren();
        while (attachments.hasNext()) {
            Resource attachment = attachments.next();
            String attachmentPath = attachment.getPath();
            String attachmentName = attachment.getName();
 
            log.debug("The attachmentPath is " + attachmentPath + " and the attachmentname is " + attachmentName);
            com.adobe.aemfd.docmanager.Document attachmentDoc = new com.adobe.aemfd.docmanager.Document(attachmentPath,
                    attachment.getResourceResolver());
            try {
                File file = new File(saveToLocation + File.separator + workItem.getId());
                if (!file.exists()) {
                    file.mkdirs();
                }
 
                attachmentDoc.copyToFile(new File(file + File.separator + attachmentName));
 
                log.debug("Saved attachment" + attachmentName);
                attachmentDoc.close();
 
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
 
        }
 
    }
```

Regel 1 - bepaalt de eigenschappen voor onze component. Het process.label bezit is wat u zult zien wanneer het associëren van component OSGi met de processtap zoals aangetoond in één van de hieronder screenshots.

Lijnen 13-15 - De procesargumenten die tot deze component OSGi worden overgegaan zijn verdeeld gebruikend &quot;,&quot;separator. De waarden voor attachPath en saveToLocation worden dan gehaald uit de koordserie.

* BijlagePath - Dit is de zelfde plaats die u in de AanpassingsVorm hebt gespecificeerd toen u de verzendactie van Aangepast Vorm vormde om AEM Workflow aan te halen. Dit is een naam van de map waarvan u wilt dat de bijlagen worden opgeslagen in AEM ten opzichte van de lading van de workflow.

* saveToLocation - dit is de plaats die u de gehechtheid wilt worden bewaard op het dossiersysteem van uw AEM server.

Deze twee waarden worden doorgegeven als procesargumenten, zoals in de onderstaande schermafbeelding wordt getoond.

![ProcessStep](assets/implement-process-step.gif)


Regel 19: bouwen wij dan gehechtheidFilePath. Het pad naar het bijlagebestand is vergelijkbaar

    /var/fd/dashboard/payload/server0/2018-11-19/3EF6ENASOQTHCPLNDYVNAM7OKA_7/Attachments/attachments

* De &quot;Bijlagen&quot; is de naam van de map ten opzichte van de lading van de workflow die is opgegeven toen u de verzendoptie voor het adaptieve formulier hebt geconfigureerd.

   ![verzendopties](assets/af-submit-options.gif)

Lijnen 24-26 - Get ResourceResolver en dan het middel die aan gehechtheidFilePath richten.

De rest van de code leidt tot de voorwerpen van het Document door het kindvoorwerp van de middel te herhalen die aan attachéFilePath richten gebruikend API. Dit documentobject is specifiek voor AEM Forms. Vervolgens wordt de methode copyToFile van het documentobject gebruikt om het documentobject op te slaan.

>[!NOTE]
>
>Aangezien wij het voorwerp van het Document gebruiken dat voor AEM Forms specifiek is, wordt het vereist dat u aemfd-cliënt-sdk gebiedsdeel in uw beven project omvat. De groep-id is com.adobe.aemfd en de artifact-id is aemfd-client-sdk.

#### Samenstellen en implementeren

[Bouw de bundel zoals ](https://helpx.adobe.com/experience-manager/using/maven_arch13.html#BuildtheOSGibundleusingMaven)
[hier beschrevenZorg ervoor de bundel en in actieve staat wordt opgesteld](http://localhost:4502/system/console/bundles)

Maak een workflowmodel. De stap van het proces van de belemmering en van het dalingsproces in het werkschemamodel. Koppel de processtap aan &quot;Aangepaste formulierbijlagen opslaan naar bestandssysteem&quot;.

Geef de benodigde procesargumenten op, gescheiden door een komma. Bijvoorbeeld Bijlagen,c:\\scrappp\\. Het eerste argument is de map waarin uw adaptieve formulierbijlagen worden opgeslagen ten opzichte van de lading van de workflow. Dit moet dezelfde waarde zijn als u hebt opgegeven bij het configureren van de verzendactie van Adaptief formulier. Het tweede argument is de locatie waar u de bijlagen wilt opslaan.

Maak een adaptief formulier. Sleep de component Bestandsbijlagen naar het formulier. Configureer de verzendactie van het formulier om de workflow aan te roepen die in de eerdere stappen is gemaakt. Geef het juiste pad voor de bijlage op.

Sla de instellingen op.

Geef een voorbeeld van het formulier weer. Voeg een aantal bijlagen toe en verzend het formulier. De bijlagen moeten in het bestandssysteem worden opgeslagen op de locatie die u in de workflow hebt opgegeven.

