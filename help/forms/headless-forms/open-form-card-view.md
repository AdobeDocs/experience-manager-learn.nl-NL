---
title: Klik op de kaart om het formulier weer te geven
description: Boor neer op formulier van kaartmening
feature: Adaptive Forms
version: 6.5
kt: 13372
topic: Development
role: User
level: Intermediate
source-git-commit: 3bbf80d5c301953b3a34ef8256702ac7445c40da
workflow-type: tm+mt
source-wordcount: '93'
ht-degree: 0%

---

# Het formulier weergeven op de kaartklik

De volgende code is gebruikt om het formulier weer te geven wanneer de gebruiker op een kaart klikt. Het pad van het formulier dat moet worden weergegeven, wordt met de functie useParams uit de URL geëxtraheerd.

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Panel from './components/Panel';
import { useState,useEffect } from "react";
import {Link, useParams} from 'react-router-dom';
import { AdaptiveForm } from "@aemforms/af-react-renderer";
export default function DisplayForm()
{
   const [selectedForm, setForm] = useState("");
   const params = useParams();
   const pathParam = params["*"];
   const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };
    
    // Add the leading '/' back on 
   const path = '/' + pathParam;
   const getAFForm = async () =>
    {
           
        const resp = await fetch(`${path}/jcr:content/guideContainer.model.json`);
        let formJSON = await resp.json();
        console.log("The contact form json is "+formJSON);
        setForm(formJSON)
    }
    
    useEffect( ()=>{
        getAFForm()
        

    },[]);
    return(
       <div>
           <AdaptiveForm mappings={extendMappings} formJson={selectedForm}/>
        </div>
    )
}
```

In de bovenstaande code wordt het pad van het formulier dat moet worden gegenereerd, geëxtraheerd van de URL en gebruikt in de aanroep om de JSON van het formulier op te halen. De opgehaalde JSON bevindt zich vervolgens in de volgende code

```javascript
        <div>
           <AdaptiveForm mappings={extendMappings} formJson={selectedForm}/>
        </div>
```
