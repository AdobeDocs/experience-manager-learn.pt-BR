---
title: Exibir os formulários buscados na exibição de cartão
description: Use a API listforms para exibir os formulários
feature: Adaptive Forms
version: 6.5
kt: 13311
topic: Development
role: User
level: Intermediate
exl-id: 7316ca02-be57-4ecf-b162-43a736b992b3
source-git-commit: 529e98269a08431152686202a8a2890712b9c835
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---

# Buscar e exibir os formulários no formato de cartão

O formato de exibição de cartão é um padrão de design que apresenta informações ou dados na forma de cartões. Cada cartão representa uma parte distinta do conteúdo ou da entrada de dados e geralmente consiste em um contêiner visualmente distinto com elementos específicos organizados dentro dele.
Cartões clicáveis no React são componentes interativos que se assemelham a cartões ou blocos e podem ser clicados ou tocados pelo usuário. Quando um usuário clica ou toca em um cartão clicável, ele aciona uma ação ou comportamento especificado, como navegar para outra página, abrir um modal ou atualizar a interface do usuário.

Neste artigo, usaremos o [API listforms](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms) para buscar os formulários e exibir os formulários no formato de cartão e abrir o formulário adaptável no evento de clique.

![exibição de cartão](./assets/card-view-forms.png)

## Modelo de cartão

O código a seguir foi usado para criar o modelo de cartão. O modelo de cartão está exibindo o título e a descrição do formulário adaptável, juntamente com o logotipo do Adobe. [Componentes da interface do usuário de material](https://mui.com/) foram usadas na criação deste layout.



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

A rota a seguir foi definida no Main.js para navegar até DisplayForm.js

```javascript
    <Route path="/displayForm/:formID" element={<DisplayForm/>} exact/>
```

## Busque os formulários

A API listforms foi usada para buscar os formulários do servidor AEM. A API retorna uma matriz de objetos JSON, cada objeto JSON representando um formulário.

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

No código acima, iteramos por meio do fetchedForms usando a função map e, para cada item na matriz fetchedForms, um componente FormCard é criado e adicionado ao contêiner Grade. Agora você pode usar o componente ListForm no aplicativo React de acordo com suas necessidades.

## Próximas etapas

[Exibir o formulário adaptável quando o usuário clicar em um cartão](./open-form-card-view.md)