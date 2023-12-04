---
title: De opgehaalde formulieren weergeven in de kaartweergave
description: De API voor lijsten gebruiken om de formulieren weer te geven
feature: Adaptive Forms
version: 6.5
jira: KT-13311
topic: Development
role: User
level: Intermediate
exl-id: c01ad68e-23c9-4564-8e3e-1924af34a493
duration: 134
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---

# De formulieren ophalen en weergeven in kaartformaat

De indeling van de kaartweergave is een ontwerppatroon dat informatie of gegevens in de vorm van kaarten presenteert. Elke kaart vertegenwoordigt een afzonderlijk stuk van inhoud of gegevensingang en typisch bestaat uit een visueel verschillende container met specifieke elementen die daarin worden geschikt.
De klikbare kaarten in React zijn interactieve componenten die op kaarten of tegels lijken en door de gebruiker kunnen worden geklikt of worden getikt. Wanneer een gebruiker op een klikbare kaart klikt of tikt, wordt een opgegeven handeling of gedrag geactiveerd, zoals navigeren naar een andere pagina, een modaal openen of de gebruikersinterface bijwerken.

In dit artikel gebruiken we de [listforms-API](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms) om de formulieren op te halen en de formulieren weer te geven in de vorm van een kaart en het aangepaste formulier te openen op de gebeurtenis click.

![kaartweergave](./assets/card-view-forms.png)

## Kaartsjabloon

De volgende code is gebruikt om de kaartsjabloon te ontwerpen. De kaartsjabloon toont de titel en beschrijving van het adaptieve formulier samen met het logo van de Adobe. [Materiële UI-componenten](https://mui.com/) zijn gebruikt bij het maken van deze lay-out.



```javascript
import Container from "@mui/material/Container";
import Form from './Form';
import PlainText from './plainText'
import TextField from './TextField'
import Button from './Button';
import { AdaptiveForm } from "@aemforms/af-react-renderer";

import { CardActionArea, Typography } from "@mui/material";
import { Box } from "@mui/system";
import { useState,useEffect } from "react";
import DisplayForm from "../DisplayForm";
import { Link } from "react-router-dom";
export default function FormCard({headlessForm}) {
const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };
   
    return (
        
            <Grid item xs={3}>
                <Paper elevation={3}>
                    <img src="/content/dam/formsanddocuments/registrationform/jcr:content/renditions/cq5dam.thumbnail.48.48.png" className="img"/>
                    <Box padding={3}>
                        <Link style={{ textDecoration: 'none' }} to={`/displayForm${headlessForm.id}`}>
                            <Typography variant="subtititle2" component="h2">
                                {headlessForm.title}
                            </Typography>
                            <Typography variant="subtititle3" component="h4">
                                {headlessForm.description}
                            </Typography>
                        </Link>
                
                    </Box>
                </Paper>
            </Grid>
    );
    

};
```

De volgende route werd bepaald in Main.js om aan DisplayForm.js te navigeren

```javascript
    <Route path="/displayForm/:formID" element={<DisplayForm/>} exact/>
```

## De formulieren ophalen

De API voor lijsten is gebruikt om de formulieren op te halen van de AEM. De API retourneert een array met JSON-objecten, elk JSON-object dat een formulier vertegenwoordigt.

```javascript
import { useState,useEffect } from "react";
import React, { Component } from "react";
import FormCard from "./components/FormCard";
import Grid from "@mui/material/Grid";
import Paper from "@mui/material/Paper";
import Container from "@mui/material/Container";
 
export default function ListForm(){
    const [fetchedForms,SetHeadlessForms] = useState([])
    const getForms=async()=>{
        const response = fetch("/adobe/forms/af/listforms")
        let headlessForms = await (await response).json();
        console.log(headlessForms.items);
        SetHeadlessForms(headlessForms.items);
    }
    useEffect( ()=>{
        getForms()
        

    },[]);
    return(
        <div>
             <div>
                <Container>
                   <Grid container spacing={3}>
                       {
                            fetchedForms.map( (afForm,index) =>
                                <FormCard headlessForm={afForm} key={index}/>
                         
                            )
                        }
                    </Grid>
                </Container>
             </div>

        </div>
    )
}
```

In de bovenstaande code doorlopen we de fetchedForms met behulp van de kaartfunctie en voor elk item in de fetchedForms-array wordt een FormCard-component gemaakt en toegevoegd aan de Grid-container. U kunt nu naar wens de component ListForm in uw Reactie-app gebruiken.

## Volgende stappen

[Het adaptieve formulier weergeven wanneer de gebruiker op een kaart klikt](./open-form-card-view.md)
