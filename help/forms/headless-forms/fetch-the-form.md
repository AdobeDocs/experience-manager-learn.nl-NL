---
title: De JSON ophalen van het adaptieve formulier dat moet worden ingesloten
description: Gebruik de API om de zoon van het adaptieve formulier op te halen
feature: Adaptive Forms
version: Experience Manager 6.5
jira: KT-13285
topic: Development
role: User
level: Intermediate
exl-id: ee534724-54ea-48e1-8c92-de1c56a928d4
duration: 50
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---

# De JSON van het formulier ophalen

Login aan uw de auteurinstantie van AEM Forms en creeer een nieuwe adaptief gebruikend **Leeg met het malplaatje van de Componenten van de Kern**. Publiceer het formulier naar uw publicatie-exemplaar.

Als u het formulier wilt insluiten, haalt u eerst de json van het adaptieve formulier op door een get call uit te voeren met onze publicatieserver.

Het volgende codefragment haalt de json van de adaptieve vorm genoemd **contactus**

```javascript
const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/L2NvbnRlbnQvZm9ybXMvYWYvZmlyc3RoZWFkbGVzcw==');
        // Get the form id manually using the listform api
        let formJSON = await resp.json();
        console.log("The contact form json is "+formJSON);
        setForm(formJSON.afModelDefinition)
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
        
        const resp = await fetch('/adobe/forms/af/dor/L2NvbnRlbnQvZm9ybXMvYWYvcmlzaGk=');
        let formJSON = await resp.json();
        setForm(formJSON.afModelDefinition)
      
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

De bovenstaande code gebruikt native HTML-componenten die zijn toegewezen aan de componenten die in het adaptieve formulier worden gebruikt. We wijzen bijvoorbeeld de adaptieve formuliercomponent voor tekstinvoer toe aan de component TextField. De inheemse componenten die in het artikel [ worden gebruikt kunnen van hier worden gedownload ](./assets/native-components.zip)

## Volgende stappen

[Een formulier selecteren in de vervolgkeuzelijst](./select-form-from-drop-down-list.md)
