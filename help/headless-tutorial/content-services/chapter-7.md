---
title: Hoofdstuk 7 - AEM Inhoudsservices van een mobiele app gebruiken - Inhoudsservices
description: In Hoofdstuk 7 van de zelfstudie wordt de Android Mobile-app uitgevoerd om geschreven inhoud van AEM Content Services te verbruiken.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1416'
ht-degree: 0%

---


# Hoofdstuk 7 - AEM inhoudsservices van een mobiele app gebruiken

Hoofdstuk 7 van de zelfstudie gebruikt een systeemeigen Android Mobile-app om inhoud van AEM Content Services te verbruiken.

## De Android Mobile-toepassing

Deze zelfstudie gebruikt een **eenvoudige systeemeigen Android Mobile-app** om Event-inhoud te verbruiken en weer te geven die door AEM Content Services wordt aangeboden.

Het gebruik van [Android](https://developer.android.com/) is grotendeels onbelangrijk en de verbruikende mobiele app kan in elk framework worden geschreven voor elk mobiel platform, bijvoorbeeld iOS.

Android wordt gebruikt voor zelfstudie vanwege de mogelijkheid om een Android-emulator in Windows, MacOs en Linux uit te voeren, vanwege de populariteit ervan en omdat deze kan worden geschreven als Java, een taal die AEM ontwikkelaars goed begrijpen.

*De Android Mobile-app van de zelfstudie is ****niet bedoeld als instructie voor het ontwikkelen van mobiele Android-apps of het overbrengen van best practices voor Android-ontwikkeling, maar om te laten zien hoe AEM Content Services kan worden gebruikt vanuit een mobiele toepassing.*

### De manier waarop AEM Content Services de Mobile App Experience

![Toewijzing van mobiele app aan inhoudsservices](assets/chapter-7/content-services-mapping.png)

1. Het **logo** zoals gedefinieerd door de **Image-component** van de pagina [!DNL Events API].
1. De **labelregel** zoals gedefinieerd op de [!DNL Events API] pagina&#39;s **Tekstcomponent**.
1. Dit **Gebeurtenislijst** wordt afgeleid van de rangschikking van de Fragmenten van de Inhoud van de Gebeurtenis, die via de gevormde **component van de Lijst van het Fragment van de Inhoud** worden blootgesteld.

## Mobiele-toepassingsdemonstratie

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### De mobiele app configureren voor gebruik buiten de landinstelling

Als AEM-publicatie niet wordt uitgevoerd op **http://localhost:4503**, kunnen de host en poort worden bijgewerkt in [!DNL Settings] van de mobiele app om naar de eigenschap AEM Publish-host/-poort te verwijzen.

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## De mobiele toepassing lokaal uitvoeren

1. Download en installeer de [Android Studio](https://developer.android.com/studio/install) om de Android-emulator te installeren.
1. **Het** Android- [!DNL APK] bestand  [GitHub > Middelen > wknd-mobile.x.x.xapk downloaden](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. **Android Studio** openen
   * Bij de eerste keer dat Android Studio wordt gestart, wordt u gevraagd [!DNL Android SDK] te installeren. Accepteer de standaardinstellingen en voltooi de installatie.
1. Open Android Studio en selecteer **Profiel of Foutopsporing APK**
1. Selecteer het APK-bestand (**wknd-mobile.x.x.apk**) dat u in stap 2 hebt gedownload en klik op **OK**
   * Als ertoe aangezet om **een Nieuwe Omslag te creëren**, of **Bestaande gebruiken**, uitgezocht **Bestaande gebruiken**.
1. Bij de eerste start van Android Studio klikt u met de rechtermuisknop op **wknd-mobile.x.x** in de lijst Projecten en selecteert u **Module-instellingen openen**.
   * Selecteer **Android API 29-Platform** onder **Modules > wknd-mobile.x.x > Afhankelijkheden tab**. Tik op OK om de wijzigingen te sluiten en op te slaan.
   * Als u dit niet doet, wordt de fout &quot;Selecteer de Android-SDK&quot; weergegeven wanneer u de emulator probeert te starten.
1. Open **AVD Manager** door **Gereedschappen > AVD Manager** te selecteren of op het pictogram **AVD Manager** in de bovenste balk te tikken.
1. Klik in het venster **AVD Manager** op **+ Virtuele apparaat maken...** als u nog geen apparaat hebt geregistreerd.
   1. Selecteer links de categorie **Telefoon**.
   1. Selecteer een **pixel 2**.
   1. Klik op de knop **Volgende**.
   1. Selecteer **Q** met **API Level 29**.
      * Bij de eerste keer dat AVD Manager wordt gestart, wordt u gevraagd om de API voor versiebeheer te downloaden. Klik op de koppeling Downloaden naast de release Q en voltooi het downloaden en installeren.
   1. Klik op de knop **Volgende**.
   1. Klik op de knop **Voltooien**.
1. Sluit het venster **AVD Manager**.
1. Selecteer **wknd-mobile.x.x** in de bovenste menubalk in de vervolgkeuzelijst **Configuraties uitvoeren/bewerken**.
1. Tik op de **Run**-knop naast de geselecteerde **Run/Edit Configuration**
1. Selecteer in het pop-upvenster het nieuwe **[!DNL Pixel 2 API 29]** virtuele apparaat en tik **OK**
1. Als de [!DNL WKND Mobile]-toepassing niet direct wordt geladen, zoekt en tikt u op het **[!DNL WKND]**-pictogram in het Android-beginscherm in de emulator.
   * Als de emulator wordt gestart maar het scherm van de emulator zwart blijft, tikt u op de **power** knop in het gereedschapsvenster van de emulator naast het emulatorvenster.
   * Als u in het virtuele apparaat wilt schuiven, klikt u en houdt u de muisknop ingedrukt en sleept u.
   * Als u de inhoud van AEM wilt vernieuwen, trekt u van boven naar beneden tot het pictogram Vernieuwen
en geeft deze weer.

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## De mobiele toepassingscode

In deze sectie wordt de Android Mobile App-code gemarkeerd die het meest interageert en afhankelijk is van AEM Content Services en die JSON-uitvoer is.

Tijdens het laden maakt de mobiele toepassing `HTTP GET` tot `/content/wknd-mobile/en/api/events.model.json`. Dit is het eindpunt voor AEM Content Services dat is geconfigureerd om de inhoud te leveren waarmee de mobiele toepassing kan worden aangedreven.

Omdat de bewerkbare sjabloon van de API voor gebeurtenissen (`/content/wknd-mobile/en/api/events.model.json`) vergrendeld is, kan de mobiele toepassing worden gecodeerd om te zoeken naar specifieke informatie op specifieke locaties in de JSON-reactie.

### Codestroom op hoog niveau

1. Als u de [!DNL WKND Mobile]-app opent, wordt een `HTTP GET`-verzoek naar de AEM-publicatie op `/content/wknd-mobile/en/api/events.model.json` aangeroepen om de inhoud te verzamelen om de gebruikersinterface van de mobiele app te vullen.
2. Na ontvangst van de inhoud van AEM, worden elk van de drie weergave-elementen van de Mobile App, het **logo, de labellijn en de gebeurtenislijst**, geïnitialiseerd met de inhoud van AEM.
   * Om aan de AEM inhoud aan het de meningselement van de Mobiele App te binden, wordt JSON die elke AEM component vertegenwoordigt, voorwerp in kaart gebracht aan een POJO van Java, die beurtelings aan het element van de Mening van Android verbindend is.
      * Afbeeldingscomponent JSON → Logo POJO → Logo ImageView
      * Tekstcomponent JSON → TagLine POJO → Text ImageView
      * JSON → Events POJO Events RecyclerView
   * *De mobiele toepassingscode kan de JSON aan POJOs wegens de bekende plaatsen binnen de grotere reactie JSON in kaart brengen. Onthoud dat de JSON-toetsen &quot;image&quot;, &quot;text&quot; en &quot;contentfragmentlist&quot; worden gedicteerd door de namen van de knooppunten van AEM. Als deze knooppuntnamen veranderen, dan zal de Mobiele App breken aangezien het niet zal weten hoe te om de vereiste inhoud van de JSON gegevens te bron.*

#### Het aanhalen van het eindpunt van de Diensten van de Inhoud van de AEM

Hieronder volgt een destillatie van de code in de `MainActivity` van de Mobile-app die verantwoordelijk is voor het aanroepen van AEM Content Services om de inhoud te verzamelen die de Mobile App-ervaring aandrijft.

```
protected void onCreate(Bundle savedInstanceState) {
    ...
    // Create the custom objects that will map the JSON -> POJO -> View elements
    final List<ViewBinder> viewBinders = new ArrayList<>();

    viewBinders.add(new LogoViewBinder(this, getAemHost(), (ImageView) findViewById(R.id.logo)));
    viewBinders.add(new TagLineViewBinder(this, (TextView) findViewById(R.id.tagLine)));
    viewBinders.add(new EventsViewBinder(this, getAemHost(), (RecyclerView) findViewById(R.id.eventsList)));
    ...
    initApp(viewBinders);
}

private void initApp(final List<ViewBinder> viewBinders) {
    final String aemUrl = getAemUrl(); // -> http://localhost:4503/content/wknd-mobile/en/api/events.mobile.json
    JsonObjectRequest request = new JsonObjectRequest(aemUrl, null,
        new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                for (final ViewBinder viewBinder : viewBinders) {
                    viewBinder.bind(response);
                }
            }
        },
        ...
    );
}
```

`onCreate(..)` is de initialisatiehaak voor de Mobile App, en registreert 3 douane  `ViewBinders` verantwoordelijk voor het ontleden van JSON en het binden van de waarden aan de  `View` elementen.

`initApp(...)` wordt dan geroepen die het HTTP- GET verzoek aan het eindpunt van de Diensten van de Inhoud van de AEM op AEM publiceert om de inhoud te verzamelen. Na ontvangst van een geldige JSON-respons wordt de JSON-respons doorgegeven aan elke `ViewBinder` die verantwoordelijk is voor het parseren van de JSON en het binden ervan aan de mobiele `View`-elementen.

#### De JSON-respons parseren

Hierna bekijken we `LogoViewBinder`, wat eenvoudig is, maar een aantal belangrijke overwegingen benadrukt.

```
public class LogoViewBinder implements ViewBinder {
    ...
    public void bind(JSONObject jsonResponse) throws JSONException, IOException {
        final JSONObject components = jsonResponse.getJSONObject(":items").getJSONObject("root").getJSONObject(":items");

        if (components.has("image")) {
            final Image image = objectMapper.readValue(components.getJSONObject("image").toString(), Image.class);

            final String imageUrl = aemHost + image.getSrc();
            Glide.with(context).load(imageUrl).into(logo);
        } else {
            Log.d("WKND", "Missing Logo");
        }
    }
}
```

De eerste regel van `bind(...)` navigeert naar beneden de JSON-respons via de toetsen **:items → root → :items** die de AEM Layout Container vertegenwoordigt waaraan de componenten zijn toegevoegd.

Van hier wordt een controle gemaakt op een sleutel genoemd **image**, die de component van het Beeld vertegenwoordigt (opnieuw, is het belangrijk deze knooppuntnaam → sleutel JSON stabiel is). Als dit object bestaat, wordt het gelezen en toegewezen aan de [aangepaste afbeeldingPOJO](#image-pojo) via de Jackson `ObjectMapper`-bibliotheek. De POJO van de afbeelding wordt hieronder besproken.

Ten slotte wordt het `src` van het logo in de Android ImageView geladen met behulp van de hulpbibliotheek [!DNL Glide].

U ziet dat we het AEM schema, de host en de poort (via `aemHost`) naar de AEM Publish-instantie moeten leveren, omdat AEM Content Services alleen het JCR-pad (dat wil zeggen: `/content/dam/wknd-mobile/images/wknd-logo.png`) aan de inhoud waarnaar wordt verwezen.

#### De afbeeldingspo{#image-pojo}

Hoewel optioneel, helpt het gebruik van de [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) of soortgelijke mogelijkheden die door andere bibliotheken zoals Gson worden geboden, complexe JSON-structuren toe te wijzen aan Java POJO&#39;s zonder dat het tradium bestaat om rechtstreeks met de native JSON-objecten zelf om te gaan. In dit eenvoudige geval wijzen wij `src` sleutel van `image` JSON voorwerp, aan het `src` attribuut in de POJO van het Beeld direct via `@JSONProperty` annotatie toe.

```
package com.adobe.aem.guides.wknd.mobile.android.models;

import com.fasterxml.jackson.annotation.JsonProperty;

public class Image {
    @JsonProperty(value = "src")
    private String src;

    public String getSrc() {
        return src;
    }
}
```

De POJO van de Gebeurtenis, die het selecteren van vele meer gegevenspunten van het voorwerp JSON vereist, profiteert van deze techniek meer dan het eenvoudige Beeld, wat wij allen willen is `src`.

## Ontdek de Mobile App Experience

Nu u weet hoe AEM Content Services native mobiele ervaring kan aansturen, kunt u gebruiken wat u hebt geleerd om de volgende stappen uit te voeren en uw wijzigingen te bekijken in de mobiele app.

Na elke stap vernieuwt u de mobiele app en controleert u de update van de mobiele ervaring.

1. **nieuw [!DNL Event] Inhoudsfragment** maken
1. Publiceren van een **bestaand [!DNL Event] inhoudsfragment** ongedaan maken
1. Publiceer een update aan **Taglijn**

## Gefeliciteerd

**U hebt de AEM zelfstudie zonder koppen voltooid!**

Voor meer informatie over Content Services en AEM als Headless CMS raadpleegt u de documentatie AEM Adobe en activering:

* [Inhoudsfragmenten gebruiken](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [Gebruikershandleiding voor WCM Core Components](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html)
* [AEM WCM Core Components Component Library](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCM Core Components GitHub Project](https://github.com/adobe/aem-core-wcm-components)
* [AEM WCM Core-componenten - Vraag het de expert](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-services.html)
* [Codevoorbeeld van Componentexportfunctie](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/bundle/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
