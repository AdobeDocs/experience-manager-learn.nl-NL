---
title: De opgehaalde formulieren weergeven in de kaartweergave
description: De API voor lijsten gebruiken om de formulieren weer te geven
feature: Adaptive Forms
version: 6.5
kt: 13311
topic: Development
role: User
level: Intermediate
source-git-commit: 6aa3dff44a7e6f1f8ac896e30319958d84ecf57f
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 0%

---


# De formulieren ophalen en weergeven in kaartformaat

De indeling van de kaartweergave is een ontwerppatroon dat informatie of gegevens in de vorm van kaarten presenteert. Elke kaart vertegenwoordigt een afzonderlijk stuk van inhoud of gegevensingang en typisch bestaat uit een visueel verschillende container met specifieke elementen die daarin worden geschikt. In dit artikel gebruiken we de [listforms-API](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms) om de formulieren op te halen en de formulieren in kaart te brengen, zoals hieronder wordt weergegeven

![kaartweergave](./assets/card-view-forms.png)

## Kaartsjabloon

De volgende code is gebruikt om de kaartsjabloon te ontwerpen. De kaartsjabloon toont de titel en beschrijving van het adaptieve formulier samen met het Adobe-logo. [Materiële UI-componenten](https://mui.com/) zijn gebruikt bij het maken van deze lay-out.

```javascript
import Paper from "@mui/material/Paper";
import Grid from "@mui/material/Grid";
import Container from "@mui/material/Container";
import { Typography } from "@mui/material";
import { Box } from "@mui/system";
const FormCard =({headlessForm}) => {
    return (
              <Grid item xs={3}>
                <Paper elevation={3}>
                    <img src="/content/dam/formsanddocuments/registrationform/jcr:content/renditions/cq5dam.thumbnail.48.48.png" className="img"/>
                    <Box padding={3}>
                    <Typography variant="subtititle2" component="h2">
                        {headlessForm.title}
                    
                    </Typography>
                    <Typography variant="subtititle3" component="h4">
                        {headlessForm.description}
                    
                    </Typography>
                    </Box>
                </Paper>
                </Grid>
          


    );
    

};
export default FormCard;
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
