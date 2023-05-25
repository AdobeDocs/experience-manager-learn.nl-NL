---
title: De JSON ophalen van het adaptieve formulier dat moet worden ingesloten
description: Gebruik de API om de zoon van het adaptieve formulier op te halen
feature: Adaptive Forms
version: 6.5
kt: 13285
topic: Development
role: User
level: Intermediate
source-git-commit: 6aa3dff44a7e6f1f8ac896e30319958d84ecf57f
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---


# De JSON van het formulier ophalen

Meld u aan bij de AEM Forms-auteur en maak een nieuwe adapter met de **Leeg met kerncomponenten** sjabloon. Publiceer het formulier naar uw publicatie-exemplaar.

Als u het formulier wilt insluiten, haalt u eerst de json van het adaptieve formulier op door een get call uit te voeren met onze publicatieserver.

Het volgende codefragment haalt de json van het adaptieve formulier genaamd **contactus**

```javascript
const getForm = async () => {
        const resp = await fetch('/content/forms/af/contactus/jcr:content/guideContainer.model.json');
        let formJSON = await resp.json();
        console.log(formJSON);
        setForm(formJSON);
      }
```

De volledige code van de component Contactfunctie wordt hieronder gegeven

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Panel from './components/Panel';
import { useState,useEffect } from "react";
import { AdaptiveForm } from "@aemforms/af-react-renderer";

export default function Contact(){
   const [selectedForm, setForm] = useState("");
   const extendMappings = {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
      };
    const getForm = async () => {
        
        const resp = await fetch('/content/forms/af/contactus/jcr:content/guideContainer.model.json');
        let formJSON = await resp.json();
        console.log(formJSON);
        setForm(formJSON);
      }
      useEffect( ()=>{
        getForm();
        

    },[]);
    return(
        
        <div>
            <h1>Get in touch with us!!!!</h1>
            <AdaptiveForm mappings={extendMappings} formJson={selectedForm} />
      
          
        </div>
    )
}
```

De bovenstaande code gebruikt native HTML-componenten die zijn toegewezen aan de componenten die in het adaptieve formulier worden gebruikt. We wijzen bijvoorbeeld de adaptieve formuliercomponent voor tekstinvoer toe aan de component TextField. De native componenten die in het artikel worden gebruikt [kan hier worden gedownload](./assets/native-components.zip)

## Volgende stappen

[Een formulier selecteren in de vervolgkeuzelijst](./select-form-from-drop-down-list.md)