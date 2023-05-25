---
title: Een formulier selecteren in een lijst met beschikbare formulieren
description: De API voor lijsten gebruiken om de vervolgkeuzelijst te vullen
feature: Adaptive Forms
version: 6.5
kt: 13346
topic: Development
role: User
level: Intermediate
source-git-commit: 6aa3dff44a7e6f1f8ac896e30319958d84ecf57f
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 0%

---


# Selecteer een formulier dat u wilt invullen in een vervolgkeuzelijst

Vervolgkeuzelijsten bieden een compacte en geordende manier om een lijst met opties weer te geven aan gebruikers. De items in de vervolgkeuzelijst worden gevuld met de resultaten van [listforms-API](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms)

![kaartweergave](./assets/forms-drop-down.png)

## Vervolgkeuzelijst

De volgende code is gebruikt om de vervolgkeuzelijst te vullen met de resultaten van de API-aanroep van listforms. Op basis van de gebruikersselectie wordt het aangepaste formulier weergegeven zodat de gebruiker het kan invullen en verzenden. [Materiële UI-componenten](https://mui.com/) zijn gebruikt bij het maken van deze interface

```javascript
import * as React from 'react';
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Box from '@mui/material/Box';
import InputLabel from '@mui/material/InputLabel';
import MenuItem from '@mui/material/MenuItem';
import FormControl from '@mui/material/FormControl';
import Select, { SelectChangeEvent } from '@mui/material/Select';
import { AdaptiveForm } from "@aemforms/af-react-renderer";

import { useState,useEffect } from "react";
export default function SelectFormFromDropDownList()
 {
    const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };

const[formPath, setFormPath] = useState('');
const[afForms,SetOptions] = useState([]);
const [selectedForm, setForm] = useState('');
const HandleChange = (event) =>
     {
        console.log("The path is "+event.target.value) 
    
        setFormPath(event.target.value)
        console.log("The formPath"+ formPath);
     };
const getForm = async () =>
     {
        const resp = await fetch(`${formPath}/jcr:content/guideContainer.model.json`);
        let formJSON = await resp.json();
        console.log(formJSON);
        setForm(formJSON);
     }
const getAFForms =async()=>
     {
        const response = await fetch("/adobe/forms/af/listforms")
        //let myresp = await response.status;
        let myForms = await response.json();
        console.log("Got response"+myForms.items[0].title);
        console.log(myForms.items)
        
        //setFormID('test');
        SetOptions(myForms.items)

        
     }
     useEffect( ()=>{
        getAFForms()
        

    },[]);
    useEffect( ()=>{
        getForm()
        

    },[formPath]);

  return (
    <Box sx={{ minWidth: 120 }}>
      <FormControl fullWidth>
        <InputLabel id="demo-simple-select-label">Please select the form</InputLabel>
        <Select
          labelId="demo-simple-select-label"
          id="demo-simple-select"
          value={formPath}
          label="Please select a form"
          onChange={HandleChange}
          
        >
       {afForms.map((afForm,index) => (
    
        
          <MenuItem  key={index} value={afForm.path}>{afForm.title}</MenuItem>
        ))}
        
       
        </Select>
      </FormControl>
      <div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
    </Box>
    

  );
  

}
```

De volgende twee API-aanroepen zijn gebruikt bij het maken van deze gebruikersinterface

* [ListForm](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms). De aanroep om de formulieren op te halen wordt slechts eenmaal gedaan wanneer de component wordt gegenereerd. De resultaten van de API-aanroep worden opgeslagen in de variabele afForms.
In de bovenstaande code doorlopen we de afForms met behulp van de kaartfunctie en voor elk item in de afForms-array wordt een MenuItem-component gemaakt en toegevoegd aan de Select-component.

* Formulier zoeken - Er wordt een aanroep naar het volgende eindpunt gemaakt, waarbij formPath het pad is naar het geselecteerde aangepaste formulier dat de gebruiker in de vervolgkeuzelijst heeft gekozen. Het resultaat van deze aanroep van GET wordt opgeslagen in selectedForm.

```
${formPath}/jcr:content/guideContainer.model.json`
```

* Het geselecteerde formulier weergeven. De volgende code is gebruikt om het geselecteerde formulier weer te geven. Het element AdaptiveForm is opgenomen in het npm-pakket aemforms/af-response-renderer en verwacht de toewijzingen en formJson als eigenschappen

```
<div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
```

## Volgende stappen

[De formulieren weergeven in de kaartindeling](./display-forms-card-view.md)



