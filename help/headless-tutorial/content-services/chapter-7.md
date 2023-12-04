---
title: Hoofdstuk 7 - AEM Inhoudsservices van een mobiele app gebruiken - Inhoudsservices
description: In Hoofdstuk 7 van de zelfstudie wordt de Android Mobile-app uitgevoerd om geschreven inhoud van AEM Content Services te verbruiken.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
duration: 573
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1350'
ht-degree: 0%

---

# Hoofdstuk 7 - AEM inhoudsservices van een mobiele app gebruiken

Hoofdstuk 7 van de zelfstudie gebruikt een systeemeigen Android Mobile-app om inhoud van AEM Content Services te verbruiken.

## De Android Mobile-toepassing

Deze zelfstudie gebruikt een **eenvoudige systeemeigen Android Mobile-app** om de inhoud van de Gebeurtenis te verbruiken en te tonen die door AEM Diensten van de Inhoud wordt blootgesteld.

Het gebruik van [Android](https://developer.android.com/) is grotendeels onbelangrijk en de verbruikende mobiele app kan in elk raamwerk voor elk mobiel platform, bijvoorbeeld iOS, worden geschreven.

Android wordt gebruikt voor zelfstudie vanwege de mogelijkheid om een Android-emulator in Windows, MacOs en Linux uit te voeren, vanwege de populariteit ervan en omdat deze kan worden geschreven als Java, een taal die AEM ontwikkelaars goed begrijpen.

*De Android Mobile-app van de zelfstudie is **niet**bedoeld om u te leren hoe u mobiele Android-apps kunt ontwikkelen of best practices voor Android-ontwikkeling kunt overbrengen, maar om te tonen hoe AEM Content Services kan worden gebruikt via een mobiele toepassing.*

### De manier waarop AEM Content Services de Mobile App Experience

![Toewijzing van mobiele app aan inhoudsservices](assets/chapter-7/content-services-mapping.png)

1. De **logo** zoals gedefinieerd door de [!DNL Events API] pagina&#39;s **Afbeeldingscomponent**.
1. De **labellijn** zoals gedefinieerd op de [!DNL Events API] pagina&#39;s **Tekstcomponent**.
1. Dit **Gebeurtenislijst** is afgeleid van de rangschikking van de Fragmenten van de Inhoud van de Gebeurtenis, die via gevormd worden blootgesteld **component Lijst met inhoudsfragmenten**.

## Mobiele-toepassingsdemonstratie

>[!VIDEO](https://video.tv.adobe.com/v/28345?quality=12&learn=on)

### De mobiele app configureren voor gebruik buiten de landinstelling

Als AEM Publiceren niet is ingeschakeld **http://localhost:4503** de host en poort kunnen worden bijgewerkt in de mobiele app [!DNL Settings] om naar de eigenschap AEM Publish host/port te wijzen.

>[!VIDEO](https://video.tv.adobe.com/v/28344?quality=12&learn=on)

## De mobiele toepassing lokaal uitvoeren

1. Download en installeer de [Android Studio](https://developer.android.com/studio/install) om de Android Emulator te installeren.
1. **Downloaden** de Android [!DNL APK] file [GitHub > Middelen > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Openen **Android Studio**
   * Als Android Studio voor het eerst wordt gestart, wordt u gevraagd om het programma [!DNL Android SDK] aanwezig zal zijn. Accepteer de standaardinstellingen en voltooi de installatie.
1. Open Android Studio en selecteer **APK voor profiel of foutopsporing**
1. Selecteer het APK-bestand (**wknd-mobile.x.x.x.apk**) gedownload in Stap 2 en klik op **OK**
   * Indien ertoe aangezet **Een nieuwe map maken**, of **Bestaande gebruiken**, selecteert u **Bestaande gebruiken**.
1. Klik met de rechtermuisknop op de knop **wknd-mobile.x.x.x** in de lijst Projecten en selecteer **Module-instellingen openen**.
   * Onder **Modules > wknd-mobile.x.x.x > tabblad Afhankelijkheden**, selecteert u **Android API 29-platform**. Tik op OK om de wijzigingen te sluiten en op te slaan.
   * Als u dit niet doet, wordt de fout &quot;Selecteer de Android-SDK&quot; weergegeven wanneer u de emulator probeert te starten.
1. Open de **AVD Manager** door **Gereedschappen > AVD Manager** of tikken op de knop **AVD Manager** in de bovenste balk.
1. In de **AVD Manager** venster, klikt u op **+ Virtueel apparaat maken...** als u nog geen apparaat hebt geregistreerd.
   1. Selecteer in het linkergedeelte de optie **Telefoon** categorie.
   1. Selecteer een **Pixel 2**.
   1. Klik op de knop **Volgende** knop.
   1. Selecteren **Q** with **API-niveau 29**.
      * Wanneer u AVD Manager voor de eerste keer start, wordt u gevraagd de versioned API te downloaden. Klik op de koppeling Downloaden naast de release Q en voltooi het downloaden en installeren.
   1. Klik op de knop **Volgende** knop.
   1. Klik op de knop **Voltooien** knop.
1. Sluit het dialoogvenster **AVD Manager** venster.
1. Selecteer in de bovenste menubalk **wknd-mobile.x.x.x** van de **Configuraties uitvoeren/bewerken** vallen.
1. Tik op de knop **Uitvoeren** naast de geselecteerde **Configuratie uitvoeren/bewerken**
1. Selecteer in het pop-upmenu de nieuwe **[!DNL Pixel 2 API 29]** virtueel apparaat en tikken **OK**
1. Als de [!DNL WKND Mobile] de toepassing laadt, zoekt en tikt niet onmiddellijk op de **[!DNL WKND]** uit het Android-beginscherm in de emulator.
   * Als de emulator wordt gestart maar het scherm van de emulator zwart blijft, tikt u op de **macht** in het gereedschapsvenster van de emulator naast het emulatorvenster.
   * Klik en sleep om binnen het virtuele apparaat te schuiven.
   * Als u de inhoud van AEM wilt vernieuwen, trekt u van boven naar beneden totdat het pictogram Vernieuwen wordt weergegeven en geeft u de inhoud vrij.

>[!VIDEO](https://video.tv.adobe.com/v/28341?quality=12&learn=on)

## De mobiele toepassingscode

In deze sectie wordt de Android Mobile App-code gemarkeerd die het meest interageert en afhankelijk is van AEM Content Services en die JSON-uitvoer is.

Tijdens het laden maakt de Mobile App `HTTP GET` tot `/content/wknd-mobile/en/api/events.model.json` Dit is het eindpunt voor AEM Content Services dat is geconfigureerd om de inhoud te leveren waarmee de mobiele app wordt aangedreven.

Omdat de bewerkbare sjabloon van de API voor gebeurtenissen (`/content/wknd-mobile/en/api/events.model.json`) is vergrendeld, kan de Mobile-app worden gecodeerd om te zoeken naar specifieke informatie op specifieke locaties in de JSON-reactie.

### Codestroom op hoog niveau

1. Het openen van de [!DNL WKND Mobile] App roept een `HTTP GET` verzoek aan de AEM op `/content/wknd-mobile/en/api/events.model.json` om de inhoud te verzamelen om de gebruikersinterface van de mobiele app te vullen.
2. Wanneer u de inhoud van AEM ontvangt, wordt met elk van de drie weergaveelementen van de Mobile-app het **logo, labellijn en gebeurtenislijst**, worden geïnitialiseerd met de inhoud van AEM.
   * Om aan de AEM inhoud aan het de meningselement van de Mobiele App te binden, wordt JSON die elke AEM component vertegenwoordigt, voorwerp in kaart gebracht aan een POJO van Java, die beurtelings aan het element van de Mening van Android verbindend is.
      * Afbeeldingscomponent JSON → Logo POJO → Logo ImageView
      * Tekstcomponent JSON → TagLine POJO → Text ImageView
      * JSON → Events POJO Events RecyclerView
   * *De mobiele toepassingscode kan de JSON aan POJOs wegens de bekende plaatsen binnen de grotere reactie JSON in kaart brengen. Onthoud dat de JSON-toetsen &quot;image&quot;, &quot;text&quot; en &quot;contentfragmentlist&quot; worden gedicteerd door de namen van de knooppunten van AEM. Als deze knooppuntnamen veranderen, dan zal de Mobiele App breken aangezien het niet zal weten hoe te om de vereiste inhoud van de JSON gegevens te bron.*

#### Het aanhalen van het eindpunt van de Diensten van de Inhoud van de AEM

Hieronder volgt een destillatie van de code in de Mobile App&#39;s `MainActivity` verantwoordelijk voor het aanroepen van AEM Content Services om de inhoud te verzamelen die de Mobile App-ervaring stimuleert.

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

`onCreate(..)` is de initialisatiehaak voor Mobiele App, en registreert 3 douane `ViewBinders` verantwoordelijk voor het parseren van de JSON en het binden van de waarden aan de `View` elementen.

`initApp(...)` wordt dan geroepen die het HTTP- GET verzoek aan het eindpunt van de Diensten van de Inhoud van de AEM bij AEM Publish doet om de inhoud te verzamelen. Bij ontvangst van een geldige JSON-respons wordt de JSON-respons doorgegeven aan elke patiënt `ViewBinder` die verantwoordelijk is voor het parseren van de JSON en het binden ervan aan de mobiele telefoon `View` elementen.

#### De JSON-respons parseren

Hierna bekijken we de `LogoViewBinder`, wat eenvoudig is, maar een aantal belangrijke overwegingen benadrukt.

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

De eerste regel van `bind(...)` navigeert onderaan de Reactie van JSON via de sleutels **:items → root → items** die staat voor de AEM Layout Container waaraan de componenten zijn toegevoegd.

Hier wordt gecontroleerd op een sleutel met de naam **image**, die staat voor de component Image (het is ook hier belangrijk dat deze knooppuntnaam → JSON-sleutel stabiel is). Als dit object bestaat, wordt het gelezen en toegewezen aan de [aangepaste afbeeldingspOJO](#image-pojo) via Jackson `ObjectMapper` bibliotheek. De POJO van de afbeelding wordt hieronder besproken.

Tot slot de `src` wordt met de klasse [!DNL Glide] helperbibliotheek.

Bericht dat wij het AEM schema, de gastheer en de haven (via `aemHost`) naar de AEM Publish-instantie omdat AEM Content Services alleen het JCR-pad (dat wil zeggen: `/content/dam/wknd-mobile/images/wknd-logo.png`) aan de inhoud waarnaar wordt verwezen.

#### De afbeeldingspo{#image-pojo}

Het gebruik van de [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) of vergelijkbare mogelijkheden die andere bibliotheken zoals Gson bieden, helpen complexe JSON-structuren toe te wijzen aan Java POJO&#39;s zonder dat ze rechtstreeks met de native JSON-objecten zelf moeten werken. In dit eenvoudige geval geven we de `src` van de `image` JSON-object, naar de `src` rechtstreeks via het `@JSONProperty` aantekening.

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

Na elke stap vernieuwt u de Mobile-app en controleert u de update van de mobiele ervaring.

1. Maken en publiceren **new [!DNL Event] Inhoudsfragment**
1. Publiceren van een **bestaand [!DNL Event] Inhoudsfragment**
1. Een update publiceren naar de **Lijn met tag**

## Gefeliciteerd

**U hebt de AEM zelfstudie zonder koppen voltooid!**

Voor meer informatie over AEM Content Services en AEM als een Headless CMS raadpleegt u de andere documentatie en materialen voor activering van de Adobe:

* [Inhoudsfragmenten gebruiken](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [Gebruikershandleiding voor WCM Core Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [AEM WCM Core Components Component Library](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCM Core Components GitHub Project](https://github.com/adobe/aem-core-wcm-components)
* [Codevoorbeeld van Componentexportfunctie](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
