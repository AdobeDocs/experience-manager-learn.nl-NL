---
title: Aan de slag met AEM headless - Hoofdstuk 7 - AEM inhoudsservices van een mobiele app gebruiken
description: In Hoofdstuk 7 van de zelfstudie wordt de Android Mobile-app uitgevoerd om geschreven inhoud van AEM Content Services te verbruiken.
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '1404'
ht-degree: 0%

---


# Hoofdstuk 7 - AEM inhoudsservices van een mobiele app gebruiken

Hoofdstuk 7 van de zelfstudie gebruikt een systeemeigen Android Mobile-app om inhoud van AEM Content Services te verbruiken.

## De Android Mobile-toepassing

In deze zelfstudie wordt een **eenvoudige systeemeigen Android Mobile-app** gebruikt om inhoud van gebeurtenissen te verbruiken en weer te geven die door AEM Content Services wordt vrijgegeven.

Het gebruik van [Android](https://developer.android.com/) is grotendeels onbelangrijk en de verbruikende mobiele app kan in elk framework voor elk mobiel platform worden geschreven, bijvoorbeeld iOS.

Android wordt gebruikt voor zelfstudie vanwege de mogelijkheid om een Android-emulator in Windows, MacOs en Linux uit te voeren, vanwege de populariteit ervan en omdat deze kan worden geschreven als Java, een taal die AEM ontwikkelaars goed begrijpen.

*De Android Mobile-app van de zelfstudie is **niet**bedoeld om u te vertellen hoe u mobiele Android-apps kunt ontwikkelen of best practices voor Android-ontwikkeling kunt overbrengen, maar om te laten zien hoe AEM Content Services kan worden gebruikt vanuit een mobiele toepassing.*

### De manier waarop AEM Content Services de Mobile App Experience

![Toewijzing van mobiele app aan inhoudsservices](assets/chapter-7/content-services-mapping.png)

1. Het **logo** zoals gedefinieerd door de [!DNL Events API] afbeeldingscomponent **van de** pagina.
1. De **labellijn** zoals gedefinieerd in de [!DNL Events API] tekstcomponent **van de** pagina.
1. Deze lijst **van** Gebeurtenissen wordt afgeleid uit de rangschikking van de Fragmenten van de Inhoud van de Gebeurtenis, die via de gevormde component **van de Lijst van het Fragment van de** Inhoud worden blootgesteld.

## Mobiele-toepassingsdemonstratie

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### De mobiele app configureren voor gebruik buiten de landinstelling

Als AEM Publish niet op **http://localhost:4503** in werking wordt gesteld kunnen de gastheer en de haven in Mobiele App&#39;s worden bijgewerkt [!DNL Settings] om aan het bezit te richten publiceert gastheer/de haven AEM.

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## De mobiele toepassing lokaal uitvoeren

1. Download en installeer de [Android Studio](https://developer.android.com/studio/install) om de Android Emulator te installeren.
1. **Download** het Android- [!DNL APK] bestand [GitHub > Assets > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Android **Studio openen**
   * Bij de eerste keer dat Android Studio wordt gestart, verschijnt de vraag of u de installatie wilt [!DNL Android SDK] uitvoeren. Accepteer de standaardinstellingen en voltooi de installatie.
1. Open Android Studio en selecteer **Profiel of Foutopsporing APK**
1. Selecteer het APK-bestand (**wknd-mobile.x.x.x.apk**) dat u in stap 2 hebt gedownload en klik op **OK**
   * Selecteer Bestaande **** gebruiken als u wordt gevraagd een nieuwe map **te maken of Bestaande****gebruiken. Selecteer Bestaande** gebruiken.
1. Klik bij de eerste start van Android Studio met de rechtermuisknop op **wknd-mobile.x.x.x** in de lijst Projecten en selecteer **Module-instellingen** openen.
   * Selecteer **Android API 29-Platform** onder Modules > wknd-mobile.x.x > tabblad **** Afhankelijkheden. Tik op OK om de wijzigingen te sluiten en op te slaan.
   * Als u dit niet doet, wordt de fout &quot;Selecteer de Android-SDK&quot; weergegeven wanneer u de emulator probeert te starten.
1. Open **AVD Manager** door **Opties > AVD Manager** te selecteren of op het pictogram **AVD Manager** te tikken in de bovenste balk.
1. Klik in het venster **AVD Manager** op **+ Virtueel apparaat maken...** als u nog geen apparaat hebt geregistreerd.
   1. Selecteer links de categorie **Telefoon** .
   1. Selecteer een **pixel 2**.
   1. Klik op de knop **Volgende** .
   1. Selecteer **Q** met **API Niveau 29**.
      * Bij de eerste keer dat AVD Manager wordt gestart, wordt u gevraagd om de API voor versiebeheer te downloaden. Klik op de koppeling Downloaden naast de release Q en voltooi het downloaden en installeren.
   1. Klik op de knop **Volgende** .
   1. Klik op de knop **Voltooien** .
1. Sluit het venster **AVD Manager** .
1. Selecteer in de bovenste menubalk **wknd-mobile.x.x.x** in de vervolgkeuzelijst Configuraties **** uitvoeren/bewerken.
1. Tik op de knop **Uitvoeren** naast de geselecteerde configuratie voor **uitvoeren/bewerken**
1. Selecteer in het pop-upvenster het nieuwe **[!DNL Pixel 2 API 29]** virtuele apparaat en tik op **OK**
1. Als de [!DNL WKND Mobile] app niet direct wordt geladen, zoekt en tikt u op het **[!DNL WKND]** pictogram in het Android-beginscherm in de emulator.
   * Als de emulator wordt gestart maar het scherm van de emulator zwart blijft, tikt u op de **aan/uit** -knop in het gereedschapsvenster van de emulator naast het emulatorvenster.
   * Als u in het virtuele apparaat wilt schuiven, klikt u en houdt u de muisknop ingedrukt en sleept u.
   * Als u de inhoud van AEM wilt vernieuwen, trekt u van boven naar beneden tot de pictogrammen Vernieuwen en laat u de inhoud los.

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## De mobiele toepassingscode

In deze sectie wordt de Android Mobile App-code gemarkeerd die het meest interageert en afhankelijk is van AEM Content Services en die JSON-uitvoer is.

Tijdens het laden maakt de Mobile App `HTTP GET` aan `/content/wknd-mobile/en/api/events.model.json` welk eindpunt van de Diensten van de Inhoud van de AEM wordt gevormd om de inhoud te verstrekken om de Mobiele App te drijven.

Omdat de bewerkbare sjabloon van de API voor gebeurtenissen (`/content/wknd-mobile/en/api/events.model.json`) vergrendeld is, kan de Mobile-app worden gecodeerd om te zoeken naar specifieke informatie op specifieke locaties in de JSON-reactie.

### Codestroom op hoog niveau

1. Als u de [!DNL WKND Mobile] app opent, wordt een `HTTP GET` verzoek ingediend bij de AEM-publicatie op `/content/wknd-mobile/en/api/events.model.json` om de inhoud te verzamelen en de gebruikersinterface van de mobiele app te vullen.
2. Wanneer u de inhoud van AEM ontvangt, worden alle drie weergaveelementen van de Mobile App, het **logo, de labellijn en de gebeurtenislijst**, geïnitialiseerd met de inhoud van AEM.
   * Om aan de AEM inhoud aan het de meningselement van de Mobiele App te binden, wordt JSON die elke AEM component vertegenwoordigt, voorwerp in kaart gebracht aan een POJO van Java, die beurtelings aan het element van de Mening van Android verbindend is.
      * Afbeeldingscomponent JSON → Logo POJO → Logo ImageView
      * Tekstcomponent JSON → TagLine POJO → Text ImageView
      * JSON → Events POJO Events RecyclerView
   * *De mobiele toepassingscode kan de JSON aan POJOs wegens de bekende plaatsen binnen de grotere reactie JSON in kaart brengen. Onthoud dat de JSON-toetsen &quot;image&quot;, &quot;text&quot; en &quot;contentfragmentlist&quot; worden gedicteerd door de namen van de knooppunten van AEM. Als deze knooppuntnamen veranderen, dan zal de Mobiele App breken aangezien het niet zal weten hoe te om de vereiste inhoud van de JSON gegevens te bron.*

#### Het aanhalen van het eindpunt van de Diensten van de Inhoud van de AEM

Hieronder volgt een destillatie van de code in de Mobile App&#39;s `MainActivity` die verantwoordelijk is voor het aanroepen van AEM Content Services om de inhoud te verzamelen die de Mobile App-ervaring aandrijft.

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

`onCreate(..)` is de initialisatiehaak voor de Mobile App, en registreert 3 douane `ViewBinders` verantwoordelijk voor het ontleden van JSON en het binden van de waarden aan de `View` elementen.

`initApp(...)` wordt dan geroepen die het HTTP- GET verzoek aan het eindpunt van de Diensten van de Inhoud van de AEM op AEM publiceert om de inhoud te verzamelen. Na ontvangst van een geldige JSON-respons wordt de JSON-respons doorgegeven aan elke `ViewBinder` die verantwoordelijk is voor het parseren van de JSON en het binden ervan aan de mobiele `View` elementen.

#### De JSON-respons parseren

Nu zullen we naar het `LogoViewBinder`voorbeeld kijken, wat eenvoudig is, maar een aantal belangrijke overwegingen benadrukt.

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

De eerste regel van `bind(...)` navigeert omlaag de JSON-reactie via de toetsen **:items → root → :items** die de AEM Layout Container vertegenwoordigen waaraan de componenten zijn toegevoegd.

Van hier wordt een controle gemaakt op een sleutel genoemd **beeld**, die de component van het Beeld vertegenwoordigt (opnieuw, is het belangrijk deze knoopnaam → sleutel JSON stabiel is). Als dit object bestaat, wordt het gelezen en toegewezen aan de [aangepaste afbeeldingPOJO](#image-pojo) via de Jackson- `ObjectMapper` bibliotheek. De POJO van de afbeelding wordt hieronder besproken.

Ten slotte `src` wordt het logo met de [!DNL Glide] hulplijnbibliotheek in de Android ImageView geladen.

U ziet dat we het AEM schema, de host en de poort (via `aemHost`) aan de AEM Publish-instantie moeten leveren, aangezien AEM Content Services alleen het JCR-pad (dat wil zeggen: `/content/dam/wknd-mobile/images/wknd-logo.png`) aan de inhoud waarnaar wordt verwezen.

#### De POJO van de afbeelding{#image-pojo}

Hoewel optioneel, helpt het gebruik van de [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) of soortgelijke mogelijkheden die door andere bibliotheken zoals Gson worden geboden, complexe JSON-structuren toe te wijzen aan Java POJO&#39;s zonder dat er rechtstreeks met de native JSON-objecten zelf hoeft te worden gewerkt. In dit eenvoudige geval wijzen wij de `src` sleutel van het voorwerp `image` JSON, aan de `src` attributen in POJO van het Beeld direct via de `@JSONProperty` aantekening toe.

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

De POJO van de Gebeurtenis, die het selecteren van vele meer gegevenspunten van het voorwerp JSON vereist, profiteert van deze techniek meer dan het eenvoudige Beeld, wat wij allen willen is het `src`.

## Ontdek de Mobile App Experience

Nu u weet hoe AEM Content Services native mobiele ervaring kan aansturen, kunt u gebruiken wat u hebt geleerd om de volgende stappen uit te voeren en uw wijzigingen te bekijken in de mobiele app.

Na elke stap vernieuwt u de mobiele app en controleert u de update van de mobiele ervaring.

1. **Nieuw[!DNL Event]inhoudsfragment maken en publiceren**
1. Publiceren van een **bestaand[!DNL Event]inhoudsfragment ongedaan maken**
1. Een update naar de **taglijn publiceren**

## Gefeliciteerd

**U hebt de AEM zelfstudie zonder koppen voltooid!**

Voor meer informatie over Content Services en AEM als Headless CMS raadpleegt u de documentatie AEM Adobe en activering:

* [Inhoudsfragmenten gebruiken](../sites/content-fragments/understand-content-fragments-and-experience-fragments.md)
* [Gebruikershandleiding voor WCM Core Components](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html)
* [AEM WCM Core Components Component Library](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCM Core Components GitHub Project](https://github.com/adobe/aem-core-wcm-components)
* [AEM WCM Core-componenten - Vraag het de expert](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-services.html)
* [Codevoorbeeld van Componentexportfunctie](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/bundle/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
