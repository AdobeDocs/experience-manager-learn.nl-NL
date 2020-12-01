---
title: LDAP gebruiken met Aem Forms Workflow
seo-title: LDAP gebruiken met Aem Forms Workflow
description: AEM Forms-workflowtaak toewijzen aan de manager van de verzender
seo-description: AEM Forms-workflowtaak toewijzen aan de manager van de verzender
feature: adaptive-forms,workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
uuid: 3e32c3a7-387f-4652-8a94-4e6aa6cd5ab8
discoiquuid: 671872b3-3de0-40da-9691-f8b7e88a9443
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 0%

---


# LDAP gebruiken met AEM Forms Workflow

AEM Forms-workflowtaak toewijzen aan de manager van de verzender.

Als u Adaptief formulier gebruikt in AEM workflow, wilt u dynamisch een taak toewijzen aan de manager van de verzender van het formulier. Om dit gebruiksgeval te verwezenlijken, zullen wij AEM met Ldap moeten vormen.

De stappen die nodig zijn om AEM te configureren met LDAP worden hier beschreven in [detail.](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)

Voor dit artikel, maak ik configuratiedossiers vast die in het vormen AEM met Adobe Ldap worden gebruikt. Deze bestanden worden opgenomen in het pakket dat u kunt importeren met pakketbeheer.

In de onderstaande schermafbeelding halen we alle gebruikers op die tot een bepaalde kostenplaats behoren. Als u alle gebruikers in de LDAP wilt ophalen, kunt u het extra filter niet gebruiken.

![LDAP-configuratie](assets/costcenterldap.gif)

In de onderstaande schermafbeelding wijzen we de groepen toe aan de gebruikers die van LDAP in AEM zijn opgehaald. U ziet welke groep met formulieren gebruikers is toegewezen aan de geïmporteerde gebruikers. De gebruiker moet lid zijn van deze groep voor interactie met AEM Forms. We slaan de eigenschap manager ook op onder het profiel/manager-knooppunt in AEM.

![Synchandler](assets/synchandler.gif)

Nadat u LDAP hebt geconfigureerd en gebruikers in AEM hebt geïmporteerd, kunnen we een workflow maken die de taak toewijst aan de manager van de verzenders. In het kader van dit artikel hebben we een eenvoudige goedkeuringsworkflow in één stap ontwikkeld.

De eerste stap in de workflow stelt de waarde van de eerste stap in op Nee. De bedrijfsregel in het adaptieve formulier schakelt het venster &quot;Details verzenden&quot; uit en geeft het deelvenster &quot;Goedgekeurd door&quot; weer op basis van de beginwaarde.

De tweede stap wijst de taak toe aan de manager van de verzender. De manager van de verzender wordt met de aangepaste code opgehaald.

![Taak toewijzen](assets/assigntask.gif)

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

Lees dit artikel om uw eigen [ ParticipantChooser uit te voeren.](https://helpx.adobe.com/experience-manager/using/dynamic-steps.html)

Om dit op uw systeem te testen (Voor de Medewerkers van Adobe kunt u dit steekproef uit de doos gebruiken)

* [Download en implementeer de setvalue-bundel](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dit is de aangepaste OSGI-bundel voor het instellen van de eigenschap van de manager.
* [Download en installeer de DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Importeer de aan dit artikel gekoppelde middelen in AEM met behulp van pakketbeheer](assets/aem-forms-ldap.zip). Als onderdeel van dit pakket worden de LDAP-configuratiebestanden, de workflow en een adaptief formulier opgenomen.
* Configureer AEM met uw LDAP met de juiste LDAP-referenties.
* Meld u aan bij AEM met uw LDAP-referenties.
* Open [timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Vul het formulier in en verzend het.
* De manager van de verzender moet het formulier ter controle krijgen.

>[!NOTE]
>
>Deze aangepaste code voor het uitpakken van de naam van de manager is getest op basis van Adobe LDAP. Als u deze code uitvoert tegen een andere LDAP, zult u uw eigen getParticipant implementatie moeten wijzigen of schrijven om de naam van de manager te krijgen.
