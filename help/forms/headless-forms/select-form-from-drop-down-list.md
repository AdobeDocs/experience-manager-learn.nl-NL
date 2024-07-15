---
title: Een formulier selecteren in een lijst met beschikbare formulieren
description: De API voor lijsten gebruiken om de vervolgkeuzelijst te vullen
feature: Adaptive Forms
version: 6.5
jira: KT-13346
topic: Development
role: User
level: Intermediate
exl-id: 49b6a172-8c96-4fc6-8d31-c2109f65faac
duration: 88
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 0%

---

# Selecteer een formulier dat u wilt invullen in een vervolgkeuzelijst

Vervolgkeuzelijsten bieden een compacte en geordende manier om een lijst met opties weer te geven aan gebruikers. De punten in de drop-down lijst zullen met de resultaten van [ lijsten API ](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms) worden bevolkt

![ kaart-mening ](./assets/forms-drop-down.png)

## Vervolgkeuzelijst

De volgende code is gebruikt om de vervolgkeuzelijst te vullen met de resultaten van de API-aanroep van listforms. Op basis van de gebruikersselectie wordt het aangepaste formulier weergegeven zodat de gebruiker het kan invullen en verzenden. [ Materiële componenten UI ](https://mui.com/) zijn gebruikt in het creëren van deze interface

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

const[formID, setFormID] = useState('');
const[afForms,SetOptions] = useState([]);
const [selectedForm, setForm] = useState('');
const HandleChange = (event) =>
     {
        console.log("The form id is "+event.target.value) 
    
        setFormID(event.target.value)
        
     };
const getForm = async () =>
     {
        
        console.log("The formID in getForm"+ formID);
        const resp = await fetch(`/adobe/forms/af/${formID}`);
        let formJSON = await resp.json();
        console.log(formJSON.afModelDefinition);
        setForm(formJSON.afModelDefinition);
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
        

    },[formID]);

  return (
    <Box sx={{ minWidth: 120 }}>
      <FormControl fullWidth>
        <InputLabel id="demo-simple-select-label">Please select the form</InputLabel>
        <Select
          labelId="demo-simple-select-label"
          id="demo-simple-select"
          value={formID}
          label="Please select a form"
          onChange={HandleChange}
          
        >
       {afForms.map((afForm,index) => (
    
        
          <MenuItem  key={index} value={afForm.id}>{afForm.title}</MenuItem>
        ))}
        
       
        </Select>
      </FormControl>
      <div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
    </Box>
    

  );
  

}
```

De volgende twee API-aanroepen zijn gebruikt bij het maken van deze gebruikersinterface

* [ ListForm ](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms). De aanroep om de formulieren op te halen wordt slechts eenmaal gedaan wanneer de component wordt gegenereerd. De resultaten van de API-aanroep worden opgeslagen in de variabele afForms.
In de bovenstaande code doorlopen we de afForms met behulp van de kaartfunctie en voor elk item in de afForms-array wordt een MenuItem-component gemaakt en toegevoegd aan de Select-component.

* De Vorm van de vangst - A krijgt vraag wordt gemaakt aan [ getForm ](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/Get-Form-Definition), waar identiteitskaart identiteitskaart identiteitskaart van de geselecteerde adaptieve vorm door de gebruiker in de drop-down lijst is. Het resultaat van deze aanroep van GET wordt opgeslagen in selectedForm.

```
const resp = await fetch(`/adobe/forms/af/${formID}`);
let formJSON = await resp.json();
console.log(formJSON.afModelDefinition);
setForm(formJSON.afModelDefinition);
```

* Het geselecteerde formulier weergeven. De volgende code is gebruikt om het geselecteerde formulier weer te geven. Het element AdaptiveForm is opgenomen in het npm-pakket aemforms/af-response-renderer en verwacht de toewijzingen en formJson als eigenschappen

```
<div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
```

## Volgende stappen

[De formulieren weergeven in de kaartindeling](./display-forms-card-view.md)
