---
title: Selecionar um formulário de uma lista de formulários disponíveis
description: Usar a API listforms para preencher a lista suspensa
feature: Adaptive Forms
version: 6.5
kt: 13346
topic: Development
role: User
level: Intermediate
source-git-commit: 6aa3dff44a7e6f1f8ac896e30319958d84ecf57f
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---


# Selecione um formulário para preencher em uma lista suspensa

As listas suspensas fornecem uma maneira compacta e organizada de apresentar uma lista de opções aos usuários. Os itens na lista suspensa serão preenchidos com os resultados de [API listforms](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms)

![exibição de cartão](./assets/forms-drop-down.png)

## Lista suspensa

O código a seguir foi usado para preencher a lista suspensa com os resultados da chamada à API listforms. Com base na seleção do usuário, o formulário adaptável é exibido para o usuário preencher e enviar. [Componentes da interface do usuário de material](https://mui.com/) foram usados na criação desta interface

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

As duas chamadas de API a seguir foram usadas na criação desta interface de usuário

* [FormulárioLista](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms). A chamada para buscar os formulários é feita apenas uma vez quando o componente é renderizado. Os resultados da chamada da API são armazenados na variável afForms.
No código acima, iteramos por meio do afForms usando a função map e, para cada item na matriz afForms, um componente MenuItem é criado e adicionado ao componente Selecionar.

* Buscar formulário - Uma chamada get é feita para o seguinte endpoint, onde formPath é o caminho para o formulário adaptável selecionado pelo usuário na lista suspensa. GET O resultado desta chamada é armazenado no seletedForm.

```
${formPath}/jcr:content/guideContainer.model.json`
```

* Exibir o formulário selecionado. O código a seguir foi usado para exibir o formulário selecionado. O elemento AdaptiveForm é fornecido no pacote npm aemforms/af-response-renderer e espera os mapeamentos e o formJson como suas propriedades

```
<div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
```

## Próximas etapas

[Exibir os formulários no layout do cartão](./display-forms-card-view.md)



