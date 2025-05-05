---
title: LDAP gebruiken met AEM Forms Workflow
description: AEM Forms-workflowtaak toewijzen aan de manager van de verzender
feature: Adaptive Forms, Workflow
topic: Integrations
role: Developer
version: Experience Manager 6.4, Experience Manager 6.5
level: Intermediate
exl-id: 2e9754ff-49fe-4260-b911-796bcc4fd266
last-substantial-update: 2021-09-18T00:00:00Z
duration: 111
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 0%

---

# LDAP gebruiken met AEM Forms Workflow

AEM Forms-workflowtaak toewijzen aan de manager van de verzender.

Als u Adaptief formulier gebruikt in de AEM-workflow, wilt u een taak dynamisch toewijzen aan de manager van de verzender van het formulier. Voor dit gebruiksgeval moeten we AEM configureren met LDAP.

De stappen nodig om AEM met LDAP te vormen worden verklaard hier in [ detail.](https://helpx.adobe.com/nl/experience-manager/6-5/sites/administering/using/ldap-config.html)

Voor dit artikel, maak ik configuratiedossiers vast die in het vormen van AEM met Adobe LDAP worden gebruikt. Deze bestanden worden opgenomen in het pakket dat u kunt importeren met pakketbeheer.

In de onderstaande schermafbeelding halen we alle gebruikers op die tot een bepaalde kostenplaats behoren. Als u alle gebruikers in de LDAP wilt ophalen, kunt u het extra filter niet gebruiken.

![ Configuratie LDAP ](assets/costcenterldap.gif)

In de onderstaande schermafbeelding wijzen we de groepen toe aan de gebruikers die van LDAP naar AEM zijn opgehaald. U ziet welke groep met formulieren gebruikers is toegewezen aan de geïmporteerde gebruikers. De gebruiker moet lid zijn van deze groep voor interactie met AEM Forms. We slaan de manager-eigenschap ook op onder het profiel/manager-knooppunt in AEM.

![ Synchandler ](assets/synchandler.gif)

Nadat u LDAP hebt geconfigureerd en gebruikers naar AEM hebt geïmporteerd, kunnen we een workflow maken die de taak toewijst aan de manager van de verzenders. In het kader van dit artikel hebben we een eenvoudige goedkeuringsworkflow in één stap ontwikkeld.

De eerste stap in de workflow stelt de waarde van de eerste stap in op Nee. De bedrijfsregel in het adaptieve formulier schakelt het venster &quot;Details verzenden&quot; uit en geeft het deelvenster &quot;Goedgekeurd door&quot; weer op basis van de beginwaarde.

De tweede stap wijst de taak toe aan de manager van de verzender. De manager van de verzender wordt met de aangepaste code opgehaald.

![ wijs Taak ](assets/assigntask.gif) toe

```java
public String getParticipant(WorkItem workItem, WorkflowSession wfSession, MetaDataMap arg2) throws WorkflowException{
resourceResolver = wfSession.adaptTo(ResourceResolver.class);
UserManager userManager = resourceResolver.adaptTo(UserManager.class);
Authorizable workflowInitiator = userManager.getAuthorizable(workItem.getWorkflow().getInitiator());
.
.
String managerPorperty = workflowInitiator.getProperty("profile/manager")[0].getString();
.
.

}
```

Het codefragment is verantwoordelijk om managers identiteitskaart te halen en de taak toe te wijzen aan de manager.

We krijgen de greep van de persoon die de workflow heeft gestart. Wij krijgen dan de waarde van het managerbezit.

Afhankelijk van hoe het managerbezit in uw LDAP wordt opgeslagen, kunt u wat koordmanipulatie moeten doen om manageridentiteitskaart te krijgen.

Gelieve te lezen dit artikel om uw eigen [ ParticipantChooser uit te voeren.](https://helpx.adobe.com/experience-manager/using/dynamic-steps.html)

Om dit op uw systeem te testen (Voor Adobe-werknemers kunt u dit voorbeeld uit de doos gebruiken)

* [ Download en stel de setvalue bundel ](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) op. Dit is de aangepaste OSGI-bundel voor het instellen van de eigenschap van de manager.
* [Download en installeer de DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [ de Invoer Assets verbonden aan dit artikel in AEM gebruikend pakketmanager ](assets/aem-forms-ldap.zip).Included als deel van dit pakket zijn LDAP configuratiedossiers, werkschema, en een adaptieve vorm.
* Configureer AEM met uw LDAP met behulp van de juiste LDAP-referenties.
* Meld u aan bij AEM met uw LDAP-referenties.
* Open [ timeoffrequestform ](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Vul het formulier in en verzend het.
* De manager van de verzender moet het formulier ter controle krijgen.

>[!NOTE]
>
>Deze aangepaste code voor het uitpakken van de naam van de manager is getest met Adobe LDAP. Als u deze code uitvoert tegen een andere LDAP, zult u uw eigen getParticipant implementatie moeten wijzigen of schrijven om de naam van de manager te krijgen.
