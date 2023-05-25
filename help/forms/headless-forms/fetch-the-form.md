---
title: Buscar o JSON do formulário adaptável para incorporar
description: Use a API para buscar o json do formulário adaptável
feature: Adaptive Forms
version: 6.5
kt: 13285
topic: Development
role: User
level: Intermediate
source-git-commit: 6aa3dff44a7e6f1f8ac896e30319958d84ecf57f
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 1%

---


# Buscar o JSON do formulário

Faça logon na instância de autor do AEM Forms e crie um novo adaptável usando o **Em branco com Componentes principais** modelo. Publique o formulário na instância de publicação.

Para incorporar o formulário, primeiro buscamos o json do formulário adaptável fazendo uma chamada get contra nosso servidor de publicação.

O trecho de código a seguir busca o json do formulário adaptável chamado **contactus**

```javascript
const getForm = async () => {
        const resp = await fetch('/content/forms/af/contactus/jcr:content/guideContainer.model.json');
        let formJSON = await resp.json();
        console.log(formJSON);
        setForm(formJSON);
      }
```

O código completo do componente da função Contato é fornecido abaixo

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

O código acima usa componentes html nativos que são mapeados para os componentes usados no formulário adaptável. Por exemplo, estamos mapeando o componente de formulário adaptável de entrada de texto para o componente TextField. Os componentes nativos usados no artigo [pode ser baixado aqui](./assets/native-components.zip)

## Próximas etapas

[Selecione um formulário na lista suspensa](./select-form-from-drop-down-list.md)